# 第五章 图形变换

> **期中/期末考试分值：12分**（计算题6分 + 矢量运算6分）

---

## 1. 矢量运算基础【高频考点】

### 1.1 矢量的基本概念

**定义：**
矢量（向量）是既有大小又有方向的量，在计算机图形学中用于表示位置、方向、速度等。

**表示方法：**
- 二维矢量：$\vec{v} = (x, y)$ 或 $\vec{v} = \begin{pmatrix} x \\ y \end{pmatrix}$
- 三维矢量：$\vec{v} = (x, y, z)$ 或 $\vec{v} = \begin{pmatrix} x \\ y \\ z \end{pmatrix}$

### 1.2 矢量的基本运算

#### 矢量加法

**公式：**
$$\vec{a} + \vec{b} = (a_x + b_x, a_y + b_y, a_z + b_z)$$

**几何意义：** 平行四边形法则或三角形法则，表示位移的叠加。

#### 矢量减法

**公式：**
$$\vec{a} - \vec{b} = (a_x - b_x, a_y - b_y, a_z - b_z)$$

**几何意义：** 从点B指向点A的向量，常用于计算两点间的方向。

#### 数乘运算

**公式：**
$$k\vec{v} = (kv_x, kv_y, kv_z)$$

**几何意义：**
- $k > 1$：矢量伸长
- $0 < k < 1$：矢量缩短
- $k < 0$：矢量反向

### 1.3 矢量长度与单位化

#### 矢量长度（模）

**公式：**
$$|\vec{v}| = \sqrt{v_x^2 + v_y^2 + v_z^2}$$

#### 单位化（归一化）

**公式：**
$$\hat{v} = \frac{\vec{v}}{|\vec{v}|} = \left(\frac{v_x}{|\vec{v}|}, \frac{v_y}{|\vec{v}|}, \frac{v_z}{|\vec{v}|}\right)$$

**性质：** 单位向量的模为1，即 $|\hat{v}| = 1$

### 1.4 点积（内积/数量积）【重点】

**公式：**
$$\vec{a} \cdot \vec{b} = a_x b_x + a_y b_y + a_z b_z = |\vec{a}||\vec{b}|\cos\theta$$

**几何意义：**
- 表示两个矢量的相似程度
- $\theta$ 为两矢量夹角

**夹角计算：**
$$\cos\theta = \frac{\vec{a} \cdot \vec{b}}{|\vec{a}||\vec{b}|}$$

**应用场景：**
- 判断两向量是否垂直：$\vec{a} \cdot \vec{b} = 0$
- 计算投影长度：$proj_{\vec{b}}\vec{a} = \frac{\vec{a} \cdot \vec{b}}{|\vec{b}|}$
- 光照计算中的漫反射强度

### 1.5 叉积（外积/矢量积）【重点】

**公式：**
$$\vec{a} \times \vec{b} = \begin{vmatrix} \vec{i} & \vec{j} & \vec{k} \\ a_x & a_y & a_z \\ b_x & b_y & b_z \end{vmatrix} = (a_y b_z - a_z b_y, a_z b_x - a_x b_z, a_x b_y - a_y b_x)$$

**模的计算：**
$$|\vec{a} \times \vec{b}| = |\vec{a}||\vec{b}|\sin\theta$$

**几何意义：**
- 结果是一个垂直于 $\vec{a}$ 和 $\vec{b}$ 所在平面的矢量
- 模等于以 $\vec{a}$ 和 $\vec{b}$ 为边的平行四边形面积
- 方向由右手定则确定

**应用场景：**
- 计算平面法向量
- 判断点在直线的哪一侧
- 计算三角形面积


---

## 2. 二维图形几何变换【重点算法】

### 2.1 齐次坐标系统

#### 引入原因

平移、比例和旋转等变换的组合变换处理形式不统一，难以级联。齐次坐标技术将变换统一为矩阵乘法形式。

#### 基本思想

把一个n维空间中的几何问题转换到n+1维空间中解决。

#### 齐次坐标表示

**二维点的齐次坐标：**
$$(x, y) \rightarrow (x, y, 1)$$

**三维点的齐次坐标：**
$$(x, y, z) \rightarrow (x, y, z, 1)$$

**规格化：** 当 $w \neq 0$ 时，$(wx, wy, w)$ 表示二维点 $(x, y)$

**无穷远点：** 当 $w = 0$ 时，$(x, y, 0)$ 表示无穷远点

### 2.2 平移变换（Translation）【高频考点】

**几何关系：**
$$\begin{cases} x' = x + T_x \\ y' = y + T_y \end{cases}$$

