---
tags:
  - csharp
  - "#TCP"
  - "#UTF8"
up: "[[專案/獨立遊戲之路/CSharpRoad|CSharpRoad]]"
date: 2024-03-30
課程: "[[C编程-第四季-高级篇-宇宙最简单2022最新版|C编程-第四季-高级篇-宇宙最简单2022最新版]]"
---
- [1] Socket 參數 (網路類型, 傳輸方式, 協議)

- [1] Encoding. UTF 8. GetString ( ) 
將 Byte 轉成字串

- [1] TcpServer.Bind ( )
服務器綁定 ip 和 port

- [1] tcpServer.Accept ( )
開啟服務器 等待接收

- [1] client.Receive ( )
接收數據



```csharp file:Program
using System.Net;
using System.Net.Sockets;
using System.Text;

namespace _26_TCP服務器端TcpServer
{
    internal class Program
    {
        static void Main(string[] args)
        {
            
            Socket tcpServer = new Socket(
            AddressFamily.InterNetwork, 
            SocketType.Stream, 
            ProtocolType.Tcp);

            //綁定IP和端口
            IPAddress ip = new IPAddress (new byte[] { 192,168,8,155 });
            //Ip+Port
            IPEndPoint point = new IPEndPoint(ip, 7788);

            tcpServer.Bind(point);

            //最大連接數
            tcpServer.Listen(100);

            Console.WriteLine("等待連接");
            Socket client = tcpServer.Accept();

            Console.WriteLine("一個客戶端連接");

            //接收數據 client.Receive
            byte[] data = new byte[1024];
            int count = client.Receive(data);

            //將接收到的數據轉換成字符串  UTF8.GetString
            string message = Encoding.UTF8.GetString(data, 0, count);
            Console.WriteLine("接收到的消息:" + message);
            

            client.Close();

            tcpServer.Close();
        }
    }
}

```