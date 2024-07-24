锯齿形成的原因：
- 光栅化覆盖的像素点是离散的；
- 三角形面的边缘覆盖了像素中心点，像素被着色；
- 边缘没有覆盖到像素中心点，像素不被着色；
- 因为三角形面是离散的像素而非连续的像素，从而形成锯齿效果。

![image](https://github.com/user-attachments/assets/d593f30a-5b9c-4fd0-8686-b0ae3977fe5d)

***  
抗锯齿基本原理：
- 三角形面覆盖的像素，根据周围像素颜色进行渐变；
- 降低像素与周围颜色离散性，增加与周围颜色连续性；
- 从而降低锯齿效果。

![image](https://github.com/user-attachments/assets/1933b036-034f-478f-9945-f3fbcc48dd2b)

***  
实现抗锯齿2种思路：
- 思路1：多次采样一个像素，综合多次采样的结果，计算最终像素颜色。（MSAA，TAA）
- 思路2：通过后处理方式，寻找屏幕中像素块的边界，将边界两侧像素颜色进行插值，从而形成平滑过渡的边缘。（FXAA，SMAA）
*** 
四种主流抗锯齿方案：
- MSAA（多重采样抗锯齿）
- TAA（时间抗锯齿）
- FXAA（快速近似抗锯齿）
- SMAA（亚像素形态抗锯齿）
***  
