---
tags:
  - csharp
  - "#UDP"
up: "[[專案/獨立遊戲之路/CSharpRoad|CSharpRoad]]"
date: 2024-03-30
課程: "[[C编程-第四季-高级篇-宇宙最简单2022最新版|C编程-第四季-高级篇-宇宙最简单2022最新版]]"
---

- [1] Udp 服務器不用建立連接設定接收數據的 IP 限制接收數據

- [1] Socket 參數 : AddressFamily. InterNetwork, SocketType. Dgram,  ProtocolType. Udp

- [1] 接收數據
UdpServer.ReceiveFrom ( Byte [ ] , ref endPoint )


```csharp file:Program
using System.Net;
using System.Net.Sockets;

namespace _28_UDP服務器
{
    internal class Program
    {
        static void Main(string[] args)
        {
            //udp服務器 不用建立連接 設定接收數據的IP限制 接收數據
            Socket udpServer = new Socket (
            AddressFamily.InterNetwork,
            SocketType.Dgram, 
            ProtocolType.Udp);

            IPAddress ip = new IPAddress(new byte[] { 192, 168, 8, 155 });
            
            IPEndPoint point = new IPEndPoint(ip, 7788);

            //綁定ip和端口
            udpServer.Bind(point);
            
            IPEndPoint iPEndPoint = new IPEndPoint(IPAddress.Any, 0);
            EndPoint endPoint = (EndPoint)iPEndPoint;
            byte[] data = new byte[1024];

            int count  =  udpServer.ReceiveFrom(data, ref endPoint);

            string message = Encoding.UTF8.GetString(data, 0, count);

            Console.WriteLine("接收到的消息:" + message);



        }
    }
}

```