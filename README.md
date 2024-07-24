锯齿形成的原因：
- 光栅化覆盖的像素点是离散的；
- 三角形面的边缘覆盖了像素中心点，像素被着色；
- 边缘没有覆盖到像素中心点，像素不被着色；
- 因为三角形面是离散的像素而非连续的像素，从而形成锯齿。
***  
主流抗锯齿方案：
- MSAA（多重采样抗锯齿）
- TAA（时间抗锯齿）
- FXAA（快速近似抗锯齿）
- SMAA（亚像素形态抗锯齿）
***  
