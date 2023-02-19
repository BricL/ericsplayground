# 目錄：
* [初始化 TS 專案在 vscode](#1)  
* [NVM 讓電腦安裝多個版本的 NodeJS](#2)  
* [NodeJs SimpleHttpServer 套件](#3)
* [mySQL 套件](#4)
* [ORM 套件](#5)

<span id="1"></span>  

## :sunny:初始化 TS 專案在 vscode

>1. install nvm
>2. using nvm install nodeJS (Recommand stable version)
>3. mkdir folder
>4. ./code folder
>5. npm init
>6. npm install typescript --save-dev
   安裝 TS
>7. npm install @types/node --save-dev
   安裝 node 在 TS 下的 Type definition
>8. npx tsc -init
   初始化 TS 設定檔案，會動產生 tsconfig.json
>9. 設定 tsconfig.json：
>   - "sourceMap": true,
>   - "outDir": "./dst",
>   - "rootDir": "./src",
>10. 設定 package.json：
>    ```json
>    "scripts": {
>    	"build": "tsc",
>    	"start": "node ./dst/"
>    }
>    ```
>    這樣就可以執行：  
>    ```c
>    npm run build //執行TS編譯  
>    npm run start //啟動程序   
>    ```

<span id="2"></span>  

## :sunny:NVM 讓電腦安裝多個版本的 NodeJS

nodejs 有相當多的版本，在公司或團隊內可能會依專案的新、舊或客戶需求而採不同版本，如果使用官方表準安裝，就只能在一台電腦上安裝一個版本，遇到專案不同版本的狀況切換環境就會相當不便利，因此誕生了 NVM 套件。   

> 基本指令：
>
> * nvm install {{版本號}}，安裝指定版
> * nvm alias default {{版本號}}，設定預設開啟的版本
> * nvm use {{版本號}}，指定當前使用的版本
> * nvm list，列出本地端所安裝的版本
> * nvm list-remote，列出目前可用的遠端版本
>   <br><br>

### 遇到的問題
1. **Mac 10.15後，安裝需要注意**   

   Since macOS 10.15, the default shell is zsh and
   nvm will look for .zshrc to update, none is installed by default. Create one
   with touch ~/.zshrc and run the install script again.
2. **每次開啟新的 Console 時都需要重新設定 nvm use node，不然無法使用npm 或 node 指令**

   要每次開啟 console 都讓 nvm 自動設定好預設版本的 node，請設定以下指令：

   > nvm alias default node

   這樣每次開啟新的 console 時就不會認不得 node、npm 指令。<span style="color:red">如果在 vscode 在 debug 時遇到無法啟動，有很高的“機率”是這個沒有設定</span>。

### 參考資源

* 官方github  
[https://github.com/nvm-sh/nvm#installing-and-updating](https://github.com/nvm-sh/nvm#installing-and-updating)
* How do I install
multiple node js version on the same machine  
[https://www.loginradius.com/blog/engineering/run-multiple-nodejs-version-on-the-same-machine/](https://www.loginradius.com/blog/engineering/run-multiple-nodejs-version-on-the-same-machine/)

<span id="3"></span>  

## :sunny:NodeJs SimpleHttpServer 套件  
可在本地端簡單架設一個 Http server 並指定 host 的資料夾位置，可以用來模擬 CDN 讓 Unity 可以在本地端或 Local LAN 下，測試下載 AssetBundles 用。  

若安裝時進行“全域”安裝：
>npm install simplehttpserver -g  

這樣未來在 console 下只需要鍵入以下指令，就能啟動功能：  

>./simplehttpserver ~/temp

### 參考資源
* npm 套件頁面  
https://www.npmjs.com/package/simplehttpserver?activeTab=readme

<span id="4"></span>  

## :sunny:mySQL 套件  

<span id="5"></span>  

## :sunny:ORM 套件
---

<br>

[返回目錄](/README.md)
