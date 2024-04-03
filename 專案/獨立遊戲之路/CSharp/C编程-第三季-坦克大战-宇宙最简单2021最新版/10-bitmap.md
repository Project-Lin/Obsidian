---
tags:
  - csharp
up: "[[專案/獨立遊戲之路/CSharpRoad|CSharpRoad]]"
date: 2024-03-29T16:18:00
課程: "[[C编程-第四季-高级篇-宇宙最简单2022最新版|C编程-第四季-高级篇-宇宙最简单2022最新版]]"
---
- [1] 使用 bitmap 創建透明背景圖片
```csharp file:Program
//使用bitmap創建star1
Bitmap bitmap = Properties.Resources.Star1;
//使用bitmap方法 將背景黑色變透明
bitmap.MakeTransparent(Color.Black);
//繪製圖片
g.DrawImage(bitmap, 200, 200);
```
