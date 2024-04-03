---
tags:
  - csharp
  - TCP
  - UTF8
up: "[[專案/獨立遊戲之路/CSharpRoad|CSharpRoad]]"
date: 2024-03-30
課程: "[[C编程-第四季-高级篇-宇宙最简单2022最新版|C编程-第四季-高级篇-宇宙最简单2022最新版]]"
---

- [1] 發送數據 : TcpClint . Send ( )

- [1] 字串轉換成 Byte : Encoding. UTF 8. GetBytes ( )


```csharp file:Program
using System.Net;
using System.Net.Sockets;
using System.Text;

namespace _27_TCP客戶端
{
    internal class Program
    {
        static void Main(string[] args)
        {
            Socket tcpClint = new Socket(AddressFamily.InterNetwork, SocketType.Stream, ProtocolType.Tcp);

            //綁定IP和端口
            IPAddress ip = new IPAddress(new byte[] { 192, 168, 8, 155 });
            //Ip+Port
            IPEndPoint point = new IPEndPoint(ip, 7788);

            tcpClint.Connect(point);

            Console.WriteLine("連接成功");

            //發送數據tcpClint.Send
            string message = "Hello,Server";
            //數據只能是byte類型 (編碼)UTF8.GetBytes
            tcpClint.Send(Encoding.UTF8.GetBytes(message));

            tcpClint.Close();

        }
    }
}

```