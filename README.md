# java10
方法重写覆盖  OVerride
TCP网络通信编程
netstat指令
1.netstat -an 可以查看当前主机网络情况，包括端口监听情况和网络链接情况
2.netstat -an|more 可以分页显示
3.要求在dos控制台下执行
外部地址是客户端
内部地址一般是服务器
通过netstat -anb来查看外部地址来自哪里

TCP网络通讯不为人知的秘密
1.当客户端链接到服务端后，实际上客户端也是通过一个端口和服务端进行通讯的，这个端口是TCP/IP来分配的，是不确定的，是随机的。
2.示意图
3.程序验证+netstat


UDP网络通信编程  了解
1.类DatagramSocket 和DatagramPacket实现了基于Udp协议网络程序
2.UDP数据报通过数据报套接字 DatagramSocekt 发送和接收，系统不保证UDP数据报一定能够安全送到目的地，也不能确定什么时候可以抵达。
3.DatagramPacket 对象封装了UDP数据恶报，在数据恶报中包含了发送端的IP地址和端口号以及接收端的iP地址和端口号
4.UDP协议中每个数据报都给出了完整的地址信息，因此无序建立发送和接收方的连接


UDP说明：1.没有明确的服务端和客户端，演变成数据的发送端和接收端。
2.接收数据和发送数据时通过DatagramSocket对象完成
3.将数据封装到DatagramPacket 对象，发送

基本流程：
1.核心的两个类/对象 DatagramSocket与DatagramPacket
2.建立发送端，接收打算（没有服务端和客户端概念）
3.建立数据包
4.调用DatagramSocket的发送，接收方法
5.关闭DatagramSocket

案例
1.编写一个接收端A，和一个发送端B
2.接收端A在9999端口等待接收数据（receive）
3.发送端B向接收端A发送数据“hello，明天吃火锅~”
4.接收端A接收到发送端B发送的数据，回复“好的，明天见”
5.发送端接收 回复的数据，再退出

public class UDPreceiverA {
    public static void main(String[] args) throws IOException {
        //1.创建一个DatagramSocket对象，准备在9898端口接收数据
        DatagramSocket datagramSocket = new DatagramSocket(9999);
        //2.构建一个DatagramPacket对象，准备接收数据
        //在前面讲解UDP协议时，老师说过一个数据包最大64k
        byte[]buf = new byte[64*1024];
        DatagramPacket datagramPacket = new DatagramPacket(buf,buf.length);
        //3.调用接收方法,将通过网络传输的DatagramPacket对象
        //填充到 packet对象
        //老师提示：当有数据包发送到 本机的9898端口时，这个方法会接收到数据到Packet离去
        //如果没有数据包发送到本机的9898端口，就会阻塞等待
        System.out.println("接收端A等待接收数据。。。");
        datagramSocket.receive(datagramPacket);/////////////////////
        //4.可以把packet进行拆包，取出数据，并显示，

        int length = datagramPacket.getLength();//实际接收到的数据长度
        byte[] data = datagramPacket.getData();
        //接收字节数组
        String s = new String(data, 0, length);

        System.out.println(s);



        byte[]datw = "好的，明天见".getBytes();
        DatagramPacket datagramPacket1 = new DatagramPacket(datw, 0, datw.length,InetAddress.getByName("192.168.1.5"),9998);

        datagramSocket.send(datagramPacket1);
        System.out.println("A端退出");
        datagramSocket.close();



    }
}===========================


public class UDPSenderB {
    public static void main(String[] args) throws IOException {


        //创建DatagramSocket对象，准备发送和接收数据
        //准备在9797端口，接收数据
        DatagramSocket datagramSocket = new DatagramSocket(9998);
        //不要一样的端口，因为我们是在同一台电脑进行
        //将需要发送的数据，封装到DatagramPacket对象 data内容字节数组，data.length,主机（IP），端口
        byte[] bytes = "hello,明天去吃火锅".getBytes();
        DatagramPacket datagramPacket = new DatagramPacket(bytes,bytes.length, InetAddress.getByName("192.168.1.5"),9999);
        //Ip地址
        datagramSocket.send(datagramPacket);////////////////////

      //接收数据

        byte[] bytes1 = new byte[1024];
        DatagramPacket datagramPacket1 = new DatagramPacket(bytes1, 0, bytes1.length);
        datagramSocket.receive(datagramPacket1);

        int length = datagramPacket1.getLength();

        byte[] data = datagramPacket1.getData();
        String s = new String(data, 0, length);

        System.out.println(s);
        datagramSocket.close();


        System.out.println("B端退出");

    }
A：
}接收端A等待接收数据。。。
hello,明天去吃火锅
A端退出
B：好的，明天见
B端退出


本章作业：
1.使用字符流的方式，编写一个客户端程序和服务器端程序
2.客户端发送“name”，服务器端接收到后，返回 “我是nova”，nova是你自己的名字，
3.客户端发送“hobby”，服务器端接收到后，返回“编写java程序”
4.不是这两个问题，回复“你说啥呢”
