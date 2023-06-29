# Unity Anti-Aliasing 梳理

![Untitled](Unity%20Anti-Aliasing%20%E6%A2%B3%E7%90%86%20f171cf6291254c0081b15b97d7dc97e0/Untitled.png)

---

## Unity 支援的 AA 列表分類

- Hardware AA
    - MSAA (Multi-Sampling Anti-Aliasing)，多重采样抗锯齿
- Post-processing AA
    - FXAA (Fast Approximate Anti-Aliasing)，快速近似抗锯齿
    - SMAA (Enhanced Subpixel Morphological Anti-Aliasing)，增强型子像素形态抗锯齿
    - TAA (Temporal AA)，时域抗锯齿

## Unity 的 AA 列表用技術分類

- 超采样方法 (Super Sampling)
    - 影像以高解析度渲染，然後使用縮小過程（down-sampling）將其縮小至目標解析度。這樣做的好處是可以捕捉更多的細節，減少鋸齒狀效應並提升影像品質。經典方法：
        - MSAA
- 空域超采样方法 (Spatial Super Sampling)
    - 通过分析图像的边缘，例如几何边缘、阴影边缘等，来解决边缘锯齿问题的技术，經典方法：
        - FXAA
        - SMAA
- 时域超采样方法 (Temporal Super Sampling)
    - 基于时间的抗锯齿方法，简单来说就是混合相邻多个帧的结果，达到抗锯齿的效果，能同时缓解几何锯齿和着色锯齿。經典方法：
        - TAA，Only support in HDRP

## Render pipeline 列表

| Render pipeline | Post-processing support |
| --- | --- |
| https://docs.unity3d.com/Manual/SL-RenderPipeline.htmlRP | The Built-in Render Pipeline does not include a post-processing solution by default. To use post-processing effects with the Built-in Render Pipeline, download the https://docs.unity3d.com/Packages/com.unity.postprocessing@latest package. For information on using post-processing effects in the Built-in Render Pipeline, see the https://docs.unity3d.com/Packages/com.unity.postprocessing@latest documentation. |
| https://docs.unity3d.com/Packages/com.unity.render-pipelines.universal@latest/index.html | URP includes its own post-processing solution, which Unity installs when you create a Project using a URP Template. For information on using post-processing effects in URP, see the https://docs.unity3d.com/Packages/com.unity.render-pipelines.universal@latest/index.html?subfolder=/manual/integration-with-post-processing.html. |
| https://docs.unity3d.com/Packages/com.unity.render-pipelines.high-definition@latest/index.html?preview=1 | HDRP includes its own post-processing solution, which Unity installs when you create a Project using an HDRP Template. For information on using post-processing effects in HDRP, see the https://docs.unity3d.com/Packages/com.unity.render-pipelines.high-definition@latest/index.html?subfolder=/manual/Post-Processing-Main.html. |

> Unity目前有三種Render pipeline，每種有自己的AA設定，支援的種類也有一點點不同，要特別注意。
> 

## 各種Pipeline支持的方法

| Description | Available in URP integrated solution? | Available in HDRP integrated solution? | Available in PPv2 package? |
| --- | --- | --- | --- |
| The Anti-aliasing effect softens the appearance of edges in your scene.Depending on your render pipeline, you can use MSAA (hardware anti-aliasing), or FXAA, SMAA, or TAA (anti-aliasing post-processing effects) | Yes

FXAA and SMAA can be enabled in the https://docs.unity3d.com/Packages/com.unity.render-pipelines.universal@latest?subfolder=/manual/camera-component-reference.html#renderingcomponent.

You can also configure MSAA (hardware anti-aliasing) in https://docs.unity3d.com/Manual/class-QualitySettings.html | Yes

FXAA, SMAA, and TAA are implemented in Project Settings > Frame Settings > HDRP Default Settings; see https://docs.unity3d.com/Packages/com.unity.render-pipelines.high-definition@latest?subfolder=/manual/Anti-Aliasing.html?q=ANTI

You can also configure MSAA (hardware anti-aliasing) in the HDRP Asset - see https://docs.unity3d.com/Packages/com.unity.render-pipelines.high-definition@latest?subfolder=/manual/Anti-Aliasing.html?q=ANTI. | Yes

For FXAA, SMAA, and TAA, see https://docs.unity3d.com/Packages/com.unity.postprocessing@latest?subfolder=/manual/Anti-aliasing.html

