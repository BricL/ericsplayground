## :sunny:UnityCICD

### Unity 2020.3 與 Android Studio Command Line 注意事項

* 請安裝 JDK 版本 11.0.15
* 請安裝 Gradle 版本 6.1.1
* Gradle 6.1.1 對應的 Gradle Android Plugin 版本為 4.0.0
* Unity 2020.3.43 版本產生的 Android Project 預設使用以上版本，因此本機端安裝同樣版本才不會出問題 (同一個版本的 Gradle 搭配不同版本的 JDK 就有時會遇到錯誤)
* 操作命令：

  * GRANDLE_PATH 表示 Grable的安裝路徑
    > (目前我電腦的位置是：C:\Users\Eric\.gradle\wrapper\dists\gradle-6.1.1-all\cfmwm155h49vnt3hynmlrsdst\gradle-6.1.1\bin)
  * PROJECT_PATH 表示 Ryu 的 Android Project 路徑
    > (目前我電腦的位置是：C:\Users\Eric\Documents\Projects\MGDR\megido_client_eric\BatchBuild\Android\Ryu)
  * 記得將 GRANDLE_PATH 設定到 Windows 的環境變數，才能直接使用 grandle.bat 命令
  * 建置前，請先將 Working Directory 切換至 PROJECT_PATH
  * Cmd: grandle -v
    > 可以查詢目前所使用的版本與相關SDK版本
    > ![](/images/gradle-version.png)
  * Cmd: grandle launcher:assembleDebug
    > 執行專案建置 (Build Project)
  * Cmd: grandle clean
    > 執行專案清理 (Clean Project)

---

<br>

[返回目錄](/README.md)
