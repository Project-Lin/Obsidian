---
up: "[[專案/獨立遊戲之路/CSharp/C编程-第三季-坦克大战-宇宙最简单2021最新版/第三季-坦克大战-宇宙最简单2021最新版|第三季-坦克大战-宇宙最简单2021最新版]]"
繼承: "[[專案/獨立遊戲之路/CSharp/C编程-第三季-坦克大战-宇宙最简单2021最新版/專案檔/GameObject|GameObject]]"
---
- [1]                                       

```csharp file:NotMovething
using System;
using System.Collections.Generic;
using System.Drawing;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace _04_坦克大戰_正式
{
    //不可移動物件
    internal class NotMovething:GameObject
    {
        public NotMovething(int x,int y,Image img)
        {
            this.X = x;
            this.Y = y;
            this.Img = img;
        }

        private Image img;
        public Image Img 
        { 
            get 
            { 
                return img; 
            } 
            set 
            { 
                img = value;
                Width = img.Width;
                Height = img.Height; 
            } 
        }

        protected override Image GetImage()
        {
            return Img;
        }

    }
}
```