**齐次坐标矩阵形式：**
$$[x', y', 1] = [x, y, 1] \begin{bmatrix} 1 & 0 & 0 \\ 0 & 1 & 0 \\ T_x & T_y & 1 \end{bmatrix}$$

**平移变换矩阵：**
$$T(T_x, T_y) = \begin{bmatrix} 1 & 0 & 0 \\ 0 & 1 & 0 \\ T_x & T_y & 1 \end{bmatrix}$$

**性质：**
- $T_x > 0$：向右平移
- $T_x < 0$：向左平移
- $T_y > 0$：向上平移
- $T_y < 0$：向下平移

### 2.3 比例/缩放变换（Scale）【高频考点】

**几何关系：**
$$\begin{cases} x' = x \cdot S_x \\ y' = y \cdot S_y \end{cases}$$

**齐次坐标矩阵形式：**
$$[x', y', 1] = [x, y, 1] \begin{bmatrix} S_x & 0 & 0 \\ 0 & S_y & 0 \\ 0 & 0 & 1 \end{bmatrix}$$

**缩放变换矩阵：**
$$S(S_x, S_y) = \begin{bmatrix} S_x & 0 & 0 \\ 0 & S_y & 0 \\ 0 & 0 & 1 \end{bmatrix}$$

**性质：**
- $S_x = S_y$：等比缩放（保持形状）
- $S_x \neq S_y$：非等比缩放（形状改变）
- $S > 1$：放大
- $0 < S < 1$：缩小
- $S < 0$：反射

**注意：** 相对于原点的缩放会改变图形位置，相对于重心的缩放保持图形中心不变。

### 2.4 旋转变换（Rotation）【高频考点】

**几何推导：**

设点 $(x, y)$ 到原点距离为 $r$，与x轴夹角为 $\phi$，则：
$$\begin{cases} x = r\cos\phi \\ y = r\sin\phi \end{cases}$$

旋转角度 $\theta$ 后：
$$\begin{cases} x' = r\cos(\phi + \theta) = r\cos\phi\cos\theta - r\sin\phi\sin\theta = x\cos\theta - y\sin\theta \\ y' = r\sin(\phi + \theta) = r\cos\phi\sin\theta + r\sin\phi\cos\theta = x\sin\theta + y\cos\theta \end{cases}$$

**齐次坐标矩阵形式：**
$$[x', y', 1] = [x, y, 1] \begin{bmatrix} \cos\theta & \sin\theta & 0 \\ -\sin\theta & \cos\theta & 0 \\ 0 & 0 & 1 \end{bmatrix}$$

**旋转变换矩阵：**
$$R(\theta) = \begin{bmatrix} \cos\theta & \sin\theta & 0 \\ -\sin\theta & \cos\theta & 0 \\ 0 & 0 & 1 \end{bmatrix}$$

**性质：**
- $\theta > 0$：逆时针旋转
- $\theta < 0$：顺时针旋转
- 旋转中心为原点


### 2.5 对称/反射变换（Reflection）

#### 关于x轴对称

**几何关系：**
$$\begin{cases} x' = x \\ y' = -y \end{cases}$$

**变换矩阵：**
$$Ref_x = \begin{bmatrix} 1 & 0 & 0 \\ 0 & -1 & 0 \\ 0 & 0 & 1 \end{bmatrix}$$

#### 关于y轴对称

**几何关系：**
$$\begin{cases} x' = -x \\ y' = y \end{cases}$$

**变换矩阵：**
$$Ref_y = \begin{bmatrix} -1 & 0 & 0 \\ 0 & 1 & 0 \\ 0 & 0 & 1 \end{bmatrix}$$

#### 关于原点对称

**几何关系：**
$$\begin{cases} x' = -x \\ y' = -y \end{cases}$$

**变换矩阵：**
$$Ref_o = \begin{bmatrix} -1 & 0 & 0 \\ 0 & -1 & 0 \\ 0 & 0 & 1 \end{bmatrix}$$

#### 关于直线y=x对称

**几何关系：**
$$\begin{cases} x' = y \\ y' = x \end{cases}$$

**变换矩阵：**
$$Ref_{y=x} = \begin{bmatrix} 0 & 1 & 0 \\ 1 & 0 & 0 \\ 0 & 0 & 1 \end{bmatrix}$$

### 2.6 错切变换（Shear）

#### 沿x轴方向错切

**几何关系：**
$$\begin{cases} x' = x + y \cdot sh_x \\ y' = y \end{cases}$$

**变换矩阵：**
$$Sh_x = \begin{bmatrix} 1 & 0 & 0 \\ sh_x & 1 & 0 \\ 0 & 0 & 1 \end{bmatrix}$$