You can also configure MSAA (hardware anti-aliasing) in https://docs.unity3d.com/Manual/class-QualitySettings.html |

2020之後，URP 的 MSAA 設定被放在 Universe Render Pipeline Asset 中的 Quality，而 FXAA 及SMAA 的設定是在 Camera Component中，如下圖：

![*MSAA 設定*](Unity%20Anti-Aliasing%20%E6%A2%B3%E7%90%86%20f171cf6291254c0081b15b97d7dc97e0/Untitled%201.png)

*MSAA 設定*

![*FXAA 及MSAA 的設定*](Unity%20Anti-Aliasing%20%E6%A2%B3%E7%90%86%20f171cf6291254c0081b15b97d7dc97e0/Untitled%202.png)

*FXAA 及MSAA 的設定*

### URP 下 AA 的設定組合可以有

- MSAA + Non
- MSAA + FXAA
- MSAA + SMAA
- Non + FXAA
- Non + SMAA

## 各種AA基本原理

### MSAA

在 Rasterization 時，三角所產生的每個 pixel 對其進行 Sub-Pixel 的覆蓋與深度(為了決定當有複數個三角形覆蓋相同 Sub-Pixel 時由誰佔據)測試。而被佔據的 Sub-Pixel 的顏色計算只會有一次 Pixel Shader 計算，而 Pixel 最終的顏色，由其內的所有 Sub-Pixel 進行顏色平均。

由上述可知，相對於 Super Sampling 暴力的將畫面提升解析度 (x2 x4 x8) 進行渲染，在縮小至原目標解析度，在 Pixel Shader 上的計算量差距就出來了。簡單想，原本計算 800 x 600 次的 pixel shader 計算，提升解析度後變成計算 1600 x 1200 次。

![Untitled](Unity%20Anti-Aliasing%20%E6%A2%B3%E7%90%86%20f171cf6291254c0081b15b97d7dc97e0/Untitled%203.png)

![*Results from non-MSAA and 4x MSAA rendering when a triangle partially covers a pixel. Based on an image from Real-Time Rendering, 3rd Edition*](Unity%20Anti-Aliasing%20%E6%A2%B3%E7%90%86%20f171cf6291254c0081b15b97d7dc97e0/Untitled%204.png)

*Results from non-MSAA and 4x MSAA rendering when a triangle partially covers a pixel. Based on an image from Real-Time Rendering, 3rd Edition*

![*Supersampling applied to a rasterized triangle, using various sub-pixel patterns. Notice how aliasing is reduced as the sample rate increases, even though the number of pixels is the same in all cases. Image from Real-Time Rendering, 3rd Edition, A K Peters 2008*](Unity%20Anti-Aliasing%20%E6%A2%B3%E7%90%86%20f171cf6291254c0081b15b97d7dc97e0/Untitled%205.png)

*Supersampling applied to a rasterized triangle, using various sub-pixel patterns. Notice how aliasing is reduced as the sample rate increases, even though the number of pixels is the same in all cases. Image from Real-Time Rendering, 3rd Edition, A K Peters 2008*

![*An image from the D3D10 documentation detailing the results of rasterizing various primitives with 4xMSAA.*](Unity%20Anti-Aliasing%20%E6%A2%B3%E7%90%86%20f171cf6291254c0081b15b97d7dc97e0/Untitled%206.png)

*An image from the D3D10 documentation detailing the results of rasterizing various primitives with 4xMSAA.*

### FXAA

