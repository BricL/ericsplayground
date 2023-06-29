# Ray Marching

---

[https://www.youtube.com/watch?v=dSY_8Ii-HcI](https://www.youtube.com/watch?v=dSY_8Ii-HcI)

Ray marching is a method in computer graphics that renders 3D scenes by iteratively marching along a ray to determine the distance to the nearest surface. It's useful for rendering complex shapes like fractals and distance fields.

By defining a distance function that returns the nearest surface at a given point in space, a ray marching algorithm can be implemented to move along the ray, updating the position and distance at each step.

Ray marching is a powerful technique that can be parallelized for real-time rendering of complex scenes on modern hardware. It also supports various lighting models and effects like shadows and reflections.

Overall, ray marching is an important tool for artists and developers in computer graphics to render complex scenes efficiently.

## 翻譯翻譯

![Untitled](Ray%20Marching%2096a0a526cf674c068c10c9aab3a503dc/Untitled.png)

- 從眼睛(Camera)與畫面上的一個 fragment，成一條射線
- 沿射線一步一步向前偵測，看是否有”碰撞到物體”
    - 如果有，則進行著色
    - 如果沒有且尚未到達前進步數上限，則繼續前進
    - 若達步數上限，放棄著色或以黑色替代
- 碰撞到物體
    - 這裡的 “物體” 要拋開一般 Polygon 建模(由三角面組成得模型)的思維，否則會難以理解
    - 這裡的 “物體” 由數學公式所描述
    - 採用 SDF(signed distance functions) 進行偵測

![Untitled](Ray%20Marching%2096a0a526cf674c068c10c9aab3a503dc/Untitled%201.png)

一開始難懂的是 Ray 跟模型間的關系呢？思想還卡在場景上要有模型，且要跟每個物件上的每個三角面進行碰撞偵測，不然怎樣知道最近的點在哪？在 shader 不可能吧！

但其實在 Raymarching 裡，所有的物件基本上屬於 procedure object ，就是說用數學函數或多個數學函數的組合，去描述物件的形狀(外觀)。簡單的圓形、正方形還可以想像，若換成地形或石頭就很難了~

還是不懂嗎？看一下大神(Inigo Quilez)的 DEMO 比較能懂摟！

[https://www.youtube.com/watch?v=8--5LwHRhjk](https://www.youtube.com/watch?v=8--5LwHRhjk)

大神(Inigo Quilez)也提供了各種 “物體 的距離計算 (Primitives SDF)“ 及其代碼，可以參考：

[Inigo Quilez](https://iquilezles.org/articles/distfunctions/)

## 參考文獻

[Ray Marching 101](https://zhuanlan.zhihu.com/p/34494449)

[Signed Distance Field(基础篇)](https://zhuanlan.zhihu.com/p/93901692)

[Ray Marching and Signed Distance Functions](https://jamie-wong.com/2016/07/15/ray-marching-signed-distance-functions/)

[Inigo Quilez](https://iquilezles.org/articles/distfunctions/)

[Raymarching Distance Fields: Concepts and Implementation in Unity](https://adrianb.io/2016/10/01/raymarching.html)

[https://www.youtube.com/watch?v=Cp5WWtMoeKg](https://www.youtube.com/watch?v=Cp5WWtMoeKg)

---

[Intro To Ray Marching](Ray%20Marching%2096a0a526cf674c068c10c9aab3a503dc/Intro%20To%20Ray%20Marching%206bd825bbbfea4cd99af18b79c7841838.md)

[Ray Marching Texture In Depth](Ray%20Marching%2096a0a526cf674c068c10c9aab3a503dc/Ray%20Marching%20Texture%20In%20Depth%20914cde5f04764db39e95d555c384ff06.md)