**几何意义：** 将图形上关于y轴的平行线沿x方向推成倾斜线。

#### 沿y轴方向错切

**几何关系：**
$$\begin{cases} x' = x \\ y' = y + x \cdot sh_y \end{cases}$$

**变换矩阵：**
$$Sh_y = \begin{bmatrix} 1 & sh_y & 0 \\ 0 & 1 & 0 \\ 0 & 0 & 1 \end{bmatrix}$$

**几何意义：** 将图形上关于x轴的平行线沿y方向推成倾斜线。

---

## 3. 组合变换【重点】

### 3.1 变换的组合

二维变换的组合变换是对图形做一次以上的基本变换。组合变换通过矩阵乘法实现。

**重要性质：** 矩阵乘法不满足交换律，因此变换顺序影响最终结果。

### 3.2 相对于任意点的缩放

**问题：** 相对于点 $(x_f, y_f)$ 进行缩放变换。

**步骤：**
1. 将缩放中心平移到原点：$T_1 = T(-x_f, -y_f)$
2. 相对于原点缩放：$S = S(S_x, S_y)$
3. 将缩放中心平移回原位置：$T_2 = T(x_f, y_f)$

**组合变换矩阵：**
$$T = T_1 \cdot S \cdot T_2$$

$$T = \begin{bmatrix} 1 & 0 & 0 \\ 0 & 1 & 0 \\ -x_f & -y_f & 1 \end{bmatrix} \begin{bmatrix} S_x & 0 & 0 \\ 0 & S_y & 0 \\ 0 & 0 & 1 \end{bmatrix} \begin{bmatrix} 1 & 0 & 0 \\ 0 & 1 & 0 \\ x_f & y_f & 1 \end{bmatrix}$$

$$= \begin{bmatrix} S_x & 0 & 0 \\ 0 & S_y & 0 \\ x_f(1-S_x) & y_f(1-S_y) & 1 \end{bmatrix}$$

### 3.3 相对于任意点的旋转【高频考点】

**问题：** 相对于点 $(x_r, y_r)$ 旋转角度 $\theta$。

**步骤：**
1. 将旋转中心平移到原点：$T_1 = T(-x_r, -y_r)$
2. 相对于原点旋转：$R = R(\theta)$
3. 将旋转中心平移回原位置：$T_2 = T(x_r, y_r)$

**组合变换矩阵：**
$$T = T_1 \cdot R \cdot T_2$$

$$T = \begin{bmatrix} \cos\theta & \sin\theta & 0 \\ -\sin\theta & \cos\theta & 0 \\ x_r(1-\cos\theta)+y_r\sin\theta & y_r(1-\cos\theta)-x_r\sin\theta & 1 \end{bmatrix}$$

### 3.4 变换顺序的影响

**示例：** 先旋转再平移 vs 先平移再旋转

设旋转角度为 $\theta$，平移量为 $(T_x, T_y)$：

**先旋转再平移：**
$$M_1 = R(\theta) \cdot T(T_x, T_y)$$

**先平移再旋转：**
$$M_2 = T(T_x, T_y) \cdot R(\theta)$$

一般情况下 $M_1 \neq M_2$，因此变换顺序很重要。


---

## 4. 三维图形几何变换

### 4.1 三维平移变换

**几何关系：**
$$\begin{cases} x' = x + T_x \\ y' = y + T_y \\ z' = z + T_z \end{cases}$$

**变换矩阵：**
$$T(T_x, T_y, T_z) = \begin{bmatrix} 1 & 0 & 0 & 0 \\ 0 & 1 & 0 & 0 \\ 0 & 0 & 1 & 0 \\ T_x & T_y & T_z & 1 \end{bmatrix}$$

### 4.2 三维缩放变换

**几何关系：**
$$\begin{cases} x' = x \cdot S_x \\ y' = y \cdot S_y \\ z' = z \cdot S_z \end{cases}$$

**变换矩阵：**
$$S(S_x, S_y, S_z) = \begin{bmatrix} S_x & 0 & 0 & 0 \\ 0 & S_y & 0 & 0 \\ 0 & 0 & S_z & 0 \\ 0 & 0 & 0 & 1 \end{bmatrix}$$

### 4.3 三维旋转变换

#### 绕x轴旋转

**变换矩阵：**
$$R_x(\theta) = \begin{bmatrix} 1 & 0 & 0 & 0 \\ 0 & \cos\theta & \sin\theta & 0 \\ 0 & -\sin\theta & \cos\theta & 0 \\ 0 & 0 & 0 & 1 \end{bmatrix}$$

