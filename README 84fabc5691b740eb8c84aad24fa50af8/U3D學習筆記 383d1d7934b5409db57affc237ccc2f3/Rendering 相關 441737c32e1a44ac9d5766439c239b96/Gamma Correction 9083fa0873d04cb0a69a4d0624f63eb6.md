# Gamma Correction

---

## 人眼的感覺

- 過去陰極管射線的顯示裝置(電視、顯示器)，電壓與亮度非正比，電壓增加2倍並不會生成2倍的亮度，與物理現像不符。
- 對於顯示器來說，兩倍的輸入電壓不會帶來兩倍的亮度而是接近 exp 2.2。雖然在數學上不正確，但對人眼的感知來說是更接近自然的。我們可以透過觀察下面這張圖片來說明：
    
    ![Untitled](Gamma%20Correction%209083fa0873d04cb0a69a4d0624f63eb6/Untitled.png)
    
    上面是非線性的亮度顯示(從0.0~1.0)，比界接近人眼對於亮度的觀察感知。但回到物理正確性上，下面線性的亮度顯示(從0.0~1.0)才是正確的亮度顯示。雖然如此，正確的線性亮度卻帶來人類不自然的觀察感受。或者說，人眼對於暗部的感知強過於對亮部。因此，當前的顯示器大部分都還是保留以 exp 2.2 當作是亮度顯示的基準。
    

## 數學上的對錯

- 非線性關係如下：
    
    $$
    Device_{out} = f(color_{org}) = u{^{2.2}}
    \quad
    Device_{out} \in [0, 1] \quad
    u \in [0, 1]
    $$
    
    因此可知，顯示裝置在輸出色彩亮度上會與原本想要的亮度暗一些。
    
    ![Untitled](Gamma%20Correction%209083fa0873d04cb0a69a4d0624f63eb6/Untitled%201.png)
    
- 為了讓顏色顯示回歸線性，解法是將原要輸出顏色亮度進行調整，修正函數如下：

$$
Color_{gc} = f(color_{org}) = u^{\frac{1}{2.2}}
$$

- 呈上，經修正後的顏色亮度會比原本的亮，但由於顯示器本身自帶非線性關係會把亮度變暗，合成後就能回到線性關係，這就是俗稱的 Gamma Correction：
    
    $$
    l = {u^{2.2}} + u_i^{\frac{1}{2.2}}
    $$
    
    ![Untitled](Gamma%20Correction%209083fa0873d04cb0a69a4d0624f63eb6/Untitled%202.png)
    

## sRGB

- 一般我們常見的 sRGB 色彩空間上，所有的顏色亮度都已經過 Gamma Correction 校正後的結果。
    
    ![Untitled](Gamma%20Correction%209083fa0873d04cb0a69a4d0624f63eb6/Untitled%203.png)
    
- 呈上，由於 sRGB 色彩空間被廣泛地使用，因此我們在很多工具如 Photoshop、GIMP…等，預設的圖片儲存都屬於 sRBG 空間中，也因此都經過 Gamma Correction。

## 在 Shader 中的計算

- 我們在 Shader 中的數學計算基本上都是假設顏色在線性空間中
- 可想而知，如果一張貼圖從 Photoshop 而來，在取顏色的時候若不錯任何處理，我們拿到的顏色會是 Gamma Correction 的亮度結果並非線性。
- 呈上，這樣經過計算後所得到的結果當然會跟真實有所誤差，會呈現稍微暗了些，如下圖 (左（Gamma Space），右（Linear Space)所示：
    
    ![Untitled](Gamma%20Correction%209083fa0873d04cb0a69a4d0624f63eb6/Untitled%204.png)
    
- 現在新一代的遊戲引擎 (Untiy、Unreal、Godot) 都能幫你處理 Gamma Correction 的設定。如果開啟，在取出貼圖中顏色時會自動進行線性轉換，所以在 Shader 中取得的 color 會是線性的。而最後輸出 Color Buffer 至顯示裝置時會對整個結果進行 Gamma Correction，確保與顯示器的非線性關係合成後，結果會回到線性的正確關係上。
    
    ![Untitled](Gamma%20Correction%209083fa0873d04cb0a69a4d0624f63eb6/Untitled%205.png)
    

## 參考文獻

[Gamma、Linear、sRGB 和Unity Color Space，你真懂了吗？](https://zhuanlan.zhihu.com/p/66558476)

[LearnOpenGL - Gamma Correction](https://learnopengl.com/Advanced-Lighting/Gamma-Correction)