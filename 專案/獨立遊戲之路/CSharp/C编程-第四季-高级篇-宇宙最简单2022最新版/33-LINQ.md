---
Daily: "[[20240331]]"
tags:
  - csharp
  - "#LINQ"
  - Lambda
up: "[[專案/獨立遊戲之路/CSharpRoad|CSharpRoad]]"
課程: "[[C编程-第四季-高级篇-宇宙最简单2022最新版]]"
date: 2024-03-31
---


```csharp  file:Program
using System.Collections.ObjectModel;

namespace _33_LINQ
{
    internal class Program
    {
        static void Main(string[] args)
        {
            var masterList = new List<Master>()
            {
                new Master() {Id = 1,Age=100,Kongfu="閃電五連鞭",Door="馬門", Level=5, Name="馬寶國" },
                new Master() {Id = 2,Age=60,Kongfu="大麻拳",Door="椅子門", Level=3, Name="toyz" },
                new Master() {Id = 3,Age=80,Kongfu="三槍拳",Door="成吉思汗", Level=7, Name="陳之憾" },
                new Master() {Id = 4,Age=40,Kongfu="金幣炸彈",Door="董事會", Level=7, Name="中培生" },
                new Master() {Id = 5,Age=900,Kongfu="閃電五連鞭",Door="馬門", Level=9, Name="馬英九" },
                new Master() {Id = 6,Age=50,Kongfu="大麻拳",Door="椅子門", Level=4, Name="阿扣" },
                new Master() {Id = 7,Age=70,Kongfu="三槍拳",Door="成吉思汗", Level=2, Name="小雨" },
                new Master() {Id = 8,Age=20,Kongfu="豪車衝撞",Door="董事會", Level=1, Name="赴俄帶" },
            };

            var kongfuList = new List<Kongfu>()
            {
                new Kongfu() {Id = 1, Name="閃電五連鞭",Power=1 },
                new Kongfu() {Id = 2, Name="大麻拳",Power=3 },
                new Kongfu() {Id = 3, Name="三槍拳" ,Power=6},
                new Kongfu() {Id = 4, Name="金幣炸彈",Power=4 },
                new Kongfu() {Id = 5, Name="豪車衝撞",Power=9 },
            };

            //使用for循環查詢等級大於5的人
            var res = new List<Master>();
            foreach(var temp in masterList)
            {
                if (temp.Level > 5)
                {
                    res.Add(temp);
                }
                
            }

            foreach(var temp in res) 
            {
                Console.WriteLine(temp);
            }
            
            //使用LINQ查詢

            var resoure = from m in masterList    //form 設定查詢的集合  m 是臨時變量
                          where m.Level > 5          //where後面寫查詢的條件  多條件可以使用 && 加上條件
                          select m;               //返回集合  可以選擇返回的屬性(m.Name 只返回名字

            foreach (var temp in resoure)
            {
                Console.WriteLine(temp);
            }//返回集合

            //使用where方法過濾
            var resoure1 = masterList.Where(Test);
            //改成lambda表達式
            var resoure2 = masterList.Where(m => m.Level > 8);

			//聯合條件
  			var resoure3 = from m in masterList
        	from k in kongfuList
        	where m.Kongfu == k.Name && k.Power > 5
            select new { Master = m, Kongfu = k };

			//使用join on 聯合
   		     var resoure4 = from m in masterList
             join k in kongfuList on m.Kongfu equals k.Name
             where k.Power > 5 
             select new {master = m, Kongfu = k};

			//分組查詢 名子按照拳法分類 統計拳法有多少人在使用
		     var resoure5 = from k in kongfuList
             join m in masterList on k.Name equals m.Kongfu
             into groups
             orderby groups.Count()
             select new { Kongfu = k, count = groups.Count() };

			//針對單一字段分組
   			var resoure6 = from m in masterList
            group m by m.Kongfu 
            into groups
            select new {key =groups.Key, count = groups.Count() };
   			//key 表示是哪一種konfu分的組

            foreach (var temp in resoure2)
            {
                Console.WriteLine(temp);
            }//返回集合


			//any  all  判斷集合中是否滿足某個條件
            //any 任一個數據滿足就返回true
            bool ans = masterList.Any(m => m.Kongfu == "三槍拳");
   
            //all 全部滿足才返回true
            bool ans1 = masterList.All(m => m.Kongfu == "三槍拳");
        
        }
        static bool Test( Master m)
        {
            if (m.Level > 5)
            {
                return true;
            }
            return false;
        }
    }
}

```


