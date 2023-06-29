# HybridCLR 使用心得

![logo.jpg](HybridCLR%20%E4%BD%BF%E7%94%A8%E5%BF%83%E5%BE%97%20ff190360d2944b81b221f9b3c0ede8f1/logo.jpg)

[https://github.com/focus-creative-games/hybridclr](https://github.com/focus-creative-games/hybridclr)

HybridCLR 是 Unity 全平台原生 C# 熱更方案，具有特性完整、零成本、高性能、低內存等優點。使用時需要將業務邏輯分割成獨立的 DLL，並註意彼此引用問題。與其他純代碼的 Package/Plugin 相容性問題較少，效能損耗極低。

## 方案的優點

- 門檻低，開發人員無需學習其他語言
- 寫作方式不變，依 Untiy 慣用思路即可，專案維護容易
- 能直接使用 Unity 上許多次世代的開發流程（如：圖形式編程語言），無需想辦法與其他語言介接或放棄該部分的熱更能力

## 基礎使用流程

### 狀況1：第一次/有新增dll至專案

1. Generate > All
Generate 主要是把專案中，熱更/非熱更的 dll 分離出來。
2. CompileDll > ActiveBuildTarget
產生熱更的 dll 。
3. Build > BuildAssetsAndCopyToHotfixedGroup
將熱更新的 dll 相關檔案拷貝至 addressable 資料夾下

### 狀況2：一般熱更

1. CompileDll > ActiveBuildTarget
2. Build > BuildAssetsAndCopyToHotfixedGroup

### 狀況3：何時需重打新包？

1. 當使用到 `**新的 dll 時（之前在遊戲中沒有使用過的）`**
原包體將無法識別而導致呼叫相關 function 失敗。
2. 執行 HybridCLR > Generate > All 等同 Player Build
如果 Addressable > build > Build Addressables on Player Build 設定為 **ˋBuild content on Player Buildˋ，**這將導致 Addressable **ˋhash 版號ˋ** 強迫上升，而原包體認不得上升後的hash，導致無法下載新的 Assetbundle 內容。

![Untitled](HybridCLR%20%E4%BD%BF%E7%94%A8%E5%BF%83%E5%BE%97%20ff190360d2944b81b221f9b3c0ede8f1/Untitled.png)

![截圖 2023-05-07 下午4.05.13.png](HybridCLR%20%E4%BD%BF%E7%94%A8%E5%BF%83%E5%BE%97%20ff190360d2944b81b221f9b3c0ede8f1/%25E6%2588%25AA%25E5%259C%2596_2023-05-07_%25E4%25B8%258B%25E5%258D%25884.05.13.png)

## 與其他 Package/Plugin 相容性問題

先說結論：純代碼的 Package/Plugin 與 HybridCLR 的搭配應該是最佳方式，問題最少。

由於 HybridCLR 會對 AOT 進行一些 dll 的抽離等特殊動作，若是遇到有其他的 Package/Plugin 也對 AOT 進行特殊操作，高機率兩邊碰撞導致死機。目前遇到的是 Unity Visual Scripting，在每次進行 Player Build 時都會進行一些神秘的動作，然後才開始執行 HybridCLR 建置，然後死機。

## 我的實作策略

### Unity 專案結構

![ScriptCompilation.jpg](HybridCLR%20%E4%BD%BF%E7%94%A8%E5%BF%83%E5%BE%97%20ff190360d2944b81b221f9b3c0ede8f1/ScriptCompilation.jpg)

其中：

- Assembly-CSharp.dll 是根，遊戲的啟動 main 被包含在內。
- 其他 dll 依據專案本身使用的 Plugin 或 Assembly definition，分別被 Assembly-CSharp.dll 或彼此相互引用。

使用 HybridCLR 的訣竅只有一個： 

## “將業務邏輯分割成獨立dll，注意彼此引用問題”

官方教程中，建議初學者優先考慮以 Assembly-CSharp.dll 為熱更目標進行練習。這樣只需把業務邏輯都放在內，無須考慮多個 dll 間的相依性，整個換掉即完成。但這樣的缺點很明顯，所有業務邏輯都在 Assembly-CSharp.dll 中導致 Player Build 效率低下是可預期的，當在大型專案。

### 我領悟的實作策略

![我的 HybridCLR 實作策略.jpg](HybridCLR%20%E4%BD%BF%E7%94%A8%E5%BF%83%E5%BE%97%20ff190360d2944b81b221f9b3c0ede8f1/%25E6%2588%2591%25E7%259A%2584_HybridCLR_%25E5%25AF%25A6%25E4%25BD%259C%25E7%25AD%2596%25E7%2595%25A5.jpg)

這樣切分的好處如下：

- 最基礎的更新業務邏輯放在Assembly-CSharp.dll中，App 啟動第一件事情就是讓 Launcher 檢查是否有需要更新的 “資源及代碼”。
- 進入遊戲及啟動 Lobby.dll 及其他 Game.dll。
- 這樣的切分也將遊戲間業務邏輯分割得相當清楚，在出問題時只需針對問題的 dll 進行除錯。

## 效能損耗極低

相較於其他 Lua、iFix 方案（見[官方頁面](https://focus-creative-games.github.io/hybridclr/performance/#%E6%B5%8B%E8%AF%95%E6%8A%A5%E5%91%8A)）

![Untitled](HybridCLR%20%E4%BD%BF%E7%94%A8%E5%BF%83%E5%BE%97%20ff190360d2944b81b221f9b3c0ede8f1/Untitled%201.png)

![Untitled](HybridCLR%20%E4%BD%BF%E7%94%A8%E5%BF%83%E5%BE%97%20ff190360d2944b81b221f9b3c0ede8f1/Untitled%202.png)

## 同場加映

YT上找到一場演說特別比較 xLua、toLua、sLua 性能比較，也值得聽聽看後面的性能分析。

[https://www.youtube.com/watch?v=uZBmNzsPWwQ](https://www.youtube.com/watch?v=uZBmNzsPWwQ)

## 參考文獻

- HybridCLR 官網，請參照
    
    [关于HybridCLR | Code Philosophy](https://focus-creative-games.github.io/hybridclr/about/#文档)
    
- 關於 Addressable Remote Content Distribution 相關說明，請參照
    
    [](https://www.notion.so/HybridCLR-ff190360d2944b81b221f9b3c0ede8f1?pvs=4#c423b9f88b4541a4a680b626f1a33077)
    
    ![截圖 2023-05-07 下午7.21.43.png](HybridCLR%20%E4%BD%BF%E7%94%A8%E5%BF%83%E5%BE%97%20ff190360d2944b81b221f9b3c0ede8f1/%25E6%2588%25AA%25E5%259C%2596_2023-05-07_%25E4%25B8%258B%25E5%258D%25887.21.43.png)