---
up: "[[專案/獨立遊戲之路/CSharp/C编程-第三季-坦克大战-宇宙最简单2021最新版/第三季-坦克大战-宇宙最简单2021最新版|第三季-坦克大战-宇宙最简单2021最新版]]"
繼承:
---
- [1]                                       

```csharp file:SoundManager
using _04_坦克大戰_正式.Properties;
using System;
using System.Collections.Generic;
using System.Linq;
using System.Media;
using System.Text;
using System.Threading;
using System.Threading.Tasks;

namespace _04_坦克大戰_正式
{
    internal class SoundManager
    {
        private static SoundPlayer startPlayer = new SoundPlayer(Resources.start);
        private static SoundPlayer addPlayer = new SoundPlayer(Resources.add);
        private static SoundPlayer hitPlayer = new SoundPlayer(Resources.hit);
        private static SoundPlayer blastPlayer = new SoundPlayer(Resources.blast);
        private static SoundPlayer firePlayer = new SoundPlayer(Resources.fire);
        //public static void PlayStart()
        //{

        //    startPlayer.Play();

        //}
        //public static void PlayAdd()
        //{
        //    addPlayer.Play();
        //}
        //public static void PlayHit()
        //{
        //    hitPlayer.Play();
        //}
        //public static void PlayBlast()
        //{
        //    blastPlayer.Play();
        //}
        //public static void PlayFire()
        //{
        //    firePlayer.Play();
        //}

        public static void PlayStart(int a)
        {
            switch (a)
            {
                case 1:
                    startPlayer.Play();
                    break;
                case 2:
                    addPlayer.Play();
                    break;
                case 3:
                    hitPlayer.Play();
                    break;
                case 4:
                    blastPlayer.Play();
                    break;
                case 5:
                    firePlayer.Play();
                    break;
            }


        }

    }
}
```