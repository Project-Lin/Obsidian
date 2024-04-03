---
up: "[[專案/獨立遊戲之路/CSharp/C编程-第三季-坦克大战-宇宙最简单2021最新版/第三季-坦克大战-宇宙最简单2021最新版|第三季-坦克大战-宇宙最简单2021最新版]]"
繼承:
---
- [1]                                       

```csharp file:GameObjectManager
using System;
using System.CodeDom.Compiler;
using System.Collections.Generic;
using System.Drawing;
using System.Linq;
using System.Security.AccessControl;
using System.Text;
using System.Threading.Tasks;
using System.Windows.Forms;
using _04_坦克大戰_正式.Properties;

namespace _04_坦克大戰_正式
{
    internal class GameObjectManager
    {
        private static List<NotMovething> wallList = 
        new List<NotMovething>();
        
        private static List<NotMovething> steelList = 
        new List<NotMovething>();
        
        private static List<EnemyTank> tankList = new List<EnemyTank>();
        private static List<Bullet> bulletList = new List<Bullet>();
        private static List<Explosion> explosionList = 
        new List<Explosion>();
        
        private static NotMovething Boss = new NotMovething(0, 0, 
        Resources.Boss);
        private static MyTank myTank ;
        private static Object _bulletLock = new Object();


        private static int enemyBornTime = 60;
        private static int enemyBornCount = 60;

        private static Point[] point = new Point[3];

        public static void Start()
        {
            point[0].X = 0; point[0].Y = 0;
            point[1].X = 7*30; point[1].Y = 0;
            point[2].X = 14*30; point[2].Y = 0;
        }
        public static void Update()
        {
            foreach (NotMovething wall in wallList)
            {
                wall.Update();
            }
            foreach (NotMovething steel in steelList)
            {
                steel.Update();
            }

            foreach (EnemyTank tank in tankList)
            {
                tank.Update();
            }

            
            CheckAndDestoryBullet();
            foreach (Bullet bullet in bulletList)
            {
                bullet.Update();
            }

            DestoryExplosion();
            foreach (Explosion exp in explosionList)
            {
                exp.Update();
            }



            Boss.Update();

            myTank.Update();

            EnemyBron();
        }

        public static void CreatBullet(int x,int y,
        	BulletType bulletType, Direction direction)
        {
           Bullet bullet = new Bullet(x, y, 5, direction, bulletType);
            
           bulletList.Add(bullet);
 
             
        }

        public static void CheckAndDestoryBullet()
        {
            List<Bullet> destoryList = new List<Bullet>();
 
           foreach (Bullet bullet in bulletList)
           {
               if (bullet.IsDestory)
               {
                   destoryList.Add(bullet);
               }


           }

           foreach (Bullet bullet in destoryList)
           {
                bulletList.Remove(bullet);
           }

            
        }

        public static void CreateExplosion(int x,int y)
        {
            Explosion explosion = new Explosion(x, y);
            explosionList.Add(explosion);
        }

        public static void DestoryExplosion()
        {
            List<Explosion> destoryList = new List<Explosion>();

            foreach (Explosion explosion in explosionList)
            {
                if (explosion.IsDestory)
                {
                    destoryList.Add(explosion);
                }
            }

            foreach (Explosion explosion in destoryList)
            {
                explosionList.Remove(explosion);
            }

        }
        private static void EnemyBron()
        {
            
            enemyBornCount++;
            if(enemyBornCount < enemyBornTime) return;

            SoundManager.PlayStart(2);
            //隨機生成一個0~2的數字
            Random r = new Random();
            int index = r.Next(0, 3);
            Point p = point[index];
            //隨機生成一個1~4的數字
            int enemytype = r.Next(1, 5);
            switch (enemytype)
            {
                case 1:
                    CreateEnemyTank1(p.X, p.Y);
                    break;
                case 2:
                    CreateEnemyTank2(p.X, p.Y);
                    break;
                case 3:
                    CreateEnemyTank3(p.X, p.Y);
                    break;
                case 4:
                    CreateEnemyTank4(p.X, p.Y);
                    break;
            }
            

            enemyBornCount = 0;
        }
        public static void DestoryEnemyTank(EnemyTank tank)
        {
            tankList.Remove(tank);
        }

        private static void CreateEnemyTank1(int x,int y)
        {
            EnemyTank enemyTank = new EnemyTank(x, y, 2,
            Resources.GrayDown,
            Resources.GrayUp,
            Resources.GrayRight,
            Resources.GrayLeft);
            
            tankList.Add(enemyTank);
        }

        private static void CreateEnemyTank2(int x, int y)
        {
            EnemyTank enemyTank = new EnemyTank(x, y, 2,
            Resources.GreenDown,
            Resources.GreenUp,
            Resources.GreenRight,
            Resources.GreenLeft);
            
            tankList.Add(enemyTank);
        }

        private static void CreateEnemyTank3(int x, int y)
        {
            EnemyTank enemyTank = new EnemyTank(x, y, 4,
             Resources.QuickDown,
             Resources.QuickUp, 
             Resources.QuickRight, 
             Resources.QuickLeft);
             
            tankList.Add(enemyTank);
        }

        private static void CreateEnemyTank4(int x, int y)
        {
            EnemyTank enemyTank = new EnemyTank(x, y, 1, 
            Resources.SlowDown, 
            Resources.SlowUp, 
            Resources.SlowRight, 
            Resources.SlowLeft);
            
            tankList.Add(enemyTank);
        }

        public static NotMovething IsCollidedWall(Rectangle rt)
        {
            foreach (NotMovething wall in wallList)
            {
                //1.取得每一個牆的矩形
                //2.判斷是否與傳入的矩形相交
                //3.如果相交就返回true
                if (wall.GetRectangle().IntersectsWith(rt))
                {
                    return wall;
                }

                
            }
            return null;
        }

        public static NotMovething IsCollidedSteel(Rectangle rt)
        {
            foreach (NotMovething wall in steelList)
            {
                //1.取得每一個牆的矩形
                //2.判斷是否與傳入的矩形相交
                //3.如果相交就返回true
                if (wall.GetRectangle().IntersectsWith(rt))
                {
                    return wall;
                }


            }
            return null;
        }

        public static bool IsCollidedBoss(Rectangle rt)
        {
            return Boss.GetRectangle().IntersectsWith(rt);

        }

        public static EnemyTank IsCollidedEnemy(Rectangle rt)
        {
            foreach (EnemyTank tank in tankList)
            {
                
                if (tank.GetRectangle().IntersectsWith(rt))
                {
                    return tank;
                }


            }
            return null;
        }

        public static MyTank IsCollidedMyTank(Rectangle rt)
        {


                if (myTank.GetRectangle().IntersectsWith(rt))
                {
                    return myTank;

                }


            return null;
        }



        public static void CreateMap()
        {
            //(x座標,y座標,長度,圖片,存放的List)
            CreateWall(1, 1, 5, Resources.wall,wallList);
            CreateWall(3, 1, 5, Resources.wall, wallList);
            CreateWall(5, 1, 4, Resources.wall, wallList);
            CreateWall(7, 1, 3, Resources.wall, wallList);
            CreateWall(9, 1, 4, Resources.wall, wallList);
            CreateWall(11, 1, 5, Resources.wall, wallList);
            CreateWall(13, 1, 5, Resources.wall, wallList);

            CreateWall(7, 5, 1, Resources.steel, steelList);
            CreateWall(2, 7, 1, Resources.wall, wallList);
            CreateWall(3, 7, 1, Resources.wall, wallList);
            CreateWall(4, 7, 1, Resources.wall, wallList);
            CreateWall(6, 7, 1, Resources.wall, wallList);
            CreateWall(7, 6, 2, Resources.wall, wallList);
            CreateWall(8, 7, 1, Resources.wall, wallList);
            CreateWall(10, 7, 1, Resources.wall, wallList);
            CreateWall(11, 7, 1, Resources.wall, wallList);
            CreateWall(12, 7, 1, Resources.wall, wallList);

            CreateWall(0, 7, 1, Resources.steel, steelList);
            CreateWall(14, 7, 1, Resources.steel, steelList);

            CreateWall(1, 9, 5, Resources.wall, wallList); 
            CreateWall(3, 9, 5, Resources.wall, wallList);
            CreateWall(5, 9, 4, Resources.wall, wallList);
            CreateWall(6, 10, 1, Resources.wall, wallList);
            CreateWall(7, 10, 2, Resources.wall, wallList);
            CreateWall(8, 10, 1, Resources.wall, wallList);
            CreateWall(9, 9, 4, Resources.wall, wallList);
            CreateWall(11, 9, 5, Resources.wall, wallList);
            CreateWall(13, 9, 5, Resources.wall, wallList);
            

            CreateWall(6, 14, 1, Resources.wall, wallList);
            CreateWall(7, 13, 1, Resources.wall, wallList);
            CreateWall(8, 14, 1, Resources.wall, wallList);


            CreateBoss(7, 14);



        }

        private static void CreateWall(int x, int y,int count,Image img,List<NotMovething> wallList)
        {   

            //以30為一個單位
            int xPosition = x*30;
            int yPosition = y*30;
            for(int i=yPosition;i<yPosition+count*30;i+=15)
            {
                NotMovething wall1 = new NotMovething(xPosition, i, img);
                NotMovething wall2 = new NotMovething(xPosition+15, i, img);
                wallList.Add(wall1);
                wallList.Add(wall2);
            }

        }

        public static void DestoryWall(NotMovething wall)
        {
            wallList.Remove(wall);
        }
        private static void CreateBoss(int x, int y)
        {
            

            //以30為一個單位
            Boss.X= x * 30;
            Boss.Y = y * 30;

        }

        public static void CreatMyTank()
        {

            int x = 5 * 30;
            int y = 14 * 30;
            myTank = new MyTank(x,y,2);


        }

        public static void KeyDown(KeyEventArgs args)
        {
            myTank.KeyDown(args);
        }

        public static void KeyUp(KeyEventArgs args)
        {
            myTank.KeyUp(args);
        }

    }
}
```