#### 绕y轴旋转

**变换矩阵：**
$$R_y(\theta) = \begin{bmatrix} \cos\theta & 0 & -\sin\theta & 0 \\ 0 & 1 & 0 & 0 \\ \sin\theta & 0 & \cos\theta & 0 \\ 0 & 0 & 0 & 1 \end{bmatrix}$$

#### 绕z轴旋转

**变换矩阵：**
$$R_z(\theta) = \begin{bmatrix} \cos\theta & \sin\theta & 0 & 0 \\ -\sin\theta & \cos\theta & 0 & 0 \\ 0 & 0 & 1 & 0 \\ 0 & 0 & 0 & 1 \end{bmatrix}$$

### 4.4 三维对称变换

#### 关于xOy平面对称

**变换矩阵：**
$$SY_{xy} = \begin{bmatrix} 1 & 0 & 0 & 0 \\ 0 & 1 & 0 & 0 \\ 0 & 0 & -1 & 0 \\ 0 & 0 & 0 & 1 \end{bmatrix}$$

#### 关于xOz平面对称

**变换矩阵：**
$$SY_{xz} = \begin{bmatrix} 1 & 0 & 0 & 0 \\ 0 & -1 & 0 & 0 \\ 0 & 0 & 1 & 0 \\ 0 & 0 & 0 & 1 \end{bmatrix}$$

#### 关于yOz平面对称

**变换矩阵：**
$$SY_{yz} = \begin{bmatrix} -1 & 0 & 0 & 0 \\ 0 & 1 & 0 & 0 \\ 0 & 0 & 1 & 0 \\ 0 & 0 & 0 & 1 \end{bmatrix}$$

### 4.5 三维错切变换

三维错切变换有六种基本形式：

| 错切类型 | 描述 |
|---------|------|
| 沿x含y错切 | x坐标随y变化 |
| 沿x含z错切 | x坐标随z变化 |
| 沿y含x错切 | y坐标随x变化 |
| 沿y含z错切 | y坐标随z变化 |
| 沿z含x错切 | z坐标随x变化 |
| 沿z含y错切 | z坐标随y变化 |

### 4.6 三维变换矩阵的一般形式

$$A = \begin{bmatrix} a_{11} & a_{12} & a_{13} & a_{14} \\ a_{21} & a_{22} & a_{23} & a_{24} \\ a_{31} & a_{32} & a_{33} & a_{34} \\ a_{41} & a_{42} & a_{43} & a_{44} \end{bmatrix}$$

各部分功能：
- 左上角 $3 \times 3$ 子矩阵：比例、旋转、错切变换
- 左下角 $1 \times 3$ 子矩阵：平移变换
- 右上角 $3 \times 1$ 子矩阵：透视投影变换
- 右下角元素 $a_{44}$：整体比例因子


---

## 5. 坐标系变换

### 5.1 图形显示流程

```
三维世界坐标系 → 三维裁剪 → 投影 → 窗口到视区变换 → 二维屏幕坐标系
```

### 5.2 坐标系类型

#### 局部坐标系（Local Coordinates）
- 也称模型坐标系或对象坐标系
- 用于定义单个物体的几何形状
- 原点通常在物体的中心或某个特征点

#### 世界坐标系（World Coordinates）
- 用于描述场景中所有物体的位置和方向
- 是一个全局参考系
- 所有物体通过变换从局部坐标系转换到世界坐标系

#### 摄像机坐标系（Camera/View Coordinates）
- 也称观察坐标系或视图坐标系
- 以摄像机（观察者）为原点
- 用于确定物体相对于观察者的位置

#### 屏幕坐标系（Screen Coordinates）
- 二维坐标系
- 用于在显示设备上定位像素

### 5.3 局部坐标到世界坐标的变换

**模型变换（Model Transformation）：**

将物体从局部坐标系变换到世界坐标系，通常包括：
1. 缩放：调整物体大小
2. 旋转：调整物体方向
3. 平移：调整物体位置

**变换矩阵：**
$$M_{model} = T \cdot R \cdot S$$

### 5.4 世界坐标到摄像机坐标的变换

**视图变换（View Transformation）：**

将世界坐标系中的点变换到以摄像机为中心的坐标系。

**摄像机参数：**
- 摄像机位置：$\vec{eye}$
- 观察目标点：$\vec{target}$
- 上方向向量：$\vec{up}$

**构建视图矩阵的步骤：**

1. 计算摄像机坐标系的三个基向量：
   - $\vec{z} = normalize(\vec{eye} - \vec{target})$（观察方向的反方向）
   - $\vec{x} = normalize(\vec{up} \times \vec{z})$（右方向）
   - $\vec{y} = \vec{z} \times \vec{x}$（上方向）

