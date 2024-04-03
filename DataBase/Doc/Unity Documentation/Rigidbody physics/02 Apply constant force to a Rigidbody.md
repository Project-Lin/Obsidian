---
tags:
  - Unity
  - "#Doc"
  - Rigidbody
aliases: 
Home: "[[Unity Manual]]"
Previous: "[[01 Configure Rigidbody Colliders]]"
next: 
---
# 剛體上施加恆定力

>在 Unity 中，若要向遊戲物件的剛體 (Rigidbody) 施加恆定的線性或旋轉力，可以添加 恆定力 (Constant Force) 組件。該組件由 API 類 ConstantForce 所表示。有關如何配置組件屬性的詳細信息，請參閱恆定力組件參考。

### 最大速度限制

需要注意的是，恆定力並不等於恆定速度。施加恆定力後，物體的運動速度會隨著時間的推移而不斷增加，這與現實生活中物體會加速到一定速度後保持勻速運動不同。在 Unity 的物理模擬中，默認情況下線性加速度和角加速度會一直持續下去，直到剛體達到最大速度為止。剛體的最大線性速度和最大角速度可以通過代碼進行設置，具體屬性為 Rigidbody.maxLinearVelocity 和 Rigidbody.maxAngularVelocity。

### 配置恆定向前加速度

以下是如何使遊戲物件持續向前加速（例如模擬火箭的運動）：

添加 恆定力 組件到遊戲物件上。
在 恆定力 組件中，將 相對力 (Relative Force) 的 Z 軸設置為正值。
在 剛體 (Rigidbody) 組件中，禁用 使用重力 (Use Gravity) 屬性。這可以確保沒有其他重力會影響遊戲物件的運動。
在 剛體 (Rigidbody) 組件中，設置 阻力 (Drag) 屬性，使剛體不會超過您想要的最大速度（阻力越高，最大速度越低）。這可能需要一些微調才能達到您想要的效果。


#### 額外說明
文檔中提到了關節 (Joint) 的概念，沒有進行詳細解釋。關節用於連接剛體對象，並限制剛體對象之間的運動。
文檔最後提到了 PhysX 物理系統休眠的細節，可以參考官方文檔了解更多信息。
總結


恆定力組件可以向剛體施加恆定的線性和旋轉力。
剛體在受到恆定力後會持續加速，直到達到最大速度。
可以通過代碼設置剛體的最大線性速度和最大角速度。
可以通過配置恆定力組件和剛體屬性來模擬各種物理運動，例如火箭的運動。



>To apply a constant linear or rotational force to a GameObject ’s Rigidbody , add the Constant Force component (represented by the API class ConstantForce) to your GameObject. See Constant Force component reference for details on how to configure the properties on the component.

Set maximum velocity limitations Constant force is not the same as constant speed. When you apply a constant force, the speed of movement accelerates over time based on the value of the force. In real life, this acceleration continues indefinitely. By default in Unity’s physics simulation, linear acceleration continues indefinitely, and angular acceleration continues until the Rigidbody reaches a max velocity of 50 rad/s. You can change these maximum velocities in code, via the properties Rigidbody.maxLinearVelocity and Rigidbody.maxAngularVelocity.

Configure constant forward acceleration To make a GameObject constantly accelerate forward (for example, to make it behave like a rocket), do the following:

1. Add a Constant Force component to the GameObject.
2. On the Constant Force component, set the Relative Force Z axis to a positive value.
3. On the Rigidbody, disable Use Gravity. This ensures that there is no competing gravitational force acting upon the GameObject.
4. On the Rigidbody component, set the Drag property so that the Rigidbody does not exceed your preferred maximum velocity (the higher the drag, the lower the maximum velocity will be). This might require some trial and error to get the effect you want.