# LINQ 的使用方法

## 準備工作

### 建立類別

- 建立了 `Master` 和 `Kongfu` 兩個類別，分別表示武林高手和武功。

## LINQ 查詢方法

### 使用 LINQ 查詢

- 使用 `from` 關鍵字從 `masterList` 中選擇符合條件的元素。
- 使用 `where` 子句指定條件，這裡是查找等級大於 5 的武林高手。
- 使用 `select` 子句返回符合條件的元素集合。

### 使用 Where 方法過濾

- 使用 `Where` 方法從 `masterList` 中過濾符合條件的元素。
- 定義了 `Test` 方法，根據武林高手的等級進行過濾。

### 使用 [[10-匿名方法和Lambda表達式| Lambda 表達式]] 

- 使用 `Where` 方法結合 Lambda 表達式從 `masterList` 中過濾符合條件的元素。
- Lambda 表達式 `m => m.Level > 8` 表示選擇等級大於 8 的武林高手。
- Lambda 表達式可以使用 `m =>` 的形式定義匿名函數，其中 `m` 是代表每個元素的變數，`m.Level > 8` 是條件判斷。

## 聯合查詢步驟

### 連續使用 form

- 使用 `from` 關鍵字從 `masterList` 和 `kongfuList` 中選擇元素。
- 使用 `where` 子句指定聯合條件，這裡是當 `Master` 的 `Kongfu` 屬性等於 `Kongfu` 的 `Name` 屬性，且 `Kongfu` 的 `Power` 大於 5。
- 使用 `select` 子句返回符合條件的元素集合，這裡返回的集合中包含了 `Master` 和 `Kongfu` 的相關訊息。

### 使用Join on

- 使用 `join` 關鍵字將 `masterList` 和 `kongfuList` 兩個資料集合聯合起來，並根據 `Master` 的 `Kongfu` 屬性和 `Kongfu` 的 `Name` 屬性進行聯合。
- 使用 `on` 子句指定聯合條件，這裡是當 `Master` 的 `Kongfu` 屬性等於 `Kongfu` 的 `Name` 屬性時進行聯合。
- 使用 `where` 子句過濾符合條件的元素，這裡是當 `Kongfu` 的 `Power` 大於 5 時。
- 使用 `select` 子句返回符合條件的元素集合，這裡返回的集合中包含了 `Master` 和 `Kongfu` 的相關訊息。


## 分組查詢和排序

### Into 分組

- 使用 `join` 關鍵字將 `kongfuList` 和 `masterList` 兩個資料集合聯合起來，並根據 `Kongfu` 的 `Name` 屬性和 `Master` 的 `Kongfu` 屬性進行聯合。
- 使用 `into` 關鍵字將聯合的結果分組，這裡根據 `Kongfu` 的 `Name` 屬性分組。

### Orderby 排序  

- 使用 `orderby` 子句對分組後的結果按照組內元素的數量進行排序。
- 使用 `select` 子句返回結果，其中包含了拳法和使用該拳法的人數。


##   單一字段分組步驟

### Group by 

- 使用 `group by` 關鍵字將 `masterList` 資料集合根據 `Kongfu` 字段進行分組。
    - `m` 表示要分組的元素。
    - `m.Kongfu` 指定了按照哪個字段進行分組。

### 分組統計

- 使用 `select` 子句返回結果，其中包含了拳法和使用該拳法的人數。
    - `groups.Key` 表示每個分組的鍵值，即拳法名稱。
    - `groups.Count()` 表示該分組中的元素個數，即使用該拳法的人數。


## 量詞操作符

### 使用 `Any` 方法

- 使用 `Any` 方法判斷是否有任一個元素滿足指定條件。
    - `masterList.Any(m => m.Kongfu == "三槍拳")` 表示判斷是否有任一個元素的 Kongfu 字段等於 "三槍拳"。
    - 如果有任一個元素滿足條件，則返回 `true`，否則返回 `false`。

### 使用 `All` 方法

- 使用 `All` 方法判斷是否所有元素都滿足指定條件。
    - `masterList.All(m => m.Kongfu == "三槍拳")` 表示判斷是否所有元素的 Kongfu 字段都等於 "三槍拳"。
    - 如果所有元素都滿足條件，則返回 `true`，否則返回 `false`。