FXAA（Fast Approximate Anti-Aliasing）由 [Nvidia](https://zh.wikipedia.org/wiki/Nvidia) 員工 Timothy Lottes 開發的[反鋸齒](https://zh.wikipedia.org/wiki/%E5%8F%8D%E9%8B%B8%E9%BD%92)算法，被套用在 Post-processing 最後一個階段，也就是在轉換成LDR與sRBG之後使用。優點是效能快、記憶體少、實作簡單、硬體支持度廣，缺點則是物體在邊緣銳利度上打了折扣，能調整的參數不多，比起其他 AA 的品質上只能算普通。

在與 UI/HUD 整合方面，最好是在畫 UI/HUD 之前進行，否則 UI 圖片周圍也會受 AA 影響變得模糊，如下圖所示:

![在 Unity 中將 Canvas 設定為Screen Space (Camera)，啟動 FSAA 來觀察對UI造成的影響。](Unity%20Anti-Aliasing%20%E6%A2%B3%E7%90%86%20f171cf6291254c0081b15b97d7dc97e0/Untitled%207.png)

在 Unity 中將 Canvas 設定為Screen Space (Camera)，啟動 FSAA 來觀察對UI造成的影響。

演算法步驟描述如下:

![截圖 2023-06-10 下午4.07.09.png](Unity%20Anti-Aliasing%20%E6%A2%B3%E7%90%86%20f171cf6291254c0081b15b97d7dc97e0/%25E6%2588%25AA%25E5%259C%2596_2023-06-10_%25E4%25B8%258B%25E5%258D%25884.07.09.png)

> 
> 
> 1. FXAA takes as input non-linear RGB color data which it converts internally
> into a scalar estimate of luminance for shader logic.
> 2. FXAA checks local contrast to avoid processing non-edges. Detected edges are
> in red, with blending towards yellow to represent the amount of detected subpixel aliasing (drawn using FXAA_DEBUG_PASSTHROUGH shader define).
> 3. Pixels passing the local contrast test are then classified as horizontal, in gold, or
> vertical, in blue (FXAA_DEBUG_HORZVERT).
> 4. Given edge orientation, the highest contrast pixel pair 90 degrees to the edge is
> selected, in blue/green (FXAA_DEBUG_PAIR).
> 5. The algorithm searches for end-of-edge in both the negative and positive,
> red/blue, directions along the edge. Checking for a significant change in average
> luminance of the high contrast pixel pair along the edge
> (FXAA_DEBUG_NEGPOS).
> 6. Given the ends of the edge, pixel position on the edge is transformed into to a
> sub-pixel shift 90 degrees perpendicular to the edge to reduce the aliasing,
> red/blue for -/+ horizontal shift and gold/skyblue for -/+ vertical shift
> (FXAA_DEBUG_OFFSET).
> 7. The input texture is re-sampled given this sub-pixel offset.
> 8. Finally a lowpass filter is blended in depending on the amount of detected subpixel aliasing

個人的解讀：

1. 將顏色轉乘 Luma。
2. 從 Luma 找出邊界。
3. 從邊界判斷是 “水平” 或 “垂直”。
    
    ```glsl
    // Compute an estimation of the gradient along the horizontal and vertical axis.
    float edgeHorizontal =	abs(-2.0 * lumaLeft + lumaLeftCorners)	+ abs(-2.0 * lumaCenter + lumaDownUp ) * 2.0	+ abs(-2.0 * lumaRight + lumaRightCorners);
    float edgeVertical =	abs(-2.0 * lumaUp + lumaUpCorners)		+ abs(-2.0 * lumaCenter + lumaLeftRight) * 2.0	+ abs(-2.0 * lumaDown + lumaDownCorners);
    	
    // Is the local edge horizontal or vertical ?
    bool isHorizontal = (edgeHorizontal >= edgeVertical);
    ```
    
4. 依水平方（或垂直）向的兩端（正、負）移動，垂直取樣上或下（若為水平方向則取樣左或右）進行計算，確認是否已到達方向的末端，若 ”否“ 則繼續移動直到迴圈上限或到達邊界。
    
    ```glsl
    for(int i = 2; i < ITERATIONS; i++){
    		// If needed, read luma in 1st direction, compute delta.
    		if(!reached1){
    			lumaEnd1 = rgb2luma(textureLod(sampler2D(screenTexture, sClampLinear), uv1, 0.0).rgb);
    			lumaEnd1 = lumaEnd1 - lumaLocalAverage;
    		}
    		// If needed, read luma in opposite direction, compute delta.
    		if(!reached2){
    			lumaEnd2 = rgb2luma(textureLod(sampler2D(screenTexture, sClampLinear), uv2, 0.0).rgb);
    			lumaEnd2 = lumaEnd2 - lumaLocalAverage;
    		}
    		// If the luma deltas at the current extremities is larger than the local gradient, we have reached the side of the edge.
    		reached1 = abs(lumaEnd1) >= gradientScaled;
    		reached2 = abs(lumaEnd2) >= gradientScaled;
    		reachedBoth = reached1 && reached2;
    			
    		// If the side is not reached, we continue to explore in this direction, with a variable quality.
    		if(!reached1){
    			uv1 -= offset * QUALITY(i);
    		}
    		if(!reached2){
    			uv2 += offset * QUALITY(i);
    		}
    			
    		// If both sides have been reached, stop the exploration.
    		if(reachedBoth){ break;}
    }	
    ```
    
5. 承上，依照最後的位置計算”偏移量“。
    
    ```glsl
    // Compute the distances to each side edge of the edge (!).
    float distance1 = isHorizontal ? (In.uv.x - uv1.x) : (In.uv.y - uv1.y);
    float distance2 = isHorizontal ? (uv2.x - In.uv.x) : (uv2.y - In.uv.y);
    	
    // In which direction is the side of the edge closer ?
    bool isDirection1 = distance1 < distance2;
    float distanceFinal = min(distance1, distance2);
    	
    // Thickness of the edge.
    float edgeThickness = (distance1 + distance2);
    	
    // Is the luma at center smaller than the local average ?
    bool isLumaCenterSmaller = lumaCenter < lumaLocalAverage;
    	
    // If the luma at center is smaller than at its neighbour, the delta luma at each end should be positive (same variation).
    bool correctVariation1 = (lumaEnd1 < 0.0) != isLumaCenterSmaller;
    bool correctVariation2 = (lumaEnd2 < 0.0) != isLumaCenterSmaller;
    	
    // Only keep the result in the direction of the closer side of the edge.
    bool correctVariation = isDirection1 ? correctVariation1 : correctVariation2;
    
    // UV offset: read in the direction of the closest side of the edge.
    float pixelOffset = - distanceFinal / edgeThickness + 0.5;
    	
    // If the luma variation is incorrect, do not offset.
    float finalOffset = correctVariation ? pixelOffset : 0.0;
    ```
    
6. 計算 Sub-Pixel Shifting，其意思就是計算取樣中心點之周圍Luma平均，周圍平均Luma 減掉 取樣中心 Luma，被以中心點及其周圍 Luma 共（3x3）取樣點中最大與最小間的距離去除。
    
    ```glsl
    // Sub-pixel shifting
    // Full weighted average of the luma over the 3x3 neighborhood.
    float lumaAverage = (1.0/12.0) * (2.0 * (lumaDownUp + lumaLeftRight) + lumaLeftCorners + lumaRightCorners);
    // Ratio of the delta between the global average and the center luma, over the luma range in the 3x3 neighborhood.
    float subPixelOffset1 = clamp(abs(lumaAverage - lumaCenter)/lumaRange,0.0,1.0);
    float subPixelOffset2 = (-2.0 * subPixelOffset1 + 3.0) * subPixelOffset1 * subPixelOffset1;
    // Compute a sub-pixel offset based on this delta.
    float subPixelOffsetFinal = subPixelOffset2 * subPixelOffset2 * SUBPIXEL_QUALITY;
    ```
    
7. 比較 4、5 兩個偏移量，看誰比較大就用誰。
8. 進行取樣，作為最終顏色輸出。

心得：感覺上，直接計算 5 （sub-pixel shifting），也可以有一定的效果存在。

Nvidia 原始文件PDF與代碼：

[Rendu/fxaa.frag at master · kosua20/Rendu](https://github.com/kosua20/Rendu/blob/master/resources/common/shaders/screens/fxaa.frag)

[NVIDIA FXAA 3.11 by TIMOTHY LOTTES](https://gist.github.com/kosua20/0c506b81b3812ac900048059d2383126#file-gistfile1-txt-L343)

[Shadertoy](https://www.shadertoy.com/view/ttXGzn)

[https://developer.download.nvidia.com/assets/gamedev/files/sdk/11/FXAA_WhitePaper.pdf](https://developer.download.nvidia.com/assets/gamedev/files/sdk/11/FXAA_WhitePaper.pdf)

### SMAA

To be continued…

### TAA

[https://www.youtube.com/watch?v=m-h_VUQJIvE](https://www.youtube.com/watch?v=m-h_VUQJIvE)

To be continued…

## 參考文獻

[A Quick Overview of MSAA](https://mynameismjp.wordpress.com/2012/10/24/msaa-overview/)

[抗锯齿相关技术介绍：MSAA、FXAA、SMAA、TXAA、MSAA_慕木子的博客-CSDN博客](https://blog.csdn.net/MumuziD/article/details/105672823)