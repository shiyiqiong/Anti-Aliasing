**锯齿形成的原因：**
- 光栅化覆盖的像素点是离散的；
- 三角形面的边缘覆盖了像素中心点，像素被着色；
- 边缘没有覆盖到像素中心点，像素不被着色；
- 因为三角形面是离散的像素而非连续的像素，从而形成锯齿效果。

![image](https://github.com/user-attachments/assets/d593f30a-5b9c-4fd0-8686-b0ae3977fe5d)

***  
**抗锯齿基本原理：**
- 三角形面覆盖的像素，根据周围像素颜色进行渐变；
- 降低像素与周围颜色离散性，增加与周围颜色连续性；
- 从而降低锯齿效果。

![image](https://github.com/user-attachments/assets/1933b036-034f-478f-9945-f3fbcc48dd2b)

***  
**实现抗锯齿2种思路：**
- 思路1：多次采样一个像素，综合多次采样的结果，计算最终像素颜色。（MSAA，TAA）
- 思路2：通过后处理方式，寻找屏幕中像素块的边界，将边界两侧像素颜色进行插值，从而形成平滑过渡的边缘。（FXAA，SMAA）
*** 
**四种主流抗锯齿方案：**
- MSAA（多重采样抗锯齿）
- TAA（时间抗锯齿）
- FXAA（快速近似抗锯齿）
- SMAA（亚像素形态抗锯齿）
***  
**MSAA（多重采样抗锯齿）原理：**
- 同一个像素进行多次亚像素采样；
- 将亚像素存储到更大面积的多重采样抗锯齿贴图上；
- 再将多重采样抗锯齿贴图渲染到摄像机渲染缓冲区；
- 硬件会直接用box filter，计算亚像素颜色平均值作为像素最终颜色。

**注意点：**
- 采样点优化：  
  - 亚像素一般共享像素中心采样颜色，不需要重复采样；
  - 如果像素中心没有被三角形面覆盖，GPU挑选最近通过覆盖测试的亚像素作为采样点。
- 多重采样抗锯齿贴图内存占用：
  - 例如4次多重采样抗锯齿贴图，是普通贴图内存占用4倍；
  - 因此不适用在延迟渲染路径上，因为原本延迟渲染的GBuffer已经占用很多显存。
- HDR（高动态范围）：
  - 在高动态范围，亮度差别巨大的边缘，会导致抗锯齿效果不佳。
  - 处理方案：先将高动态范围颜色映射到标准动态范围，进行多重采样抗锯齿后，再将标准动态范围还原到高动态范围，继续后续的渲染。
***  
**TAA（时间抗锯齿）原理：**
- 与多重采样空锯齿原理类似；
- 对其时间上和空间上进行了优化；
- 时间上：将多次采样分摊到多帧中；
- 空间上：只缓存上一帧颜色混合结果，用于与当前帧进行混合计算。
- 
**注意点**
- 重投影：
  - 物体静态，摄像机移动；
  - 通过当前帧深度信息，转换为世界空间坐标（物体静止，世界空间不变）；
  - 将世界空间坐标，通过上一帧投影矩阵转换为，找到上一帧对应投影位置。 
- 动态物体：
  - Motion Vector贴图（运动矢量贴图），记录物体上一帧在屏幕投影空间的偏移值；
  - 通过偏移值，可以找到上一帧对应投影位置。
  - 没有记录到运动矢量贴图，做不了动态物体时间抗锯齿：粒子烟雾、水流、半透明物体。
- HDR（高动态范围）：
  - 在高动态范围，亮度差别巨大的边缘，会导致抗锯齿效果不佳。
  - 处理方案：先将高动态范围颜色映射到标准动态范围，进行时间抗锯齿，再将标准动态范围还原到高动态范围，继续后续的渲染。
***  
**FXAA（快速近似抗锯齿）原理：**
*** 
**SMAA（亚像素形态抗锯齿）原理：**
*** 
**参考资料：**

https://zhuanlan.zhihu.com/p/415087003

https://zhuanlan.zhihu.com/p/425233743

https://zhuanlan.zhihu.com/p/431384101

https://zhuanlan.zhihu.com/p/342211163
