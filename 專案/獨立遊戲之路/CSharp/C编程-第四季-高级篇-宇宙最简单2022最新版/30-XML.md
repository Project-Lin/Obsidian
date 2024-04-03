---
Daily: "[[20240331]]"
tags:
  - csharp
  - "#xml"
up: "[[專案/獨立遊戲之路/CSharpRoad|CSharpRoad]]"
課程: "[[C编程-第四季-高级篇-宇宙最简单2022最新版]]"
date: 2024-03-31
---

- [1] 元素命名規則
- [ ] 可以含字母數字
- [ ] 不能數字或標點開頭
- [ ] 不能以xml 開頭
- [ ] 不能包含空格



> [!bug] XML使用注釋
> 使用注釋 在讀取Node的時候也會被讀取到 建議不要寫注釋在文件內


```xml {'title':'hallo.xml'} file:skills
<?xml version="1.0" encoding="utf-8" ?> 
<!--1.0版本，utf-8编码-->
<!--XML文档必須有根元素-->
<skills>          <!--根元素-->
	<skill>           
		<id>2</id>       
		<name lang="zh">火球術</name>   <!-- <元素名稱 屬性> 賦值 </元素名稱> --> 
		<damage>100</damage>
	</skill>
	<skill>
		<id>3</id>
		<name lang="zh">寒冰箭</name>
		<damage>150</damage>
	</skill>
	<skill>
		<id>4</id>
		<name lang="zh">烈焰風暴</name>
		<damage>300</damage>
	</skill>


</skills>

```

```csharp {'title':'hallo.cs'} file:XML操作
using System.Xml;

namespace _30_XML操作
{
    internal class Program
    {
        static void Main(string[] args)
        {
            List<SKill> List = new List<SKill>();
            XmlDocument xml = new XmlDocument();

            //載入xml文件  兩種方法 第一種是直接讀取xml文本 第二種是讀取xml文件路徑
            xml.LoadXml(File.ReadAllText("Skills.xml"));
            xml.Load("Skills.xml");

            XmlNode root = xml.ChildNodes[3];
            XmlNodeList SkillList = root.ChildNodes;
            foreach (XmlNode skill in SkillList)
            {
                SKill sKillObj = new SKill();

                ////方法一: 遍歷子節點
                //foreach (XmlNode node in skill.ChildNodes)
                //{
                //    if(node.Name == "id")
                //    {
                //        sKillObj.ID = int.Parse(node.InnerText);
                //    }
                //    else if (node.Name == "name")
                //    {
                //        sKillObj.Name = node.InnerText;
                //        sKillObj.language = node.Attributes[0].Value;
                //    }
                //    else 
                //    {
                //        sKillObj.Damage = int.Parse(node.InnerText);
                //    }

                //}

                //方法二: 使用XmlElement
                XmlElement idEle = skill["id"];   //取得id節點 <id>2</id>
                sKillObj.ID = int.Parse(idEle.InnerText);

                XmlElement nameEle = skill["name"];   //取得name節點 <name language="chinese">火球術</name>
                sKillObj.Name = nameEle.InnerText;

                ////單個屬性可以用以下方法
                //sKillObj.language = nameEle.GetAttribute("lang");

                //多個屬性可以用以下方法  先取得集合 再用索引取得特定屬性
                XmlAttributeCollection attributeCollection = nameEle.Attributes;
                XmlAttribute attribute = attributeCollection["lang"];

                XmlElement damageEle = skill["damage"];   //取得damage節點 <damage>100</damage>
                sKillObj.Damage = int.Parse(damageEle.InnerText);

                List.Add(sKillObj);
            }

            foreach (SKill item in List)
            {
                Console.WriteLine($"技能編號:{item.ID} 技能名稱:{item.Name} 傷害:{item.Damage} 語言:{item.language}");
            }

        }
    }
}

```

- [1] XmlElement  元素單位   
- ``<id>2</id>``
- ``<name lang="zh">火球術</name>``


