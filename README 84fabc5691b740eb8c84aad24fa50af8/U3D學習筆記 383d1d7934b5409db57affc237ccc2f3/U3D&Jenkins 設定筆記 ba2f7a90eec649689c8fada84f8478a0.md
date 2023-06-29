# U3D&Jenkins 設定筆記

## Unity 2020.3 與 Android Studio Command Line 注意事項

- 請安裝 JDK 版本 11.0.15
- 請安裝 Gradle 版本 6.1.1
- Gradle 6.1.1 對應的 Gradle Android Plugin 版本為 4.0.0
- Unity 2020.3.43 版本產生的 Android Project 預設使用以上版本，因此本機端安裝同樣版本才不會出問題 (同一個版本的 Gradle 搭配不同版本的 JDK 就有時會遇到錯誤)
- 操作命令：
    - GRANDLE_PATH 表示 Grable的安裝路徑 > (目前我電腦的位置是：C:\Users\Eric\.gradle\wrapper\dists\gradle-6.1.1-all\cfmwm155h49vnt3hynmlrsdst\gradle-6.1.1\bin)
    - PROJECT_PATH 表示 Ryu 的 Android Project 路徑 > (目前我電腦的位置是：C:\Users\Eric\Documents\Projects\client_eric\BatchBuild\Android\WS)
    - 記得將 GRANDLE_PATH 設定到 Windows 的環境變數，才能直接使用 grandle.bat 命令
    - 建置前，請先將 Working Directory 切換至 PROJECT_PATH
    - Cmd: grandle -v > 可以查詢目前所使用的版本與相關SDK版本 >
        
        ![gradle-version.png](U3D&Jenkins%20%E8%A8%AD%E5%AE%9A%E7%AD%86%E8%A8%98%20ba2f7a90eec649689c8fada84f8478a0/gradle-version.png)
        
    - Cmd: grandle launcher:assembleDebug > 執行專案建置 (Build Project)
    - Cmd: grandle clean > 執行專案清理 (Clean Project)

## Git Step Plugin：每個 Git 指令 Timeout 為十分鐘

Jenkins pipeline 中的 git step，對於 git 的指令上限執行時間 (timeout) 為 10 分鐘，超過就會被中斷。若遇專案過 + 大網速過慢，10 分鐘內抓不完是常有的。晴天霹靂的是，依官方文件說法 git step 這 Plugin 無法設定 Timeout。

![jenkins_git_step_doc.png](U3D&Jenkins%20%E8%A8%AD%E5%AE%9A%E7%AD%86%E8%A8%98%20ba2f7a90eec649689c8fada84f8478a0/jenkins_git_step_doc.png)

### 解決辦法

但文件中透漏 checkout step Plugin 可設定 Timeout，所以可改用 checkout step，方法如下： 

- 主要設定在 extensions: [[$class:‘CloneOption’, timeout:10], [$class:‘CheckoutOption’, timeout:10]]，將 CloneOption 與CheckoutOption 中的 timeout 設定成想要的時間長度。
- 記得要啟動LFS的功能，一樣在 extensions: [[$class:‘CloneOption’, timeout:10, lfs:true] 啟動。

## Jenkins 設定

- env.WORKSPACE，Jenkins為每個專案都有自己的資料目錄在 workspace 底下，而這個 workspace 資料夾一般位於 /.jenkins 底下。
- 呈上，善用這點將設定路徑綁在 /.jenkins 底下，這樣就可以達到懶人設定法，不用每次換電腦都要重新設定專案位置、Unity位置… etc。
- Pipeline Script 中 options { buildDiscarder logRotator }，可以用來設定封存的檔案、建置的檔案最多放幾個？或最多放幾天？就會被自動清除。