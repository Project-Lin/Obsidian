---
up: "[[專案/獨立遊戲之路/CSharp/C编程-第三季-坦克大战-宇宙最简单2021最新版/第三季-坦克大战-宇宙最简单2021最新版|第三季-坦克大战-宇宙最简单2021最新版]]"
繼承:
---
- [1]                                       

```csharp file:GameObject
using System;
using System.Collections.Generic;
using System.Drawing;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace _04_坦克大戰_正式
{
    internal abstract class GameObject
    {
        //priveate int x;
        //public x {get{return x;} set{x=value;}}
        //屬性
        public int X { get; set; }
        public int Y { get; set; }

        public int Width { get; set; }
        public int Height { get; set; }

        //protected 抽象方法不能設定為private 
        //但可以設定為protected 子類別就可以繼承 同時不被外界調用
        protected abstract Image GetImage();

        


        public virtual void DrawSelf()
        {
            Graphics g = GameFramework.g;
            g.DrawImage(GetImage(), this.X, this.Y);
        }

        public virtual void Update()
        {
            DrawSelf();
        }

        public Rectangle GetRectangle()
        {
            Rectangle rec = new Rectangle(this.X, this.Y, 
            	this.Width, this.Height);
            
            return rec;
        }


    }
}
```