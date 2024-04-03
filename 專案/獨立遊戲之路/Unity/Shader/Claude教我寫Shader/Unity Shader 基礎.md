---
Daily: '[[20240403]]'
tags:
  - "#Unity"
up: "[[專案/獨立遊戲之路|獨立遊戲之路]]"
專案: "[[Claude教我寫Shader]]"
date: 2024-04-03
---
- [1]                                                           
## Unity Shader 基礎

### 1. Shader 概述

#### 什麼是 Shader？

- **Shader 是一種用於描述物體如何被渲染的程式**
- **定義物體的外觀、光照效果、紋理貼圖等視覺屬性**
- 在 Unity 中,Shader 使用 ShaderLab 語言編寫,**運行在 GPU 上**

#### Shader 在 Unity 中的作用

- **控制物體的外觀和視覺效果**
- **實現複雜的光照模型和材質屬性**
- **優化渲染性能,減輕 CPU 的負擔**

#### Shader 的類型

- **頂點著色器(Vertex Shader):處理頂點的位置、法線、紋理座標等屬性**
- **片段著色器(Fragment Shader):處理每個像素的顏色、透明度、紋理採樣等屬性**
- **表面著色器(Surface Shader):基於物理的渲染(PBR)的高級著色器**
- **計算著色器(Compute Shader):用於通用計算和並行處理的著色器**

### 2. URP 渲染管線介紹

#### URP 的優勢和特點

- **更好的性能和效能**
- **更簡單的設置和使用**
- **支援多平台和不同硬體**
- **提供了一組預定義的渲染器和著色器**

#### URP 與內置渲染管線的區別

- **URP 是預定義的渲染管線,內置渲染管線是 Unity 的預設渲染管線**
- **URP 提供更好的性能和優化,內置渲染管線更加靈活和可自訂**
- **URP 使用不同的 Shader 語法和結構,需要專門為 URP 編寫 Shader**

### 3. 設置 Unity 專案與 URP

#### 創建新的 Unity 專案

1. 打開 Unity Hub,點擊「新建」按鈕
2. 選擇「3D」模板,輸入專案名稱,選擇存儲位置
3. **在「Unity 版本」下拉選單中選擇「Unity 2023.2.16f1」**
4. 點擊「創建」按鈕,等待專案創建完成

#### 導入 URP 資源包

1. 打開 Unity 編輯器,進入專案視窗
2. 點擊「視窗」>「Package Manager」打開套件管理器
3. **在套件列表中找到「Universal RP」套件,點擊「安裝」按鈕**
4. 等待 URP 套件安裝完成

#### 配置 URP 渲染管線

1. 在專案視窗中,右鍵點擊「資產」>「創建」>「渲染管線」>「Universal Render Pipeline」>「Pipeline Asset」
2. 選擇一個存儲位置,為渲染管線資產命名(例如「URPAsset」)
3. **在「編輯」>「專案設置」>「圖形」中,將「Scriptable Render Pipeline Settings」設置為剛剛創建的 URP 資產**




 Pass
         {
            CGPROGRAM
            #pragma vertex vert
            #pragma fragment frag
            // make fog work
            #pragma multi_compile_fog

再來是這段 前面有提到使用urp推薦使用