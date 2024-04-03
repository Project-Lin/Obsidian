---
up: "[[專案/獨立遊戲之路/CSharp/C编程-第三季-坦克大战-宇宙最简单2021最新版/第三季-坦克大战-宇宙最简单2021最新版|第三季-坦克大战-宇宙最简单2021最新版]]"
繼承: "[[專案/獨立遊戲之路/CSharp/C编程-第三季-坦克大战-宇宙最简单2021最新版/專案檔/GameObject|GameObject]]"
---
- [1]                                       

```csharp file:Movething
using System;
using System.Collections.Generic;
using System.Drawing;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace _04_坦克大戰_正式
{
    //使用枚舉類型定義方向
    enum Direction
    {
        Up,
        Down,
        Left,
        Right
    }
    //可移動物件
    internal class Movething:GameObject
    {
        private Object _lock = new Object();
        public Bitmap BitmapUp { get; set; }
        public Bitmap BitmapDown { get; set; }
        public Bitmap BitmapLeft { get; set; }
        public Bitmap BitmapRight { get; set; }

        public int Speed { get; set; }


        private Direction dir;
        public Direction direction 
        { get 
            { 
                return dir; 
            } 
            set 
            { 
                dir = value; 
                Bitmap bmp = null;

                switch (dir)
                {
                    case Direction.Up: 
                        bmp=BitmapUp;
                        break;

                    case Direction.Down:
                        bmp = BitmapDown;
                        break;

                    case Direction.Left:
                        bmp = BitmapLeft;
                        break;

                    case Direction.Right:
                        bmp = BitmapRight;
                        break;


                    
                }
                lock (_lock)
                {
                    this.Width = bmp.Width;
                    this.Height = bmp.Height;
                }

            }
        }

        //取得圖片方法
        protected override Image GetImage()
        {
            //根據方向返回不同的圖片
            Bitmap bitmap = null;
            switch (direction)
            {
                case Direction.Up:
                    bitmap = BitmapUp;
                    break;
                case Direction.Down:
                    bitmap = BitmapDown;
                    break;
                case Direction.Left:
                    bitmap = BitmapLeft;
                    break;
                case Direction.Right:
                    bitmap = BitmapRight;
                    break;
                default:
                    bitmap = BitmapUp;
                    break;
            }
            //使圖片背景透明
            bitmap.MakeTransparent(Color.Black);
            return bitmap;
        }


        public override void DrawSelf()
        {
            lock (_lock)
            {
                base.DrawSelf();
            }
            
        }

    }
}
```