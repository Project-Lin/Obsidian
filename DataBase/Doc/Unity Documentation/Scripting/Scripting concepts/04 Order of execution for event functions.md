---
tags:
  - Unity
  - "#Doc"
aliases: 
Home: "[[Unity Manual]]"
Previous: 
next:
---
>事件函數是一組內建事件，您的 MonoBehaviour 腳本可以通過實現相應的方法（通常稱為回調）來選擇性地訂閱這些事件。這些回調對應於核心 Unity 子系統中的事件，如物理、渲染和用戶輸入，或者對腳本自身生命周期的階段，如其創建、啟動、依賴幀和獨立幀更新，以及銷毀。當事件發生時，Unity 將在您的腳本上調用相應的回調，讓您有機會對事件做出反應。

>盡管 Unity 在預定的順序中引發這些事件並調用相應的 MonoBehaviour 回調，但該順序在這裡有所記錄。了解執行順序很重要，這樣您就不會試圖使用一個回調來執行依賴於尚未被調用的另一個回調的工作。但是，請注意，某些回調是針對事件的，例如由用戶輸入觸發的事件，在遊戲運行時可以在任何時間發生。您應該將此頁面與 MonoBehaviour 腳本參考（其中事件回調列在“消息”下）結合使用，以完全了解每個事件的含義和限制。

## 腳本生命周期概述

以下圖表概述了 Unity 如何在腳本生命周期內排序和重複事件函數。

對於各種事件函數的更多信息，請參閱以下各節：

- 一般原則
- 第一個場景加載
- 編輯器
- 第一個幀更新之前
- 幀間更新
- 更新順序
- 動畫更新循環
- 渲染
- 協程
- 當物體被銷毀時
- 退出時


##   腳本生命週期流程圖

![[Pasted image 20240402110725.png]]

## 一般原則

一般而言，您不應依賴於同一事件函數被調用的順序不同的 GameObjects，除非該順序被明確記錄或可設置。如果您需要對玩家循環進行更細粒度的控制，您可以使用 PlayerLoop API。您無法指定對同一 MonoBehaviour 子類的不同實例調用事件函數的順序。例如，一個 MonoBehaviour 的 Update 函數可能在另一個 GameObject 上的相同 MonoBehaviour 的 Update 函數之前或之後被調用——包括它自己的父或子 GameObjects。

您可以使用項目設置窗口中的“Script Execution Order”選項自定義腳本的執行順序。這使您可以控制哪些腳本會在其他腳本之前或之後執行。在添加多個場景時，配置的腳本執行順序會依次應用於每個場景，而不是跨場景部分應用，因此 EngineBehaviours 和 SteeringBehaviours 會在一個場景上都更新，然後才會在下一個場景上更新。

## 第一個場景加載

這些函數在場景開始時（對於場景中的每個對象）調用一次。

- Awake：在創建新對象的新實例時首先調用的生命周期函數。始終在任何 Start 函數之前調用。如果 GameObject 在啟動期間處於非活動狀態，則不會調用 Awake，直到將其激活為止。
- OnEnable：當對象啟用且活動時調用，始終在 Awake（在相同對象上）之後並在任何 Start 之前。

對於作為場景資源一部分的對象，Awake 和 OnEnable 函數的所有腳本都在 Start 和後續函數之前調用。但是，在運行時實例化對象時無法強制執行此操作。

在個別對象的範圍內，Awake 僅保證在 OnEnable 之前被調用。在多個對象上，順序不確定，您無法依賴於一個對象的 Awake 在另一個對象的 OnEnable 之前調用。任何依賴於所有對象的 Awake 已經被調用的工作都應該在 Start 中完成。

## 首次場景加載和卸載之前

上面的圖表未顯示 SceneManager.sceneLoaded 和 SceneManager.sceneUnloaded 事件，這些事件允許您在場景加載和卸載時接收回調。有關詳細信息和示例用法，請參閱相關的腳本參考頁面。您可以期望在所有對象的 Start 之後但在場景中的所有對象的 OnEnable 之前收到 sceneLoaded 通知。請參閱禁用域和場景重新加載的詳細信息，其中包含場景加載作為執行流程的一部分的圖表。

您還可以使用 RuntimeInitializeOnLoadMethodAttribute 及其類型 BeforeSceneLoad 和 AfterSceneLoad，在場景加載之前或之後運行您的方法。有關標記為這些類型的方法的執行順序信息，請參閱 RuntimeInitializeOnLoadMethodAttribute 腳本參考主頁。

## 編輯器

- Reset：當首次將腳本附加到對象時初始化腳本的屬性，以及使用 Reset 命令時也會調用。
- OnValidate：每當設置腳本的屬性時（包括對象反序列化時，可能在各種時間發生，例如在編輯器中打開場景以及域重新加載後），都會調用。

## 第一個幀更新之前

- Start：僅在腳本實例啟用時在第一個幀更新之前調用。

對於作為場景資源的對象，Start 函數會在任何腳本的 Update 之前調用。但是，這在遊戲過程中實例化對象時無法實現。例如，如果您在另一個對象的 Update 函數中實例化一個對象，則無法在原始對象的第一次 Update 執行之前調用實例化對象的 Start。

## 幀間更新

- OnApplicationPause：在偵測到暫停時，於當前帧結束時調用，實際上是在正常幀更新之間。在調用 OnApplicationPause 後會發出一個額外的幀，以便遊戲顯示指示暫停狀態的圖形。

## 更新順序

當您跟踪遊戲邏輯和交互、動畫、攝像機位置等時，您可以使用幾種不同的事件。常見的模式是在 Update 函數中執行大多數任務，但還有其他函數可以使用。

