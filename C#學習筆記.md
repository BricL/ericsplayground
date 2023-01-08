## :sunny:C#  Reflection
.NET Framework 有二次編譯的概念，為了不同平台上使用，加上一層中間層，更靈活：

1. 透過VS編譯器　編譯成dll/exe
2. 點擊exe的時候，他有一個依賴的環境叫做CLR
3. dll/exe裡面包含兩大塊 IL（中間語言），metadata（元數據）      

metadata會紀錄這 dll/exe 裡面有哪些東西，而反射主要在做這塊。

### 優點：
1. 反射提高了程序的靈活性和擴展性。
2. 降低耦合性，提高自適應能力。
3. 它允許程式創建和控制任何類的物件，無需提前硬編碼目標類。
### 缺点：
1. 性能問題：使用反射基本上是一種解釋操作，用於字段和方法接入時要遠慢於直接代碼。 因此反射機制主要應用在對靈活性和拓展性要求很高的系統框架上，普通程式不建議使用。
2. 使用反射會模糊程序內部邏輯; 程式師希望在原始程式碼中看到程式的邏輯，反射卻繞過了原始程式碼的技術，因而會帶來維護的問題，反射代碼比相應的直接代碼更複雜。

### 參考資源：
* C# 反射（Reflection）
https://www.runoob.com/csharp/csharp-reflection.html
* 玩轉C#之【反射 Reflection 射爆妳】
https://ithelp.ithome.com.tw/articles/10288591?sc=iThelpR
* REFLECTION-使用反射執行方法的7種方式
https://blog.kkbruce.net/2017/01/reflection-method-invoke-7-ways.html
* C# - Reflection
https://www.tutorialspoint.com/csharp/csharp_reflection.htm   

---

<br>

[返回目錄](https://github.com/BricL/ericsplayground/blob/main/README.md)