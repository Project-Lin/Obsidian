---
tags:
  - Unity
  - "#Doc"
  - Rigidbody
aliases: 
Home: "[[Unity Manual]]"
Previous: "[[00 Introduction to rigid body physics]]"
next: "[[02 Apply constant force to a Rigidbody]]"
---
# 設置 Rigidbody 的碰撞器

>碰撞器（Colliders）用於定義 Rigidbody 的物理邊界。若要允許碰撞發生，必須在 GameObject 上添加碰撞器並與 Rigidbody 一同使用。

- Rigidbody 碰撞器：碰撞器確定 Rigidbody 的物理邊界。如果一個 Rigidbody 與另一個碰撞，物理引擎僅在兩個 GameObject 都附有碰撞器時計算碰撞。若一個 GameObject 具有 Rigidbody 但沒有碰撞器，它會穿透其他 GameObject，Unity 不會將其納入碰撞計算。
    
- 碰撞物體的相對質量決定它們在碰撞時的反應。
    
- 凸體和凹體碰撞器幾何形狀：PhysX 物理系統要求對於非運動學 Rigidbody 放置的任何碰撞器都必須是凸的，而非凹的。Unity 中的所有原始形狀都是凸的。然而，Unity 默認將 Mesh Collider 視為凹的。
    
- 若將默認的 Mesh Collider 應用於非運動學 Rigidbody，Unity 在運行時會產生錯誤。為確保非運動學 Rigidbody 接收基於物理的力量，需指示 Unity 使 Mesh Collider 成為凸體。啟用 Mesh Collider 的 Convex 屬性即可實現此目的。啟用 Convex 後，Unity 根據相關網格自動計算凸碰撞器形狀（稱為凸殼）。然而，由於網格的凸殼只是原始形狀的近似，可能導致模擬不準確。
    
- 為了進行更準確的碰撞模擬，可使用以下方法之一：
    
    - 使用 DCC 工具將網格分割為多個部分，這樣當 Unity 計算它們的凸殼時，它們更好地表示了總形狀。
        
    - 使用多個原始碰撞器手動構建與網格相同形狀的複合碰撞器。
        
    - 使用自動工具計算任何網格的凸近似，例如 Unity 的 V-HACD。
        
- 如果 Rigidbody 是運動學的（即它不接收基於物理的力量），則可以將任何碰撞器應用於它。



