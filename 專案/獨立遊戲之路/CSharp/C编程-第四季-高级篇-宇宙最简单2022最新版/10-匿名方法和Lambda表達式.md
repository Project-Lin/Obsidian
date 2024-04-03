---
tags:
  - csharp
  - "#Lambda"
up: "[[專案/獨立遊戲之路/CSharpRoad|CSharpRoad]]"
date: 2024-03-28T16:18:00
課程: "[[C编程-第四季-高级篇-宇宙最简单2022最新版|C编程-第四季-高级篇-宇宙最简单2022最新版]]"
---

```csharp file:Program
namespace _10_匿名方法和Lambda表達式
{
    internal class Program
    {
        static void Main(string[] args)
        {
            Func<int, int, int> a = delegate (int x, int y)
            {
                return x + y;
            };

            Console.WriteLine(a(10, 20));  //30


            //Lambda表達式

            //省略 delegate 和 參數類型 int
            //Func<參數,參數,返回值> 委託名稱 = 
                                   //(參數1,參數2) => { return 運算式; };
            Func<int, int, int> b = (x, y) => { return x + y; };

            //如果只有一條語句 可以省略{} 和 return
            Func<int, int, int> c = (x, y) => x + y;


            Console.WriteLine(b(10, 20));  //30
            Console.WriteLine(c(10, 20));  //30
        }



    }
}

```

# 匿名方法和 Lambda 表達式

## 匿名方法

### 定義匿名方法

- 使用 `delegate` 關鍵字定義匿名方法，並且指定委託的返回類型和參數類型。
- 使用 `delegate (int x, int y)` 定義了接受兩個 int 型參數並返回 int 的匿名方法。
- 在匿名方法內部，執行 `x + y` 運算。

### 使用匿名方法

- 創建了 `Func<int, int, int>` 委託 `a`，並將匿名方法賦值給它。
- 使用 `a(10, 20)` 調用匿名方法，獲取結果為 `30`。

## Lambda 表達式

### 定義 Lambda 表達式

- 使用 `Func<參數,參數,返回值>` 委託名稱 `b`，並使用 => 符號定義 Lambda 表達式。
- Lambda 表達式 `b = (x, y) => { return x + y; }` 等價於 `delegate (int x, int y) { return x + y; }`。

### Lambda 表達式的簡化形式

- 如果 Lambda 表達式只有一條語句，可以省略 `{}` 和 `return` 關鍵字。
- 創建了 `Func<int, int, int>` 委託 `c`，並使用簡化形式的 Lambda 表達式賦值給它。
- Lambda 表達式 `c = (x, y) => x + y;`。

### 使用 Lambda 表達式

- 使用 `b(10, 20)` 和 `c(10, 20)` 分別調用 Lambda 表達式，獲取結果皆為 `30`。
