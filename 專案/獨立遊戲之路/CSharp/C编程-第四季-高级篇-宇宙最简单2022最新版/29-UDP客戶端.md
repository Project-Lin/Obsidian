---
tags:
  - csharp
  - UDP
up: "[[專案/獨立遊戲之路/CSharpRoad|CSharpRoad]]"
date: 2024-03-30
課程: "[[C编程-第四季-高级篇-宇宙最简单2022最新版|C编程-第四季-高级篇-宇宙最简单2022最新版]]"
---

```csharp file:Program
using System.Net;
using System.Net.Sockets;
using System.Text;

namespace _29_UDP客戶端
{
    internal class Program
    {
        static void Main(string[] args)
        {
            Socket udpClint = new Socket(
            AddressFamily.InterNetwork, 
            SocketType.Dgram, 
            ProtocolType.Udp);

            IPAddress ip = new IPAddress(new byte[] { 192, 168, 8, 155 });

            IPEndPoint point = new IPEndPoint(ip, 7788);

            string message = "Hello World";
            udpClint.SendTo(Encoding.UTF8.GetBytes(message), point);

            udpClint.Close();
        }
    }
}

```

