---
Daily: '[[20240403]]'
tags:
  - "#Unity"
up: "[[專案/獨立遊戲之路|獨立遊戲之路]]"
專案: "[[Unity Shader著色器入門教程]]"
date: 2024-04-03
---
- [1]                                                           
```cs
Shader "Custom/Shader_01"
{   
    // 定義一個自定義 Shader，名為 "Custom/Shader_01"

    SubShader
    {   
        // 定義一個子 Shader，包含在 SubShader 中的所有 Pass 將被渲染器依次處理

        pass
        {
            // 定義一個 Pass，每個 Pass 包含一個頂點着色器和一個片段着色器

            CGPROGRAM
            // 表示下面的代碼是使用 CG 語言編寫的

            #pragma vertex vertex
            // 告訴渲染器使用 vertex 函數作為頂點着色器的入口點

            #pragma fragment fragment
            // 告訴渲染器使用 fragment 函數作為片段着色器的入口點

            // 頂點着色器(vertex shader)的函數定義：
            float4 vertex(float4 v : POSITION) : SV_POSITION
            {
                // UnityObjectToClipPos 將頂點位置從對象空間轉換為裁剪空間
                return UnityObjectToClipPos(v);
            }

            // 片段着色器(fragment shader)的函數定義：
            float4 fragment() : SV_Target
            {
                // 返回一個完全不透明的白色
                return float4(1, 1, 1, 1);
            }
            
            ENDCG
            // 表示 CG 代碼塊的結束
        }
    }
}

```