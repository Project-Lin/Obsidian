---
tags:
  - Unity
  - "#Doc"
  - Particle
  - Light
aliases:
  - 粒子燈光
Home: "[[Unity Manual]]"
Previous: 
next:
---
# 燈光模組

## 使用燈光模組

此模組是 Particle System 組件的一部分。當您創建一個新的 Particle System GameObject，或將 Particle System 組件添加到現有的 GameObject 中時，Unity 會將 Lights 模組添加到 Particle System 中。默認情況下，Unity 會禁用此模組。要創建一個新的 Particle System 並啟用此模組：

1. 點擊 GameObject > Effects > Particle System。
2. 在 Inspector 中，找到 Particle System 組件。
3. 在 Particle System 組件中，找到 Lights 模組折疊。
4. 在折疊標題的左側，啟用核取方塊。

## API

由於此模組是 Particle System 組件的一部分，因此您可以通過 ParticleSystem 類訪問它。有關如何在運行時訪問它並更改值的信息，請參閱燈光模組 API 文檔。

## 屬性

對於此部分的某些屬性，您可以使用不同的模式來設置它們的值。有關您可以使用的模式的信息，請參閱隨時間變化的屬性。

| 屬性 | 功能 |
| --- | --- |
| 燈光（Light） | 分配描述粒子燈光外觀的燈光預製件。 |
| 比率（Ratio） | 介於 0 和 1 之間的值，描述將收到燈光的粒子的比例。 |
| 隨機分佈（Random Distribution） | 選擇燈光是隨機分配還是定期分配。 |
| 使用粒子顏色（Use Particle Color） | 當設置為 True 時，燈光的最終顏色將受到其附加到的粒子顏色的調製。 |
| 尺寸影響範圍（Size Affects Range） | 啟用時，燈光中指定的範圍將乘以粒子的大小。 |
| Alpha 影響強度（Alpha Affects Intensity） | 啟用時，燈光的強度將乘以粒子的 alpha 值。 |
| 範圍乘數（Range Multiplier） | 使用此曲線在粒子的生命期內對燈光的範圍應用自定義乘數。 |
| 強度乘數（Intensity Multiplier） | 使用此曲線在粒子的生命期內對燈光的強度應用自定義乘數。 |
| 最大燈光（Maximum Lights） | 使用此設置可避免意外地創建大量燈光。 |

## 詳細信息

Lights 模組是將實時燈光快速添加到粒子效果中的快速方法。它可用於使系統對其周圍產生光線，例如火、煙花或閃電。它還允許您讓燈光從它們附加到的粒子繼承各種屬性。此模組使得很容易非常快速地創建大量實時燈光，而實時燈光的性能成本很高，特別是在正向渲染模式下。如果燈光還投射陰影，則性能成本更高。為了防止意外調整發射率而導致創建數千個實時燈光，應使用“最大燈光”屬性。創建比目標硬件能夠管理的燈光多可能會導致減速和無響應。