- FixedUpdate：以遊戲時間的固定間隔而不是每幀調用。由於這些更新是固定的，而幀率是可變的，因此在幀率高時可能在一個幀中沒有固定的更新，或者在幀率低時在一個幀中進行多個固定更新。所有物理計算和更新都在 FixedUpdate 之後立即進行，因為它是與幀率無關的，所以在計算在 FixedUpdate 中的移動時不需要將值乘以 Time.deltaTime。固定更新的間隔由 Time.fixedDeltaTime 定義，可以直接在腳本中設置，也可以通過編輯器的時間設置中的 Fixed Timestep 屬性進行設置。有關更多信息，包括用於確定是執行更新還是固定更新的時間計算，請參閱 Time。
    
- Update：每幀調用一次，是幀更新的主要函數。
    
- LateUpdate：在 Update 完成後每幀調用一次。在 LateUpdate 開始時，Update 中執行的任何計算都將完成。一個常見的用法是用於跟隨第三人稱相機。如果您使角色在 Update 中移動和轉向，您可以在 LateUpdate 中執行所有相機移動和旋轉計算。這將確保在相機跟踪其位置之前，角色已經完全移動。
    

## 動畫更新循環

上面流程圖中顯示的以下動畫循環回調是在衍生自 MonoBehaviour 的腳本上調用的：

- MonoBehaviour.OnAnimatorMove
- MonoBehaviour.OnAnimatorIK

在衍生自 StateMachineBehaviour 的腳本上會調用其他與動畫相關的事件函數：

- StateMachineBehaviour.OnStateMachineEnter
- StateMachineBehaviour.OnStateMachineExit
- StateMachineBehaviour.OnStateEnter
- StateMachineBehaviour.OnStateUpdate
- StateMachineBehaviour.OnStateExit
- StateMachineBehaviour.OnStateMove
- StateMachineBehaviour.OnStateIK

有關這些回調的含義和限制，請參閱相關的腳本參考頁面。

流程圖中顯示的其他動畫函數是內部於動畫系統中，並提供上下文。這些函數有相關的 Profiler 標記，因此您可以使用 Profiler 看到 Unity 何時調用它們。了解 Unity 何時調用這些函數可以幫助您確切了解您調用的事件函數何時被執行。有關動畫函數和分析器標記的完整執行順序，請參閱 Profiler 標記。

## 渲染

此執行順序僅適用於內建的渲染管線。有關基於 Scriptable Render Pipeline 的渲染管線的執行順序的詳細信息，請參閱 Universal Render Pipeline 或 High Definition Render Pipeline 的相關部分的文檔。如果您想在渲染之前立即執行工作，請參閱 Application.onBeforeRender。

以下是各種渲染相關事件函數的執行順序：

- OnPreCull：在攝像機剔除場景之前調用。剔除確定了攝像機可見的對象。OnPreCull 在剔除之前調用。
- OnBecameVisible/OnBecameInvisible：當對象對任何攝像機變得可見/不可見時調用。因為對象可能在任何時間變得不可見，所以未在上面的流程圖中顯示 OnBecameInvisible。
- OnWillRenderObject：如果對象可見，則每個攝像機調用一次。
- OnPreRender：在攝像機開始渲染場景之前調用。
- OnRenderObject：在所有常規場景渲染完成後調用。您可以在這一點上使用 GL 類或 Graphics.DrawMeshNow 繪製自定義幾何圖形。
- OnPostRender：在攝像機完成渲染場景後調用。
- OnRenderImage：在場景渲染完成後調用，以允許圖像的後處理。請參閱後處理效果。
- OnGUI：以響應 GUI 事件每幀多次調用。首先處理 Layout 和 Repaint 事件，然後處理每個輸入事件的 Layout 和鍵盤/鼠標事件。

請注意：OnPreCull、OnPreRender、OnPostRender 和 OnRenderImage 是內建的 Unity 事件函數，僅當這些腳本附加到與已啟用的 Camera 組件相同的對象時才調用。如果您想在 MonoBehaviour 附加到不同對象的情況下接收 OnPreCull、OnPreRender 和 OnPostRender 的等效回調，則必須使用相應的委託（請注意名稱中的小寫 on），如相應頁面的代碼示例所示。

## 協程

普通的協程更新是在 Update 函數返回後運行的。協程是一種可以暫停其執行（yield）直到給定的 YieldInstruction 完成的函數。

協程的不同使用方式：

- yield：在所有 Update 函數被調用之後，在下一幀繼續。
- yield WaitForSeconds：在指定的時間延遲後，在該幀的所有 Update 函數被調用之後繼續。
- yield WaitForFixedUpdate：在所有腳本上的 FixedUpdate 被調用之後繼續。如果協程在 FixedUpdate 之前被中斷，則它會在當前幀的 FixedUpdate 之後恢復。
- yield WWW：在 WWW 下載完成後繼續。
- yield StartCoroutine：鏈接協程，因此如果理論上的協程 coroutineA 啟動另一個協程 coroutineB，則 coroutineA 暫停並等待 coroutineB 完成後才繼續。有關示例，請參閱 MonoBehaviour.StartCoroutine。

## 當對象被銷毀時

- OnDestroy：該函數在對象的存在期的最後一幀的所有幀更新之後調用（對象可能是響應 Object.Destroy 或在場景關閉時被銷毀）。

## 退出時

這些函數在場景中的所有活動對象上調用：

- OnApplicationQuit：此函數在應用程序退出之前調用所有遊戲對象。在編輯器中，它在用戶停止播放模式時調用。
- OnDisable：當行為變得禁用或非活動時調用。