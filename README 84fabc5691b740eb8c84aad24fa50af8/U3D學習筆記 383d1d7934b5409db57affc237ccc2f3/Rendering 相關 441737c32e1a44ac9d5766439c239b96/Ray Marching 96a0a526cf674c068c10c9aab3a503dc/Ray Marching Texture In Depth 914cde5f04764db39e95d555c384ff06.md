# Ray Marching Texture In Depth

---

[https://www.youtube.com/watch?v=EjCPr-bxI34&t=612s](https://www.youtube.com/watch?v=EjCPr-bxI34&t=612s)

## 重點解析

- 如果你跟我一樣一開始無法理解為什麼，可以參考下面這張圖片：
    
    ![截圖 2023-05-19 下午4.12.48.png](Ray%20Marching%20Texture%20In%20Depth%20914cde5f04764db39e95d555c384ff06/%25E6%2588%25AA%25E5%259C%2596_2023-05-19_%25E4%25B8%258B%25E5%258D%25884.12.48.png)
    
- 貼圖(Unreal Logo)為一張黑白的 Mask 貼圖，其中 "黑色” 代表與 Ray 並沒有相交，因此繼續往前偵測。“白色” 代表 Ray 與物體碰撞並返回該物體顏色。

![下載.png](Ray%20Marching%20Texture%20In%20Depth%20914cde5f04764db39e95d555c384ff06/%25E4%25B8%258B%25E8%25BC%2589.png)

- 產生透視感是因為：
    - View Direction 本來就是 “透視投影” 中打出。
    - 第 1 層擊中
    就會是原本 2D Plane 的 fragments，因此擊中的部分(白色)會如同我們一般在貼上貼圖模型的樣子。
    - 第 2 層擊中
    第一層沒有擊中的部分就會沿著 Ray 向前，而這個向前就會對於圖片上的 uv 進行類似 “透視” 的偏移，最終在同一視角下的透視位置下取得擊中的部分(白色)。
    - 第 N 層擊中
    第二層沒有擊中的部分就會沿著 Ray 向前，繼續沿著透視投影的方向向前偵測，同樣的再向前之後的 uv 也會進行偏移，取樣後看是否擊中。
    - 重複以上過程直到 “步數為0” 或 “擊中”

## 動手實作

依據 Ray Marching 101 文章中的做法，一樣我們使用 U3D 的 Custom Node 來實現相同的效果。

```glsl
// Using custom funcion node of shader graph
// function name: RayMarching
//
// rayDir => 連接View Direction Node，注意！要設定為Tangent Space。
// texSampler => 貼圖物件
// uv => 平面的uv座標
// stepSize => 每次跨進一步的距離

float3 rayDir = viewDir * -1; //反向，因為View Direction Node預設方向是，
                              //從Fragment到Camera。
float4 output = tex2D(texSampler, uv.xy);

for(int i=0; i<30; i++)
{
    if (output.r > 0.1 && output.g > 0.1 && output.b > 0.1)
    {
        float color = 1.0 -( (float)i / 30.0);
        output = color;
        break;
    }

    uv += rayDir * stepSize;
    output = tex2D(texSampler, uv.xy);
}
inputTex = output;
```

![截圖 2023-05-19 下午12.27.55.png](Ray%20Marching%20Texture%20In%20Depth%20914cde5f04764db39e95d555c384ff06/%25E6%2588%25AA%25E5%259C%2596_2023-05-19_%25E4%25B8%258B%25E5%258D%258812.27.55.png)

![截圖 2023-05-19 下午12.27.08.png](Ray%20Marching%20Texture%20In%20Depth%20914cde5f04764db39e95d555c384ff06/%25E6%2588%25AA%25E5%259C%2596_2023-05-19_%25E4%25B8%258B%25E5%258D%258812.27.08.png)

## U3D Shader Graph Node 說明

- View Direction Node
    - 作為 View Driection 的輸入，記得切換至 Tangent Space
- UV Node
    - 取得模型上的 uv 座標
- Texture 2D Asset Node
    - 作為 Texture 的輸入，貼圖作為一個黑白的 Mask。
    - 從 Shader Code 可知道其基本上只用一個 Channel，且這個 Channel 內的值代表與 Ray 是否已碰撞的 Threadhold。

---

[BricL - Repositories](https://github.com/BricL?tab=repositories)