---
up: "[[專案/獨立遊戲之路/CSharp/C编程-第三季-坦克大战-宇宙最简单2021最新版/第三季-坦克大战-宇宙最简单2021最新版|第三季-坦克大战-宇宙最简单2021最新版]]"
繼承:
---
- [1]                                       

```csharp file:GameFramework
using System;
using System.Collections.Generic;
using System.Drawing;
using System.Linq;
using System.Security.AccessControl;
using System.Text;
using System.Threading.Tasks;
using System.Windows.Forms;

namespace _04_坦克大戰_正式
{
    enum GameStatus
    {
        Running,
        GameOver
    }
    internal class GameFramework
    {
        public static Graphics g;
        private static GameStatus status = GameStatus.Running;
        public static void Start()
        {
            GameObjectManager.Start();
            GameObjectManager.CreateMap();
            GameObjectManager.CreatMyTank();
            SoundManager.PlayStart(1);

        }

        public static void Update()
        {
            //GameObjectManager.DrawMap();
            //GameObjectManager.DrawMyTank();
            

            if(status == GameStatus.Running)
            {
                GameObjectManager.Update();
            }
            else if (status == GameStatus.GameOver)
            {
                GameOverUpdate();
            }
        }

        private static void GameOverUpdate()
        {
            int x = 450 / 2 - Properties.Resources.GameOver.Width / 2;
            int y = 450 / 2 - Properties.Resources.GameOver.Height / 2;
            g.DrawImage(Properties.Resources.GameOver, x, y);
        }
        public static void GameOver()
        {
            status = GameStatus.GameOver;
        }



    }
}
```