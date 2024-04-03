---
tags:
  - csharp
  - String
up: "[[專案/獨立遊戲之路/CSharpRoad|CSharpRoad]]"
date: 2024-03-27
課程: "[[C编程-第四季-高级篇-宇宙最简单2022最新版|C编程-第四季-高级篇-宇宙最简单2022最新版]]"
---
- [1] StringBuilder 的使用和 string 的差別在於 StringBuilder 是可變的，而 string 是不可變的

- [1] 通常在需要大量操作字符串時使用 StringBuilder，因為 StringBuilder 的性能比 string 更好

```csharp file:Program

using System.Text;

StringBuilder sb = new StringBuilder();

//Append()方法用來將字串附加到StringBuilder的結尾
sb.Append("Hello, World!");

Console.WriteLine(sb);  //Hello, World!

//Insert()方法用來將字串插入到StringBuilder的指定位置
sb.Insert(7, "C#");

Console.WriteLine(sb); //Hello, C#World!

//Remove()方法用來刪除StringBuilder的指定位置的字符
sb.Remove(7,2);

Console.WriteLine(sb);  //Hello, World!

//Replace()方法用來替換StringBuilder的指定位置的字符
sb.Replace("World","C#");
Console.WriteLine(sb);  //Hello, C#!


//StringBuilder capacity
//StringBuilder的capacity是可以設置的，當capacity不足時，會自動擴展容量
StringBuilder stringBuilder = new StringBuilder(10);


```