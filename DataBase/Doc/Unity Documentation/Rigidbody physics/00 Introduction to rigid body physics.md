---
tags:
  - Rigidbody
  - Unity
  - Doc
aliases:
  - 物理系統
  - 碰撞體
Home: "[[Unity Manual]]"
Previous: "[[Unity Manual]]"
next: "[[01 Configure Rigidbody Colliders]]"
---

# 剛體物理學簡介

在現實世界的物理學中，剛體是指任何不會在物理力作用下變形或改變形狀的物理實體。剛體的任意兩點之間的距離在時間上保持不變，不受外部力的影響。

### 模擬基於物理的行為

- 要模擬物理行為，如運動、重力、碰撞和連接，需要將場景中的物體配置為剛體。
- 在Unity的PhysX系統中，可以通過為GameObject分配Rigidbody組件來將其配置為剛體。
- 剛體組件在API中由Rigidbody類表示。

### 基於物理的運動

- 在Unity中，Rigidbody組件提供了一種基於物理的方式來控制GameObject的運動和位置。
- 使用模擬的物理力和扭矩來移動GameObject，讓物理引擎計算結果。
- 大多數情況下，如果一個GameObject有一個Rigidbody，應該使用Rigidbody屬性來移動GameObject，而不是Transform屬性。

### 剛體GameObject的約束

- Unity處理基於物理的運動是全局的，而不是局部的。
- 當具有Rigidbody的GameObject通過基於物理的運動移動時，它獨立於任何父或子GameObject移動。
- 通過腳本控制Rigidbody的主要類是AddForce（向GameObject添加力）和AddTorque（向GameObject施加扭矩）。

### 基於物理的運動和非基於物理的運動的剛體GameObject

- 在某些情況下，可能希望物理系統檢測GameObject，但不控制它。
- 在Unity中，非基於物理的運動稱為運動學運動。
- 使用Is Kinematic屬性將附加的GameObject定義為非物理的。

### 剛體優化

- 當剛體的移動速度低於睡眠閾值時，Unity將剛體設置為“睡眠”，即物理系統不再包括它在物理計算中。
- 默認情況下，剛體組件的睡眠和喚醒會自動發生。
- 可以通過腳本控制這種行為，通過Rigidbody.Sleep和Rigidbody.WakeUp方法。


###   設置 Rigidbody 的碰撞器

碰撞器（Colliders）用於定義 Rigidbody 的物理邊界。若要允許碰撞發生，必須在 GameObject 上添加碰撞器並與 Rigidbody 一同使用。

- Rigidbody 碰撞器：碰撞器確定 Rigidbody 的物理邊界。如果一個 Rigidbody 與另一個碰撞，物理引擎僅在兩個 GameObject 都附有碰撞器時計算碰撞。若一個 GameObject 具有 Rigidbody 但沒有碰撞器，它會穿透其他 GameObject，Unity 不會將其納入碰撞計算。
    
- 碰撞物體的相對質量決定它們在碰撞時的反應。
    
- 凸體和凹體碰撞器幾何形狀：PhysX 物理系統要求對於非運動學 Rigidbody 放置的任何碰撞器都必須是凸的，而非凹的。Unity 中的所有原始形狀都是凸的。然而，Unity 默認將 Mesh Collider 視為凹的。
    
- 若將默認的 Mesh Collider 應用於非運動學 Rigidbody，Unity 在運行時會產生錯誤。為確保非運動學 Rigidbody 接收基於物理的力量，需指示 Unity 使 Mesh Collider 成為凸體。啟用 Mesh Collider 的 Convex 屬性即可實現此目的。啟用 Convex 後，Unity 根據相關網格自動計算凸碰撞器形狀（稱為凸殼）。然而，由於網格的凸殼只是原始形狀的近似，可能導致模擬不準確。
    
- 為了進行更準確的碰撞模擬，可使用以下方法之一：
    
    - 使用 DCC 工具將網格分割為多個部分，這樣當 Unity 計算它們的凸殼時，它們更好地表示了總形狀。
        
    - 使用多個原始碰撞器手動構建與網格相同形狀的複合碰撞器。
        
    - 使用自動工具計算任何網格的凸近似，例如 Unity 的 V-HACD。
        
- 如果 Rigidbody 是運動學的（即它不接收基於物理的力量），則可以將任何碰撞器應用於它。