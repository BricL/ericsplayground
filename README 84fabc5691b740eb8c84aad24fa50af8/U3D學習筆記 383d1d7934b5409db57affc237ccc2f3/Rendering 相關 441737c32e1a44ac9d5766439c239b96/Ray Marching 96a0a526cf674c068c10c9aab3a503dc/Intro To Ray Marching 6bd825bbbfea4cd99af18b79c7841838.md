# Intro To Ray Marching

---

[https://www.youtube.com/watch?v=dSY_8Ii-HcI](https://www.youtube.com/watch?v=dSY_8Ii-HcI)

## 動手實作

依據 Ray Marching 101 文章中的做法，一樣我們使用 U3D 的 Custom Node 來實現相同的效果。

```glsl
// Using custom funcion node of shader graph
// function name: RayMarching
//
// currPos => Position Node (Absolute World Space)
// objCenter => Object Node (Position which is in world space)
// direction => View Direction Node
// radius => float，定義 Sphere 大小
// stepSize => float，定義每次跨出一步的大小
// steps => float，定義總步數

// Logic
float4 output = float4(0,0,0,1);
for(int currStep = 0; currStep < steps; currStep++)
{
	if (distance(currPos, objCenter) < radius)
	{
		output = float4(0,1,0,1);
		break;
	}
	else
	{
		currPos -= direction * stepSize;
	}
}
hitCol = output;
```

![截圖 2023-05-09 下午8.34.33.png](Intro%20To%20Ray%20Marching%206bd825bbbfea4cd99af18b79c7841838/%25E6%2588%25AA%25E5%259C%2596_2023-05-09_%25E4%25B8%258B%25E5%258D%25888.34.33.png)

![截圖 2023-05-08 下午10.01.35.png](Intro%20To%20Ray%20Marching%206bd825bbbfea4cd99af18b79c7841838/%25E6%2588%25AA%25E5%259C%2596_2023-05-08_%25E4%25B8%258B%25E5%258D%258810.01.35.png)

## U3D Shader Graph Node 說明

- View Direction Node, set World
    - 作為 direction 的輸入
    - 表示 “攝影機位置” 到 “每個 fragment” 的方向向量
    - 文件說明
        
        [View Direction Node | Shader Graph | 12.1.11](https://docs.unity3d.com/Packages/com.unity.shadergraph@12.1/manual/View-Direction-Node.html?q=View)
        
- Position Node, set Absolute World
    - 作為 curPos 的輸入
    - Absolute World 意思是輸入的世界座標不因使用不同的 scriptable rendering pipelin 而改變
    - 文件說明
        
        [Position Node | Shader Graph | 12.1.11](https://docs.unity3d.com/Packages/com.unity.shadergraph@12.1/manual/Position-Node.html?q=Position)
        
- Object Node, using Position
    - 作為 objCenter 的 輸入
    - 文件說明
        
        [Object Node | Shader Graph | 12.1.11](https://docs.unity3d.com/Packages/com.unity.shadergraph@12.1/manual/Object-Node.html?q=Object)
        

---

[BricL - Overview](https://github.com/BricL)