---
up: "[[專案/獨立遊戲之路/CSharp/C编程-第三季-坦克大战-宇宙最简单2021最新版/第三季-坦克大战-宇宙最简单2021最新版|第三季-坦克大战-宇宙最简单2021最新版]]"
繼承:
---
- [1]                                       

```csharp From1.cs
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.Linq;
using System.Text;
using System.Threading;
using System.Threading.Tasks;
using System.Windows.Forms;

namespace _04_坦克大戰_正式
{
    public partial class Form1 : Form
    {
        private Thread t;
        private static Graphics windowG;
        private static Bitmap TempBmp;
        public Form1()
        {
            InitializeComponent();

            this.StartPosition = FormStartPosition.CenterScreen;
            windowG = this.CreateGraphics();

            TempBmp = new Bitmap(450,450);
            Graphics bmpG = Graphics.FromImage(TempBmp);
            GameFramework.g = bmpG;
            //創建一個新的線程(遊戲主線程)
            //寫法: Thread t = new Thread(方法名);
            t = new Thread(new ThreadStart(GameMainThread));
            //啟動線程
            t.Start();
            
        }
        private void GameMainThread()
        {
            GameFramework.Start();

            while (true)
            {
                //清空畫布 塗上指定顏色
                GameFramework.g.Clear(Color.Black);

                //設定成60FPS  
                GameFramework.Update();

                windowG.DrawImage(TempBmp, 0, 0);

                Thread.Sleep(1000/60);
            }
        }

        private void Form1_FormClosed(object sender, FormClosedEventArgs e)
        {
            t.Abort();
        }

        private void Form1_KeyDown(object sender, KeyEventArgs e)
        {
            GameObjectManager.KeyDown(e);
        }

        private void Form1_KeyUp(object sender, KeyEventArgs e)
        {
            GameObjectManager.KeyUp(e);
        }
    }
}
```