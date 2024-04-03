---
up: "[[專案/獨立遊戲之路/CSharp/C编程-第三季-坦克大战-宇宙最简单2021最新版/第三季-坦克大战-宇宙最简单2021最新版|第三季-坦克大战-宇宙最简单2021最新版]]"
繼承: "[[專案/獨立遊戲之路/CSharp/C编程-第三季-坦克大战-宇宙最简单2021最新版/專案檔/Movething|Movething]]"
---
- [1]                                       

```csharp file:MyTank
using _04_坦克大戰_正式.Properties;
using System;
using System.Collections.Generic;
using System.Drawing;
using System.Linq;
using System.Text;
using System.Threading;
using System.Threading.Tasks;
using System.Windows.Forms;

namespace _04_坦克大戰_正式
{
    
    internal class MyTank: Movething
    {
        public bool IsMove { get; set; }
        public int HP { get; set; }
        public MyTank(int x,int y,int speed)
        {
            IsMove = false;
            HP = 4;
            this.X = x;
            this.Y = y;
            this.Speed = speed;
            
            this.BitmapUp = Resources.MyTankUp;
            this.BitmapDown = Resources.MyTankDown;
            this.BitmapLeft = Resources.MyTankLeft;
            this.BitmapRight = Resources.MyTankRight;

            this.direction = Direction.Up;

        }

       

        public void KeyDown(KeyEventArgs args)
        {
            switch (args.KeyCode)
            {
                case Keys.W:
                    direction = Direction.Up;
                    IsMove = true;
                    break;
                case Keys.S:
                    direction = Direction.Down;
                    IsMove = true;
                    break;
                case Keys.A:
                    direction = Direction.Left;
                    IsMove = true;
                    break;
                case Keys.D:
                    direction = Direction.Right;
                    IsMove = true;
                    break;
                case Keys.Space:
                    //發射子彈
                    Attack();
                    break;
                default:
                    break;
            }
        }

        private void Attack()
        {
            SoundManager.PlayStart(5);
            int x = this.X;
            int y = this.Y;
            switch (direction)
            {
                case Direction.Up:
                    x+=Width/2;
                    break;
                case Direction.Down:
                    x += Width / 2;
                    y += Height;
                    break;
                case Direction.Left:
                    y += Height / 2;
                    break;
                case Direction.Right:
                    x += Width;
                    y += Height / 2;
                    break;
                default:
                    break;
            }
            GameObjectManager.CreatBullet(x, y, BulletType.My, this.direction);
            
        }

        public void KeyUp(KeyEventArgs args)
        {
            switch (args.KeyCode)
            {
                case Keys.W:
                    IsMove = false;
                    break;
                case Keys.S:
                    IsMove = false;
                    break;
                case Keys.A:
                    IsMove = false;
                    break;
                case Keys.D:
                    IsMove = false;
                    break;
                default:
                    break;
            }


        }

        public override void Update()
        {
            MoveCheck();
            Move();
            base.Update();
        }



        private void MoveCheck()
        {
            #region 判斷是否超出邊界
            if (direction == Direction.Up && Y - Speed <= 0)
            {
                IsMove = false;
                return;
            }
            else if (direction == Direction.Down && Y + Speed + Height >= 450)
            {
                IsMove = false;
                return;
            }
            else if (direction == Direction.Left && X - Speed <= 0)
            {
                IsMove = false;
                return;
            }
            else if (direction == Direction.Right && X + Speed + Width >= 450)
            {
                IsMove = false;
                return;
            }
            #endregion

            //判斷有沒有看其他元素發生碰撞

            //取得移動後的矩形
            Rectangle recNow = this.GetRectangle();
            //判斷未來的矩形
            switch (direction)
            {
                case Direction.Up:
                    recNow.Y -= Speed;
                    break;
                case Direction.Down:
                    recNow.Y += Speed;
                    break;
                case Direction.Left:
                    recNow.X -= Speed;
                    break;
                case Direction.Right:
                    recNow.X += Speed;
                    break;
                default:
                    break;
            }
            if (GameObjectManager.IsCollidedWall(recNow) != null)
            {
                IsMove = false;
                return;
            }

            if (GameObjectManager.IsCollidedSteel(recNow) != null)
            {
                IsMove = false;
                return;
            }

            if (GameObjectManager.IsCollidedBoss(recNow))
            {
                IsMove = false;
                return;
            }

        }

        private void Move()
        {
            if (IsMove == false)
            {
                return;
            }
            switch (direction)
            {
                case Direction.Up:
                    this.Y -= Speed;
                    break;
                case Direction.Down:
                    this.Y += Speed;
                    break;
                case Direction.Left:
                    this.X -= Speed;
                    break;
                case Direction.Right:
                    this.X += Speed;
                    break;
                default:
                    break;
            }
        }

        public static void TakeDamage(MyTank myTank)
        {
            myTank.HP--;
            if (myTank.HP <= 0)
            {
                myTank.X = 5 * 30;
                myTank.Y = 14 * 30;
                myTank.HP = 4;
            }
        }   
    }
}
```