2. 视图变换矩阵：
$$V = \begin{bmatrix} x_x & y_x & z_x & 0 \\ x_y & y_y & z_y & 0 \\ x_z & y_z & z_z & 0 \\ -\vec{x} \cdot \vec{eye} & -\vec{y} \cdot \vec{eye} & -\vec{z} \cdot \vec{eye} & 1 \end{bmatrix}$$

### 5.5 窗口到视区的变换

**窗口（Window）：** 世界坐标系中定义的矩形区域，表示要显示的内容范围。

**视区（Viewport）：** 屏幕坐标系中的矩形区域，表示显示内容的位置和大小。

**变换公式：**

设窗口范围为 $[W_{xl}, W_{xr}] \times [W_{yb}, W_{yt}]$，视区范围为 $[V_{xl}, V_{xr}] \times [V_{yb}, V_{yt}]$

$$x_v = \frac{V_{xr} - V_{xl}}{W_{xr} - W_{xl}}(x_w - W_{xl}) + V_{xl}$$

$$y_v = \frac{V_{yt} - V_{yb}}{W_{yt} - W_{yb}}(y_w - W_{yb}) + V_{yb}$$

**简化形式：**
$$x_v = a \cdot x_w + c$$
$$y_v = b \cdot y_w + d$$

其中：
- $a = \frac{V_{xr} - V_{xl}}{W_{xr} - W_{xl}}$
- $b = \frac{V_{yt} - V_{yb}}{W_{yt} - W_{yb}}$
- $c = V_{xl} - a \cdot W_{xl}$
- $d = V_{yb} - b \cdot W_{yb}$


---

## 6. 投影变换

### 6.1 投影的基本概念

**定义：** 将n维的点变换成小于n维的点，通常是将三维点变换到二维平面上。

**投影要素：**
- 投影中心（COP）：视点或光源位置
- 投影面：成像平面
- 投影线：从投影中心到物体各点的射线

### 6.2 投影分类

```
投影
├── 平行投影（投影中心在无穷远）
│   ├── 正平行投影（投影方向垂直于投影面）
│   │   ├── 三视图（正视图、侧视图、俯视图）
│   │   └── 正轴测投影（正等轴测、正二轴测）
│   └── 斜平行投影（投影方向与投影面成角度）
└── 透视投影（投影中心与投影面距离有限）
    ├── 一点透视
    ├── 两点透视
    └── 三点透视
```

### 6.3 平行投影

#### 三视图

**正视图（主视图）：** 向xOz平面投影
$$T_v = \begin{bmatrix} 1 & 0 & 0 & 0 \\ 0 & 0 & 0 & 0 \\ 0 & 0 & 1 & 0 \\ 0 & 0 & 0 & 1 \end{bmatrix}$$

**俯视图：** 向xOy平面投影
$$T_h = \begin{bmatrix} 1 & 0 & 0 & 0 \\ 0 & 1 & 0 & 0 \\ 0 & 0 & 0 & 0 \\ 0 & 0 & 0 & 1 \end{bmatrix}$$

**侧视图：** 向yOz平面投影
$$T_w = \begin{bmatrix} 0 & 0 & 0 & 0 \\ 0 & 1 & 0 & 0 \\ 0 & 0 & 1 & 0 \\ 0 & 0 & 0 & 1 \end{bmatrix}$$

#### 正轴测投影

**特点：** 投影平面不垂直于任何一个坐标轴，能同时看到物体的三个面。

**正等轴测投影：** 三个坐标轴的变形系数相等，三根坐标轴之间的夹角均为120°。

### 6.4 透视投影【重点】

**特点：**
- 投影中心与投影平面之间的距离有限
- 产生近大远小的视觉效果
- 深度感强，更加真实

#### 一点透视投影方程

设投影中心在原点，投影平面为 $z = d$（$d > 0$）：

**投影公式：**
$$x' = \frac{x \cdot d}{z} = \frac{x}{z/d}$$
$$y' = \frac{y \cdot d}{z} = \frac{y}{z/d}$$

令 $r = -1/d$，则：
$$x' = \frac{x}{1 + rz}$$
$$y' = \frac{y}{1 + rz}$$

**透视投影矩阵：**
$$T = \begin{bmatrix} 1 & 0 & 0 & 0 \\ 0 & 1 & 0 & 0 \\ 0 & 0 & 1 & r \\ 0 & 0 & 0 & 1 \end{bmatrix}$$

