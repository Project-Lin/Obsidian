---
up: "[[專案/獨立遊戲之路/CSharp/C编程-第三季-坦克大战-宇宙最简单2021最新版/第三季-坦克大战-宇宙最简单2021最新版|第三季-坦克大战-宇宙最简单2021最新版]]"
繼承: "[[專案/獨立遊戲之路/CSharp/C编程-第三季-坦克大战-宇宙最简单2021最新版/專案檔/GameObject|GameObject]]"
---
- [1]                                       

```csharp file:Explosion
using _04_坦克大戰_正式.Properties;
using System;
using System.Collections.Generic;
using System.Drawing;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace _04_坦克大戰_正式
{
    internal class Explosion : GameObject
    {
        public bool IsDestory { get; set; }
        private int playSpeed = 2;
        private int playCount = -1;
        private int index = 0;
        private Bitmap[] bmpArry = new Bitmap[]
        {
            Resources.EXP1,
            Resources.EXP2,
            Resources.EXP3,
            Resources.EXP4,
            Resources.EXP5

        };

        public Explosion(int x, int y)
        {
            IsDestory = false;
            foreach(Bitmap item in bmpArry)
            {
                item.MakeTransparent(Color.Black);
            }

            this.X = x - Width / 2;
            this.Y = y - Height / 2;

        }
        protected override Image GetImage()
        {
            
            return bmpArry[index];
        }

        override public void Update()
        {
            if (playCount / playSpeed < 5)
            {
                playCount++;
                index = (playCount - 1) / playSpeed;
                base.Update();
            }
            else
            {
                IsDestory = true;
                base.Update();
                index = 0;
            }
            

            
        }



    }
}
```