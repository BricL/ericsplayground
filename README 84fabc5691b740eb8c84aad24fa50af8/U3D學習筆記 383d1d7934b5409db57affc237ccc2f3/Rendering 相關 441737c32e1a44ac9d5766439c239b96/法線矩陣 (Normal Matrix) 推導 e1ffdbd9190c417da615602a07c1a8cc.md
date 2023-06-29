# 法線矩陣 (Normal Matrix) 推導

法線（Normal）在進行空間轉換，從 Object Space 至 World Space 時，為什麼不能直接使用 Model Matrix 呢？

- Model Matrix 只保證在三角平面上的 “Vertex” 轉換前後不變，無論是否有非對稱縮方。
- Notmal 與 Vertex 的不同在於，它不是在平面上。
- 若 Model Matrix 有非對稱縮放時，會導致轉換後的 Normal 不垂直於平面。

![Untitled](%E6%B3%95%E7%B7%9A%E7%9F%A9%E9%99%A3%20(Normal%20Matrix)%20%E6%8E%A8%E5%B0%8E%20e1ffdbd9190c417da615602a07c1a8cc/Untitled.png)

![截圖 2023-06-18 下午4.30.34.png](%E6%B3%95%E7%B7%9A%E7%9F%A9%E9%99%A3%20(Normal%20Matrix)%20%E6%8E%A8%E5%B0%8E%20e1ffdbd9190c417da615602a07c1a8cc/%25E6%2588%25AA%25E5%259C%2596_2023-06-18_%25E4%25B8%258B%25E5%258D%25884.30.34.png)

## 數學推導

[The Normal Matrix](https://www.lighthouse3d.com/tutorials/glsl-12-tutorial/the-normal-matrix/)

文章其中的一段描述：

$$
(GN)dot(MT)=(GN)^T*(MT)
$$

可以這樣寫的原因在於

![Untitled](%E6%B3%95%E7%B7%9A%E7%9F%A9%E9%99%A3%20(Normal%20Matrix)%20%E6%8E%A8%E5%B0%8E%20e1ffdbd9190c417da615602a07c1a8cc/Untitled%201.png)