**齐次坐标变换：**
$$[x, y, z, 1] \cdot T = [x, y, z, rz+1]$$

**规格化后：**
$$[x', y', z', 1] = \left[\frac{x}{rz+1}, \frac{y}{rz+1}, \frac{z}{rz+1}, 1\right]$$

#### 灭点（Vanishing Point）

**定义：** 不平行于投影平面的平行线，经过透视投影后相交于一点，称为灭点。

**主灭点：** 平行于坐标轴的平行线在投影平面产生的灭点。

**灭点数量：**
- 一点透视：1个主灭点
- 两点透视：2个主灭点
- 三点透视：3个主灭点

**灭点位置计算：**

对于z轴方向的无穷远点 $(0, 0, 1, 0)$：
$$[0, 0, 1, 0] \cdot T_r = [0, 0, 1, r]$$

规格化后灭点为 $(0, 0, 1/r) = (0, 0, -d)$


---

## 7. 考试重点与真题解析

### 7.1 【去年真题示例】矢量运算计算题（6分）

**题目：** 已知三维空间中两个向量 $\vec{a} = (1, 2, 3)$ 和 $\vec{b} = (4, 5, 6)$，求：
1. $\vec{a} + \vec{b}$
2. $\vec{a} \cdot \vec{b}$（点积）
3. $\vec{a} \times \vec{b}$（叉积）
4. $\vec{a}$ 和 $\vec{b}$ 的夹角 $\theta$

**解答步骤：**

**第一步：矢量加法**（1分）
$$\vec{a} + \vec{b} = (1+4, 2+5, 3+6) = (5, 7, 9)$$

**第二步：点积计算**（1分）
$$\vec{a} \cdot \vec{b} = 1 \times 4 + 2 \times 5 + 3 \times 6 = 4 + 10 + 18 = 32$$

**第三步：叉积计算**（2分）
$$\vec{a} \times \vec{b} = \begin{vmatrix} \vec{i} & \vec{j} & \vec{k} \\ 1 & 2 & 3 \\ 4 & 5 & 6 \end{vmatrix}$$

$$= \vec{i}(2 \times 6 - 3 \times 5) - \vec{j}(1 \times 6 - 3 \times 4) + \vec{k}(1 \times 5 - 2 \times 4)$$

$$= \vec{i}(12 - 15) - \vec{j}(6 - 12) + \vec{k}(5 - 8)$$

$$= (-3, -6, -3)$$

**第四步：夹角计算**（2分）

首先计算两向量的模：
$$|\vec{a}| = \sqrt{1^2 + 2^2 + 3^2} = \sqrt{14}$$
$$|\vec{b}| = \sqrt{4^2 + 5^2 + 6^2} = \sqrt{77}$$

然后计算夹角：
$$\cos\theta = \frac{\vec{a} \cdot \vec{b}}{|\vec{a}||\vec{b}|} = \frac{32}{\sqrt{14} \times \sqrt{77}} = \frac{32}{\sqrt{1078}} \approx 0.974$$

$$\theta = \arccos(0.974) \approx 13.1°$$

---

### 7.2 【去年真题示例】图形变换计算题（6分）

**题目：** 将三角形ABC（顶点坐标为A(0,0)、B(2,0)、C(1,2)）绕点P(1,1)逆时针旋转90°，求变换后的顶点坐标。

**解答步骤：**

**第一步：确定变换步骤**（1分）

绕任意点旋转需要三步：
1. 将旋转中心P(1,1)平移到原点
2. 绕原点旋转90°
3. 将旋转中心平移回原位置

**第二步：构建变换矩阵**（2分）

平移矩阵 $T_1$（将P移到原点）：
$$T_1 = \begin{bmatrix} 1 & 0 & 0 \\ 0 & 1 & 0 \\ -1 & -1 & 1 \end{bmatrix}$$

旋转矩阵 $R$（逆时针90°，$\theta = 90°$）：
$$R = \begin{bmatrix} \cos90° & \sin90° & 0 \\ -\sin90° & \cos90° & 0 \\ 0 & 0 & 1 \end{bmatrix} = \begin{bmatrix} 0 & 1 & 0 \\ -1 & 0 & 0 \\ 0 & 0 & 1 \end{bmatrix}$$

平移矩阵 $T_2$（将P移回原位置）：
$$T_2 = \begin{bmatrix} 1 & 0 & 0 \\ 0 & 1 & 0 \\ 1 & 1 & 1 \end{bmatrix}$$

**第三步：计算组合变换矩阵**（1分）

$$M = T_1 \cdot R \cdot T_2$$

先计算 $T_1 \cdot R$：
$$T_1 \cdot R = \begin{bmatrix} 1 & 0 & 0 \\ 0 & 1 & 0 \\ -1 & -1 & 1 \end{bmatrix} \begin{bmatrix} 0 & 1 & 0 \\ -1 & 0 & 0 \\ 0 & 0 & 1 \end{bmatrix} = \begin{bmatrix} 0 & 1 & 0 \\ -1 & 0 & 0 \\ 1 & -1 & 1 \end{bmatrix}$$

再计算 $(T_1 \cdot R) \cdot T_2$：
$$M = \begin{bmatrix} 0 & 1 & 0 \\ -1 & 0 & 0 \\ 1 & -1 & 1 \end{bmatrix} \begin{bmatrix} 1 & 0 & 0 \\ 0 & 1 & 0 \\ 1 & 1 & 1 \end{bmatrix} = \begin{bmatrix} 0 & 1 & 0 \\ -1 & 0 & 0 \\ 2 & 0 & 1 \end{bmatrix}$$

**第四步：计算变换后的顶点坐标**（2分）

点A(0,0)：
$$[0, 0, 1] \cdot M = [0, 0, 1] \begin{bmatrix} 0 & 1 & 0 \\ -1 & 0 & 0 \\ 2 & 0 & 1 \end{bmatrix} = [2, 0, 1]$$
变换后 A' = (2, 0)

点B(2,0)：
$$[2, 0, 1] \cdot M = [2, 0, 1] \begin{bmatrix} 0 & 1 & 0 \\ -1 & 0 & 0 \\ 2 & 0 & 1 \end{bmatrix} = [2, 2, 1]$$
变换后 B' = (2, 2)

点C(1,2)：
$$[1, 2, 1] \cdot M = [1, 2, 1] \begin{bmatrix} 0 & 1 & 0 \\ -1 & 0 & 0 \\ 2 & 0 & 1 \end{bmatrix} = [0, 1, 1]$$
变换后 C' = (0, 1)

**答案：** 变换后的三角形顶点坐标为 A'(2, 0)、B'(2, 2)、C'(0, 1)


---

### 7.3 【去年真题示例】组合变换计算题（6分）

**题目：** 对点P(3, 2)依次进行以下变换：
1. 相对于原点放大2倍
2. 绕原点逆时针旋转45°
3. 向右平移5个单位

求最终坐标。

**解答步骤：**

**第一步：构建各变换矩阵**（2分）

缩放矩阵（放大2倍）：
$$S = \begin{bmatrix} 2 & 0 & 0 \\ 0 & 2 & 0 \\ 0 & 0 & 1 \end{bmatrix}$$

旋转矩阵（逆时针45°）：
$$R = \begin{bmatrix} \cos45° & \sin45° & 0 \\ -\sin45° & \cos45° & 0 \\ 0 & 0 & 1 \end{bmatrix} = \begin{bmatrix} \frac{\sqrt{2}}{2} & \frac{\sqrt{2}}{2} & 0 \\ -\frac{\sqrt{2}}{2} & \frac{\sqrt{2}}{2} & 0 \\ 0 & 0 & 1 \end{bmatrix}$$

平移矩阵（向右平移5）：
$$T = \begin{bmatrix} 1 & 0 & 0 \\ 0 & 1 & 0 \\ 5 & 0 & 1 \end{bmatrix}$$

**第二步：计算组合变换矩阵**（2分）

$$M = S \cdot R \cdot T$$

先计算 $S \cdot R$：
$$S \cdot R = \begin{bmatrix} 2 & 0 & 0 \\ 0 & 2 & 0 \\ 0 & 0 & 1 \end{bmatrix} \begin{bmatrix} \frac{\sqrt{2}}{2} & \frac{\sqrt{2}}{2} & 0 \\ -\frac{\sqrt{2}}{2} & \frac{\sqrt{2}}{2} & 0 \\ 0 & 0 & 1 \end{bmatrix} = \begin{bmatrix} \sqrt{2} & \sqrt{2} & 0 \\ -\sqrt{2} & \sqrt{2} & 0 \\ 0 & 0 & 1 \end{bmatrix}$$

再计算 $(S \cdot R) \cdot T$：
$$M = \begin{bmatrix} \sqrt{2} & \sqrt{2} & 0 \\ -\sqrt{2} & \sqrt{2} & 0 \\ 0 & 0 & 1 \end{bmatrix} \begin{bmatrix} 1 & 0 & 0 \\ 0 & 1 & 0 \\ 5 & 0 & 1 \end{bmatrix} = \begin{bmatrix} \sqrt{2} & \sqrt{2} & 0 \\ -\sqrt{2} & \sqrt{2} & 0 \\ 5 & 0 & 1 \end{bmatrix}$$

**第三步：计算最终坐标**（2分）

$$[3, 2, 1] \cdot M = [3, 2, 1] \begin{bmatrix} \sqrt{2} & \sqrt{2} & 0 \\ -\sqrt{2} & \sqrt{2} & 0 \\ 5 & 0 & 1 \end{bmatrix}$$

$$= [3\sqrt{2} - 2\sqrt{2} + 5, 3\sqrt{2} + 2\sqrt{2}, 1]$$

$$= [\sqrt{2} + 5, 5\sqrt{2}, 1]$$

$$\approx [6.414, 7.071, 1]$$

**答案：** 最终坐标为 $(\sqrt{2} + 5, 5\sqrt{2}) \approx (6.414, 7.071)$

---

### 7.4 变换矩阵的逆变换

#### 平移变换的逆

$$T^{-1}(T_x, T_y) = T(-T_x, -T_y) = \begin{bmatrix} 1 & 0 & 0 \\ 0 & 1 & 0 \\ -T_x & -T_y & 1 \end{bmatrix}$$

#### 缩放变换的逆

$$S^{-1}(S_x, S_y) = S(\frac{1}{S_x}, \frac{1}{S_y}) = \begin{bmatrix} \frac{1}{S_x} & 0 & 0 \\ 0 & \frac{1}{S_y} & 0 \\ 0 & 0 & 1 \end{bmatrix}$$

#### 旋转变换的逆

$$R^{-1}(\theta) = R(-\theta) = \begin{bmatrix} \cos\theta & -\sin\theta & 0 \\ \sin\theta & \cos\theta & 0 \\ 0 & 0 & 1 \end{bmatrix}$$

**注意：** 旋转矩阵是正交矩阵，其逆等于其转置，即 $R^{-1} = R^T$

#### 组合变换的逆

$$(M_1 \cdot M_2 \cdot M_3)^{-1} = M_3^{-1} \cdot M_2^{-1} \cdot M_1^{-1}$$

**注意：** 逆变换的顺序与原变换顺序相反。

---

## 8. 考试重点总结

### 8.1 必考知识点

**简答题重点：**
- 齐次坐标的概念和作用
- 各种基本变换的几何意义
- 透视投影与平行投影的区别
- 灭点的概念

**计算题重点：**（12分）
- 矢量运算：点积、叉积、夹角计算（6分）
- 图形变换：组合变换矩阵计算（6分）

### 8.2 高频考点

1. **矢量运算**：出现频率高
   - 点积和叉积的计算
   - 向量夹角的计算
   - 向量的单位化

2. **二维变换**：必考
   - 平移、缩放、旋转的矩阵表示
   - 绕任意点的旋转变换
   - 组合变换的计算

3. **透视投影**：常考
   - 投影公式的推导
   - 灭点的计算

### 8.3 解题技巧

**矢量运算技巧：**
- 点积结果是标量，叉积结果是向量
- 叉积计算可用行列式展开法
- 夹角计算先求点积和模长

**变换计算技巧：**
- 绕任意点变换 = 平移到原点 + 变换 + 平移回去
- 矩阵乘法从左到右依次计算
- 注意变换顺序，不可交换

**常见错误：**
- 混淆点积和叉积
- 变换顺序搞反
- 旋转角度正负方向搞错
- 齐次坐标忘记规格化

---

## 补充知识点

### 9. 矢量运算在图形学中的应用

#### 9.1 计算平面法向量

给定平面上三点 $P_1$、$P_2$、$P_3$，法向量为：
$$\vec{n} = (\vec{P_2} - \vec{P_1}) \times (\vec{P_3} - \vec{P_1})$$

#### 9.2 判断点在直线的哪一侧

给定直线上两点 $A$、$B$ 和待判断点 $P$：
$$d = (\vec{AB}) \times (\vec{AP})$$
- $d > 0$：P在直线左侧
- $d < 0$：P在直线右侧
- $d = 0$：P在直线上

#### 9.3 计算三角形面积

给定三角形三个顶点 $A$、$B$、$C$：
$$S = \frac{1}{2}|(\vec{AB}) \times (\vec{AC})|$$

### 10. 与其他章节的联系

**前置知识：**
- 第一章：计算机图形学基本概念
- 第二章：图形系统和坐标系统

**后续应用：**
- 第六章：裁剪算法中的坐标变换
- 曲线曲面：控制点的变换
- 光照计算：法向量的变换

**综合应用：**
- 三维场景渲染流水线
- 动画中的物体运动
- 交互式图形编辑
