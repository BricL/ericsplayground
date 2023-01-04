## 初始化 TS 專案在 vscode

1. install nvm
2. using nvm install nodeJS (Recommand stable version)
3. mkdir folder
4. ./code folder
5. npm init
6. npm install typescript --save-dev
   安裝 TS
7. npm install @types/node --save-dev
   安裝 node 在 TS 下的 Type definition
8. npx tsc -init
   初始化 TS 設定檔案，會動產生 tsconfig.json
9. 設定 tsconfig.json：
   - "sourceMap": true,
   - "outDir": "./dst",
   - "rootDir": "./src",  
10. 設定 package.json：

```json
	  "scripts": {
	    "test": "echo \"Error: no test specified\" && exit 1",
	    "build": "tsc",
	    "start": "node ./dst/"
	  },
```
