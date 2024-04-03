---
tags:
  - csharp
  - Delegate
up: "[[專案/獨立遊戲之路/CSharpRoad|CSharpRoad]]"
date: 2024-03-28
課程: "[[C编程-第四季-高级篇-宇宙最简单2022最新版|C编程-第四季-高级篇-宇宙最简单2022最新版]]"
---
- [1] `Action ` 委託是一個沒有返回值的泛型委託

```csharp file:Program
namespace _06_Action委託
{
    internal class Program
    {
        static void Main(string[] args)
        {
            //Action 委託是一個沒有返回值的泛型委託
            //Action 委託的泛型參數最多可以有16個

            Action a = Print;  
            a();  //無參

            //Action<int> 委託的泛型參數是int類型
            Action<int> b = PrintInt;
            b(10);  //10

            //Action<long,int,float> 委託的泛型參數是long,int,float 類型
            Action<long, int, float> c = PrintLong;
            c(10, 20, 30.0f); //10,20,30


        }
        private static void Print()
        {
            Console.WriteLine("無參");
        }
        private static void PrintInt(int x)
        {
            Console.WriteLine(x);
        }
        private static void PrintLong(long x, int y, float z)
        {
            Console.WriteLine($"{x},{y},{z}");
        }
    }
}

```