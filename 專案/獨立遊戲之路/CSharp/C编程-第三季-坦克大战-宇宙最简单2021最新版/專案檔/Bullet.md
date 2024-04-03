---
up: "[[專案/獨立遊戲之路/CSharp/C编程-第三季-坦克大战-宇宙最简单2021最新版/第三季-坦克大战-宇宙最简单2021最新版|第三季-坦克大战-宇宙最简单2021最新版]]"
繼承: "[[專案/獨立遊戲之路/CSharp/C编程-第三季-坦克大战-宇宙最简单2021最新版/專案檔/Movething|Movething]]"
---
- [1]                                       


```csharp file:Bullet
using _04_坦克大戰_正式.Properties;
using System;
using System.Collections.Generic;
using System.Drawing;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace _04_坦克大戰_正式
{
    enum BulletType
    {
        My,
        Enemy
    }
    internal class Bullet : Movething
    {
        public BulletType Type { get; set; }
        public bool IsDestory { get; set; }


        public override void Update()
        {
            MoveCheck();
            Move();
            base.Update();
        }
        

        public Bullet(int x, int y, int speed, Direction direction, BulletType bulletType)
        {
            IsDestory = false;
            this.X = x;
            this.Y = y;
            this.Speed = speed;

            this.BitmapUp = Resources.BulletUp;
            this.BitmapDown = Resources.BulletDown;
            this.BitmapLeft = Resources.BulletLeft;
            this.BitmapRight = Resources.BulletRight;
            this.direction = direction;
            this.Type = bulletType;

            this.X -= Width / 2;
            this.Y -= Height / 2;
        }

        private void Move()
        {

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

        private void MoveCheck()
        {
            #region 判斷是否超出邊界
            if (direction == Direction.Up && Y+3<= 0)
            {
                IsDestory = true;
                return;
            }
            else if (direction == Direction.Down && Y -3 >= 450)
            {
                IsDestory = true;
                return;
            }
            else if (direction == Direction.Left && X -3 <= 0)
            {
                IsDestory = true;
                return;
            }
            else if (direction == Direction.Right && X +3 >= 450)
            {
                IsDestory = true;
                return;
            }
            #endregion

            //判斷有沒有看其他元素發生碰撞
            Rectangle rec = this.GetRectangle();

            //定位碰撞體的起始點 跟長寬
            rec.X+=Width/2-3;
            rec.Y+=Height/2-3;
            rec.Width = 6;
            rec.Height = 6;

            //碰撞牆 鐵牆 坦克

            NotMovething wall = null;
            if ((wall=GameObjectManager.IsCollidedWall(rec)) != null)
            {
                IsDestory = true;
                GameObjectManager.CreateExplosion(rec.X-16, rec.Y-16);
                GameObjectManager.DestoryWall(wall);
                SoundManager.PlayStart(4);
                return;
            }

            if (GameObjectManager.IsCollidedSteel(rec) != null)
            {
                IsDestory = true;
                GameObjectManager.CreateExplosion(rec.X - 16, rec.Y - 16);
                SoundManager.PlayStart(4);
                return;
            }

            if (GameObjectManager.IsCollidedBoss(rec))
            {
                IsDestory = true;
                SoundManager.PlayStart(4);
                GameFramework.GameOver();
                return;
            }

            if (Type == BulletType.My&& GameObjectManager.IsCollidedEnemy(rec)!=null)
            {
                IsDestory = true;
                GameObjectManager.DestoryEnemyTank(GameObjectManager.IsCollidedEnemy(rec));
                GameObjectManager.CreateExplosion(rec.X - 16, rec.Y - 16);
                SoundManager.PlayStart(4);
                return;
            }
            if(Type == BulletType.Enemy && GameObjectManager.IsCollidedMyTank(rec) != null)
            {
                MyTank myTank = GameObjectManager.IsCollidedMyTank(rec);
                SoundManager.PlayStart(3);
                MyTank.TakeDamage(myTank);
                GameObjectManager.CreateExplosion(rec.X - 16, rec.Y - 16);
                return;
            }


        }

    }
}
```