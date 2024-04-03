---
Daily: '[[20240403]]'
tags:
  - "#Unity"
up: "[[專案/獨立遊戲之路|獨立遊戲之路]]"
專案: "[[Claude教我寫Shader]]"
date: 2024-04-03
---
- [1]                                                           
## Shader 語法基礎

### 1. ShaderLab 語法結構

- **一個 Shader 文件通常由多個部分組成,包括 Properties、SubShader 和 Pass**
- **Properties 用於定義 Shader 的輸入參數,如紋理、顏色等**
- **SubShader 包含了一個或多個 Pass,每個 Pass 定義了一次渲染過程**
- **Pass 中包含了頂點著色器和片段著色器的程式碼,以及一些渲染狀態設置**

### 2. 屬性(Properties)

- **Properties 塊用於定義 Shader 的輸入參數,如紋理、顏色、浮點數等**
- **每個屬性都有一個名稱、類型和預設值**
- **屬性可以在材質檢視器中顯示和編輯,允許美術人員調整 Shader 的外觀**

### 3. 子著色器(SubShader)和通道(Pass)

- **SubShader 定義了 Shader 的一個變體,用於不同的渲染平台或功能級別**
- **一個 Shader 可以包含多個 SubShader,Unity 會根據設備的功能選擇合適的 SubShader 進行渲染**
- **Pass 定義了一次渲染過程,包括頂點著色器和片段著色器的程式碼**
- **一個 SubShader 可以包含多個 Pass,用於實現不同的渲染效果,如前向渲染、延遲渲染等**