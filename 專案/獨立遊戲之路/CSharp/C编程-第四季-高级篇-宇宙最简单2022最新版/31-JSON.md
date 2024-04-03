---
Daily: "[[20240331]]"
tags:
  - csharp
  - "#json"
up: "[[專案/獨立遊戲之路/CSharpRoad|CSharpRoad]]"
課程: "[[C编程-第四季-高级篇-宇宙最简单2022最新版]]"
date: 2024-03-31
---

# JSON（JavaScript Object Notation）

## 什麼是 JSON？

JSON（JavaScript Object Notation）是一種輕量級的資料交換格式，通常用於網路應用程式中傳送結構化資料。它基於JavaScript語言的一部分，但已成為一個獨立的標準，可以由許多不同的程式語言支援和解析。

## JSON 的特點

- **人類可讀性**：JSON 格式的資料易於人類閱讀和編寫，以純文字形式表示。
- **資料結構化**：JSON 能夠表示結構化的資料，包括物件（objects）、陣列（arrays）、數字、字串、布林值和 null。
- **輕量級**：JSON 資料格式相對簡潔，不會佔用過多的空間。
- **跨平台**：JSON 在各種程式語言和平台上都有廣泛的支援。

## JSON 資料類型

JSON 支援以下幾種資料類型：

- **物件（Object）**：一組無序的鍵值對，鍵必須是字串，值可以是任意有效的 JSON 資料型別。物件以 `{}` 表示。
- **陣列（Array）**：有序的值的集合，每個值可以是任何有效的 JSON 資料型別。陣列以 `[]` 表示。
- **字串（String）**：以雙引號 `" "` 包裹的 Unicode 字符序列。
- **數字（Number）**：整數或浮點數。
- **布林值（Boolean）**：表示真或假的值。
- **null**：表示空值。

## JSON 範例

以下是一個簡單的 JSON 範例：

```json
{
  "姓名": "小明",
  "年齡": 25,
  "已婚": false,
  "興趣": ["閱讀", "運動"],
  "地址": {
    "城市": "台北",
    "郵遞區號": "100"
  }
}

```

這個 JSON 包含了一個物件，其中包含有關小明的一些資訊，包括姓名、年齡、婚姻狀況、興趣和地址。



## JSON 建構結構

JSON 建構由多個資料類型組成，可透過嵌套來建立複雜的結構。

### 物件（Object）

物件是一組無序的鍵值對，每個鍵必須是唯一的，且必須是字串。值可以是任何有效的 JSON 資料型別，包括物件、陣列、字串、數字、布林值或 null。物件由 `{}` 包裹，鍵和值以冒號 `:` 分隔，鍵值對之間以逗號 `,` 分隔。


### 陣列（Array）

陣列是有序的值的集合，每個值可以是任何有效的 JSON 資料型別。陣列由 `[]` 包裹，值之間以逗號 `,` 分隔。


### 字串（String）、數字（Number）、布林值（Boolean）、null

字串是以雙引號 `"` 包裹的 Unicode 字符序列。數字是整數或浮點數。布林值表示真或假。null 表示空值。


# 練習

```csharp  file:Program
using Newtonsoft.Json;
using System.Net.Http.Json;

namespace _31_Json操作
{
    internal class Program
    {
        static void Main(string[] args)
        {
            //Deserialize 反序列化
            Skill[] s = JsonConvert.DeserializeObject<Skill[]>(File.ReadAllText("Skills.json"));
            
            foreach (Skill temp in  s)
            {
                Console.WriteLine($"ID:{temp.ID} 名稱:{temp.Name} 傷害:{temp.Damage}");
            }
             
            //Serialize 序列化
            Skill b = new Skill();
            b.ID = 1;
            b.Name = "緩速";
            b.Damage = "50";
            
            Console.WriteLine(JsonConvert.SerializeObject(b));  //{"ID":1,"Name":"緩速","Damage":"50"}
        }
    }
}

```

```csharp  file:Class
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace _31_Json操作
{
    internal class Skill
    {
        public int ID { get; set; }
        public string Name { get; set; }
        public string Damage { get; set; }
    }
}

```

```json
 [
    {"ID": 2,"Name": "外圈刮","Damage": 150 },
    {"ID": 3,"Name": "拉過來","Damage": 75 },
    {"ID": 4,"Name": "一個劈","Damage": 350 }
 ]
```

> [!failure] 注意
> jason名稱 和 變量 要和 class中 的資料結構保持一致
