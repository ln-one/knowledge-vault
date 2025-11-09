---
tags:
  - Knowledge/Algorithm
  - Knowledge/Programming-Languages/Java
created: 2025-11-07
author:
  - ln1
status: In Progress
---

## 1. 初级排序算法

### 1.1 游戏规则

**排序问题的定义**：
将一组对象按照某种逻辑顺序重新排列的过程。

**排序算法的基本要求**：
- **输入**：一个包含N个元素的数组
- **输出**：按照某种顺序排列的数组
- **稳定性**：相等的元素在排序后保持原有的相对顺序

**排序算法模板**：
```java
public class Example {
    public static void sort(Comparable[] a) {
        // 排序算法的具体实现
    }
    
    private static boolean less(Comparable v, Comparable w) {
        return v.compareTo(w) < 0;
    }
    
    private static void exch(Comparable[] a, int i, int j) {
        Comparable t = a[i];
        a[i] = a[j];
        a[j] = t;
    }
    
    private static void show(Comparable[] a) {
        for (int i = 0; i < a.length; i++)
            StdOut.print(a[i] + " ");
        StdOut.println();
    }
    
    public static boolean isSorted(Comparable[] a) {
        for (int i = 1; i < a.length; i++)
            if (less(a[i], a[i-1])) return false;
        return true;
    }
}
```

**排序成本模型**：
- **比较次数**：调用`less()`方法的次数
- **交换次数**：调用`exch()`方法的次数
- **数组访问次数**：访问数组元素的次数

**Comparable接口**：
```java
public class Date implements Comparable<Date> {
    private final int day;
    private final int month;
    private final int year;
    
    public Date(int d, int m, int y) {
        day = d; month = m; year = y;
    }
    
    public int compareTo(Date that) {
        if (this.year < that.year) return -1;
        if (this.year > that.year) return +1;
        if (this.month < that.month) return -1;
        if (this.month > that.month) return +1;
        if (this.day < that.day) return -1;
        if (this.day > that.day) return +1;
        return 0;
    }
}
```

### 1.2 选择排序

**算法思想**：
首先找到数组中最小的元素，将它与数组的第一个元素交换位置。然后在剩下的元素中找到最小的元素，将它与数组的第二个元素交换位置。如此往复，直到整个数组排序。

**算法实现**：
```java
public class Selection {
    public static void sort(Comparable[] a) {
        int N = a.length;
        for (int i = 0; i < N; i++) {
            // 将a[i]和a[i..N)中最小的元素交换
            int min = i;
            for (int j = i+1; j < N; j++)
                if (less(a[j], a[min])) min = j;
            exch(a, i, min);
        }
    }
}
```

**算法特点**：
- **运行时间与输入无关**：即使数组已经有序，也需要进行相同次数的比较
- **数据移动最少**：交换次数为N次，是所有排序算法中最少的
- **不稳定**：可能改变相等元素的相对顺序

**性能分析**：
- **比较次数**：~N²/2（准确值为 (N-1) + (N-2) + ... + 1 + 0 = N(N-1)/2）
- **交换次数**：N
- **时间复杂度**：O(N²)
- **空间复杂度**：O(1)

**适用场景**：
- 数据规模较小
- 交换操作代价很高的情况

**可视化过程**（对数组 S O R T E X A M P L E 排序）：
```
初始: S O R T E X A M P L E
i=0:  A O R T E X S M P L E  (找到A，与S交换)
i=1:  A E R T O X S M P L E  (找到E，与O交换)
i=2:  A E E T O X S M P L R  (找到E，与R交换)
...
```

### 1.3 插入排序

**算法思想**：
将每个元素插入到其他已经有序的元素中的适当位置。类似于整理扑克牌的方式。

**算法实现**：
```java
public class Insertion {
    public static void sort(Comparable[] a) {
        int N = a.length;
        for (int i = 1; i < N; i++) {
            // 将a[i]插入到a[i-1]、a[i-2]、a[i-3]...之中
            for (int j = i; j > 0 && less(a[j], a[j-1]); j--)
                exch(a, j, j-1);
        }
    }
}
```

**算法特点**：
- **对部分有序数组很高效**：如果数组已经部分有序，插入排序会很快
- **稳定排序**：不会改变相等元素的相对顺序
- **适应性强**：对于小规模数组或部分有序数组效率很高
- **在线算法**：可以在接收数据的同时进行排序

**性能分析**：
- **最坏情况**（逆序数组）：
  - 比较次数：~N²/2
  - 交换次数：~N²/2
- **最好情况**（已排序数组）：
  - 比较次数：N-1
  - 交换次数：0
- **平均情况**：
  - 比较次数：~N²/4
  - 交换次数：~N²/4
- **时间复杂度**：O(N²)
- **空间复杂度**：O(1)

**部分有序数组的定义**：
如果数组中倒置的数量小于数组大小的某个倍数，则称该数组是部分有序的。

**倒置（Inversion）**：
数组中的两个顺序颠倒的元素。例如，在数组 E X A M P L E 中，E-A、X-A、X-M等都是倒置。

**插入排序的性质**：
- 插入排序需要的交换次数等于数组中倒置的数量
- 插入排序需要的比较次数大于等于倒置的数量，小于等于倒置的数量加上数组的大小再减一

**优化版本**（减少交换次数）：
```java
public class InsertionX {
    public static void sort(Comparable[] a) {
        int N = a.length;
        for (int i = 1; i < N; i++) {
            Comparable temp = a[i];
            int j = i;
            // 移动而不是交换
            while (j > 0 && less(temp, a[j-1])) {
                a[j] = a[j-1];
                j--;
            }
            a[j] = temp;
        }
    }
}
```

### 1.4 比较两种算法

**选择排序 vs 插入排序**：

| 特性 | 选择排序 | 插入排序 |
|------|---------|---------|
| 时间复杂度（最坏） | O(N²) | O(N²) |
| 时间复杂度（最好） | O(N²) | O(N) |
| 时间复杂度（平均） | O(N²) | O(N²) |
| 空间复杂度 | O(1) | O(1) |
| 稳定性 | 不稳定 | 稳定 |
| 比较次数 | ~N²/2 | 0 到 ~N²/2 |
| 交换次数 | N | 0 到 ~N²/2 |
| 对有序数组 | 仍需O(N²) | O(N) |
| 对逆序数组 | O(N²) | O(N²) |
| 适应性 | 无 | 强 |

**性能对比实验结果**：
- **随机数组**：插入排序比选择排序快约2倍
- **部分有序数组**：插入排序明显更快
- **完全有序数组**：插入排序是线性时间，选择排序仍是平方时间
- **逆序数组**：两者性能相近，但插入排序略慢

**选择使用建议**：
- **选择排序**：当交换操作代价很高时使用
- **插入排序**：当数组部分有序或规模较小时使用

### 1.5 希尔排序

**算法思想**：
希尔排序是插入排序的改进版本，通过比较相距一定间隔的元素来工作，各趟比较所用的距离随着算法的进行而减小，直到只比较相邻元素的最后一趟排序为止。

**核心概念 - h有序数组**：
如果一个数组中任意间隔为h的元素都是有序的，则称该数组为h有序数组。换句话说，一个h有序数组就是h个互相独立的有序数组编织在一起组成的一个数组。

**算法实现**：
```java
public class Shell {
    public static void sort(Comparable[] a) {
        int N = a.length;
        int h = 1;
        
        // 计算初始增量h
        while (h < N/3) h = 3*h + 1;  // 1, 4, 13, 40, 121, 364, 1093, ...
        
        while (h >= 1) {
            // 将数组变为h有序
            for (int i = h; i < N; i++) {
                // 将a[i]插入到a[i-h], a[i-2*h], a[i-3*h]...之中
                for (int j = i; j >= h && less(a[j], a[j-h]); j -= h)
                    exch(a, j, j-h);
            }
            h = h/3;
        }
    }
}
```

**增量序列**：
- **Shell原始序列**：N/2, N/4, ..., 1
- **Knuth序列**：1, 4, 13, 40, 121, 364, ...（3^k-1)/2
- **Sedgewick序列**：1, 5, 19, 41, 109, ...

书中使用的是Knuth序列：h = 3*h + 1

**算法特点**：
- **突破平方时间复杂度**：希尔排序是第一个突破O(N²)的排序算法
- **不稳定排序**：可能改变相等元素的相对顺序
- **代码简洁**：只需在插入排序的基础上加几行代码
- **无需额外空间**：原地排序算法

**性能分析**：
- **时间复杂度**：取决于增量序列的选择
  - 使用Knuth序列：O(N^(3/2))
  - 最坏情况：O(N²)（但实际很少出现）
  - 平均情况：约O(N^(5/4))
- **空间复杂度**：O(1)
- **比较次数**：无法精确计算，但远小于N²

**工作原理**：
1. **大间隔排序**：首先使用大间隔h对数组进行部分排序
2. **逐步缩小间隔**：减小h的值，继续排序
3. **最终插入排序**：当h=1时，进行最后一次插入排序

**为什么希尔排序快？**
- **减少倒置数量**：大间隔排序能快速减少数组中的倒置数量
- **部分有序优势**：每次h减小后，数组都变得更加有序，而插入排序对部分有序数组很高效
- **长距离移动**：元素可以快速移动到最终位置附近

**可视化示例**（h序列：13, 4, 1）：
```
原始数组: S H E L L S O R T E X A M P L E

h=13有序后: (将间隔为13的元素排序)
h=4有序后:  (将间隔为4的元素排序)
h=1有序后:  A E E E H L L M O P R S S T X (最终结果)
```

**实际应用**：
- **中等规模数组**：对于几千到几万个元素的数组，希尔排序表现优秀
- **嵌入式系统**：代码量小，不需要额外空间
- **部分有序数组**：性能接近线性时间

**希尔排序的地位**：
- 在快速排序出现之前，希尔排序是最快的通用排序算法
- 现在仍然是小规模数组排序的好选择
- 是唯一无法用输入模型准确描述性能特征的排序算法

#
# 2. 归并排序

### 2.1 归并排序的基本思想

**分治思想**：
将一个大问题分解为若干个小问题，分别解决后再合并结果。

**归并排序的核心**：
- **分解**：将数组分成两半
- **递归**：对两半分别排序
- **合并**：将两个有序数组合并成一个有序数组

**原地归并的抽象方法**：
```java
public static void merge(Comparable[] a, int lo, int mid, int hi) {
    // 将a[lo..mid]和a[mid+1..hi]归并
    int i = lo, j = mid+1;
    
    // 将a[lo..hi]复制到aux[lo..hi]
    for (int k = lo; k <= hi; k++)
        aux[k] = a[k];
    
    // 归并回到a[lo..hi]
    for (int k = lo; k <= hi; k++) {
        if      (i > mid)              a[k] = aux[j++];  // 左半边用尽
        else if (j > hi)               a[k] = aux[i++];  // 右半边用尽
        else if (less(aux[j], aux[i])) a[k] = aux[j++];  // 右半边元素小
        else                           a[k] = aux[i++];  // 左半边元素小
    }
}
```

### 2.2 自顶向下的归并排序

**算法实现**：
```java
public class Merge {
    private static Comparable[] aux;  // 归并所需的辅助数组
    
    public static void sort(Comparable[] a) {
        aux = new Comparable[a.length];
        sort(a, 0, a.length - 1);
    }
    
    private static void sort(Comparable[] a, int lo, int hi) {
        if (hi <= lo) return;
        int mid = lo + (hi - lo) / 2;
        sort(a, lo, mid);        // 将左半边排序
        sort(a, mid+1, hi);      // 将右半边排序
        merge(a, lo, mid, hi);   // 归并结果
    }
}
```

**算法特点**：
- **稳定排序**：相等元素的相对顺序不变
- **需要额外空间**：需要与原数组等大的辅助数组
- **时间复杂度稳定**：无论输入如何，都是O(N log N)
- **递归实现**：代码简洁优雅

**性能分析**：
- **比较次数**：最多需要N log N次比较
- **数组访问次数**：最多需要6N log N次数组访问（2N次复制，2N次移回，最多2N次比较）
- **时间复杂度**：O(N log N)
- **空间复杂度**：O(N)

**递归树分析**：
```
层数    子数组数量    子数组大小    比较次数
0       1            N            ≤N
1       2            N/2          ≤N
2       4            N/4          ≤N
...
log N   N            1            ≤N

总比较次数 ≤ N log N
```

**改进方法**：

1. **对小规模子数组使用插入排序**：
```java
private static void sort(Comparable[] a, int lo, int hi) {
    if (hi <= lo + CUTOFF - 1) {
        Insertion.sort(a, lo, hi);
        return;
    }
    int mid = lo + (hi - lo) / 2;
    sort(a, lo, mid);
    sort(a, mid+1, hi);
    merge(a, lo, mid, hi);
}
```
通常CUTOFF取值为7到15之间。

2. **测试数组是否已经有序**：
```java
private static void sort(Comparable[] a, int lo, int hi) {
    if (hi <= lo) return;
    int mid = lo + (hi - lo) / 2;
    sort(a, lo, mid);
    sort(a, mid+1, hi);
    if (!less(a[mid+1], a[mid])) return;  // 如果已有序则跳过merge
    merge(a, lo, mid, hi);
}
```

3. **不将元素复制到辅助数组**：
在递归调用的每个层次交换输入数组和辅助数组的角色。

### 2.3 自底向上的归并排序

**算法思想**：
先归并微型数组，然后成对归并得到的子数组，如此这般，直到将整个数组归并在一起。

**算法实现**：
```java
public class MergeBU {
    private static Comparable[] aux;
    
    public static void sort(Comparable[] a) {
        int N = a.length;
        aux = new Comparable[N];
        
        for (int sz = 1; sz < N; sz = sz+sz)           // sz: 子数组大小
            for (int lo = 0; lo < N-sz; lo += sz+sz)   // lo: 子数组索引
                merge(a, lo, lo+sz-1, Math.min(lo+sz+sz-1, N-1));
    }
}
```

**算法特点**：
- **非递归实现**：使用循环代替递归
- **适合链表排序**：自底向上的方法更适合用链表组织的数据
- **性能与自顶向下相同**：比较次数和时间复杂度相同

**归并过程可视化**（sz = 1, 2, 4, 8）：
```
原始: M E R G E S O R T E X A M P L E

sz=1: EM EG OR RS EO RT AE LM EP EX
sz=2: EEGM ORRS AEOT ELMP EX
sz=4: EEGMORRS AEELMPOT EX
sz=8: AEEEGLMMOPRRSTX
```

### 2.4 归并排序的复杂度

**命题**：
- 对于长度为N的任意数组，自顶向下的归并排序需要1/2 N lg N 至 N lg N 次比较
- 自顶向下的归并排序最多需要访问数组6N lg N次

**下界证明**：
- 没有任何基于比较的算法能够保证使用少于lg(N!) ~ N lg N次比较将长度为N的数组排序
- 归并排序是一种渐进最优的基于比较排序的算法

## 3. 快速排序

### 3.1 快速排序的基本思想

**算法思想**：
快速排序是一种分治的排序算法。它将一个数组分成两个子数组，将两部分独立地排序。

**与归并排序的区别**：
- **归并排序**：将数组分成两个子数组分别排序，并将有序的子数组归并以将整个数组排序
- **快速排序**：当两个子数组都有序时，整个数组也就自然有序了

**算法实现**：
```java
public class Quick {
    public static void sort(Comparable[] a) {
        StdRandom.shuffle(a);  // 消除对输入的依赖
        sort(a, 0, a.length - 1);
    }
    
    private static void sort(Comparable[] a, int lo, int hi) {
        if (hi <= lo) return;
        int j = partition(a, lo, hi);  // 切分
        sort(a, lo, j-1);              // 将左半部分排序
        sort(a, j+1, hi);              // 将右半部分排序
    }
}
```

### 3.2 切分（Partition）

**切分的目标**：
对于某个索引j，a[j]已经排定，a[lo]到a[j-1]中的所有元素都不大于a[j]，a[j+1]到a[hi]中的所有元素都不小于a[j]。

**切分实现**：
```java
private static int partition(Comparable[] a, int lo, int hi) {
    int i = lo, j = hi+1;        // 左右扫描指针
    Comparable v = a[lo];        // 切分元素
    
    while (true) {
        // 扫描左右，检查扫描是否结束并交换元素
        while (less(a[++i], v)) if (i == hi) break;
        while (less(v, a[--j])) if (j == lo) break;
        if (i >= j) break;
        exch(a, i, j);
    }
    exch(a, lo, j);  // 将v = a[j]放入正确的位置
    return j;        // a[lo..j-1] <= a[j] <= a[j+1..hi]达成
}
```

**切分过程**：
1. 随意取a[lo]作为切分元素（pivot）
2. 从数组左端开始向右扫描直到找到一个大于等于它的元素
3. 从数组右端开始向左扫描直到找到一个小于等于它的元素
4. 交换这两个元素
5. 重复步骤2-4直到两个指针相遇
6. 将切分元素与左子数组最右侧的元素交换

**切分轨迹示例**：
```
初始: K R A T E L E P U I M Q C X O S
     v i                           j

扫描: K R A T E L E P U I M Q C X O S
     v     i                     j

交换: K C A T E L E P U I M Q R X O S
     v       i                 j

...最终: K C A I E L E P U M Q R T X O S
           j v
```

### 3.3 快速排序的性能

**性能分析**：
- **最好情况**：每次切分都正好将数组对半分，比较次数为~N lg N
- **平均情况**：比较次数为~1.39 N lg N
- **最坏情况**：每次切分都极不平衡（如已排序数组），比较次数为~N²/2

**命题**：
- 快速排序平均需要~2N ln N次比较（以及1/6的交换）
- 快速排序最多需要约N²/2次比较，但随机打乱数组能够预防这种情况

**算法特点**：
- **原地排序**：只需要一个很小的辅助栈
- **时间复杂度**：平均O(N log N)，最坏O(N²)
- **不稳定排序**：可能改变相等元素的相对顺序
- **实际应用最快**：内循环简短，比较次数少

**为什么快速排序快？**
- **内循环简单**：比较和交换操作非常简洁
- **缓存友好**：顺序访问数组元素
- **原地排序**：不需要额外的内存空间

### 3.4 快速排序的改进

**1. 切换到插入排序**：
对于小数组，快速排序比插入排序慢。
```java
private static void sort(Comparable[] a, int lo, int hi) {
    if (hi <= lo + M) {
        Insertion.sort(a, lo, hi);
        return;
    }
    int j = partition(a, lo, hi);
    sort(a, lo, j-1);
    sort(a, j+1, hi);
}
```
转换参数M的最佳值是5到15之间。

**2. 三取样切分**：
使用子数组的一小部分元素的中位数来切分数组。
```java
private static void sort(Comparable[] a, int lo, int hi) {
    if (hi <= lo) return;
    
    // 三取样切分
    int m = medianOf3(a, lo, lo + (hi-lo)/2, hi);
    exch(a, lo, m);
    
    int j = partition(a, lo, hi);
    sort(a, lo, j-1);
    sort(a, j+1, hi);
}
```

**3. 熵最优排序（三向切分）**：
处理含有大量重复元素的数组。

### 3.5 三向切分的快速排序

**问题背景**：
实际应用中经常会出现含有大量重复元素的数组，标准快速排序的性能会下降。

**算法思想**：
将数组切分为三部分：小于切分元素、等于切分元素、大于切分元素。

**算法实现**：
```java
public class Quick3way {
    private static void sort(Comparable[] a, int lo, int hi) {
        if (hi <= lo) return;
        
        int lt = lo, i = lo+1, gt = hi;
        Comparable v = a[lo];
        
        while (i <= gt) {
            int cmp = a[i].compareTo(v);
            if      (cmp < 0) exch(a, lt++, i++);
            else if (cmp > 0) exch(a, i, gt--);
            else              i++;
        }
        // 现在 a[lo..lt-1] < v = a[lt..gt] < a[gt+1..hi]
        sort(a, lo, lt - 1);
        sort(a, gt + 1, hi);
    }
}
```

**算法过程**：
- **lt**：小于v的元素的右边界
- **gt**：大于v的元素的左边界
- **i**：当前考察的元素

**三向切分过程**：
```
a[i] < v: 交换a[lt]和a[i]，lt++, i++
a[i] > v: 交换a[i]和a[gt]，gt--
a[i] = v: i++
```

**性能分析**：
- **随机数组**：与标准快速排序性能相当
- **大量重复元素**：运行时间从线性对数级别到线性级别
- **只有若干不同值**：运行时间为线性级别

**命题**：
对于包含大量重复元素的数组，三向切分的快速排序将排序时间从线性对数级别降低到了线性级别。

## 4. 优先队列

### 4.1 优先队列的API

**优先队列**：
支持删除最大元素和插入元素的数据结构。

**API定义**：
```java
public class MaxPQ<Key extends Comparable<Key>> {
    MaxPQ()                    // 创建一个优先队列
    MaxPQ(int max)             // 创建一个初始容量为max的优先队列
    MaxPQ(Key[] a)             // 用a[]中的元素创建一个优先队列
    void insert(Key v)         // 向优先队列中插入一个元素
    Key max()                  // 返回最大元素
    Key delMax()               // 删除并返回最大元素
    boolean isEmpty()          // 返回队列是否为空
    int size()                 // 返回优先队列中的元素个数
}
```

**应用场景**：
- **事件驱动模拟**：按时间顺序处理事件
- **数值计算**：选择最重要的计算任务
- **数据压缩**：Huffman编码
- **图算法**：Dijkstra最短路径、Prim最小生成树
- **操作系统**：任务调度

### 4.2 堆的定义

**二叉堆**：
一组能够用堆有序的完全二叉树排序的元素，并在数组中按照层级存储（不使用数组的第一个位置）。

**堆有序**：
当一棵二叉树的每个结点都大于等于它的两个子结点时，它被称为堆有序。

**完全二叉树**：
只有最底层的结点不是满的，且最底层的结点从左到右填充。

**数组表示**：
- 位置k的结点的父结点的位置为⌊k/2⌋
- 位置k的结点的两个子结点的位置为2k和2k+1

```
数组: [-, T, S, R, P, N, O, A, E, I, H, G]
索引:  0  1  2  3  4  5  6  7  8  9 10 11

树形结构:
           T(1)
         /      \
      S(2)       R(3)
     /    \     /    \
   P(4)  N(5) O(6)  A(7)
   / \   / \
 E(8)I(9)H(10)G(11)
```

### 4.3 堆的算法

**上浮（swim）**：
当某个结点的优先级上升（或在堆底加入新元素）时，需要由下至上恢复堆的顺序。

```java
private void swim(int k) {
    while (k > 1 && less(k/2, k)) {
        exch(k/2, k);
        k = k/2;
    }
}
```

**下沉（sink）**：
当某个结点的优先级下降（例如将根结点替换为较小的元素）时，需要由上至下恢复堆的顺序。

```java
private void sink(int k) {
    while (2*k <= N) {
        int j = 2*k;
        if (j < N && less(j, j+1)) j++;
        if (!less(k, j)) break;
        exch(k, j);
        k = j;
    }
}
```

### 4.4 基于堆的优先队列

**完整实现**：
```java
public class MaxPQ<Key extends Comparable<Key>> {
    private Key[] pq;      // 基于堆的完全二叉树
    private int N = 0;     // 存储于pq[1..N]中，pq[0]没有使用
    
    public MaxPQ(int maxN) {
        pq = (Key[]) new Comparable[maxN+1];
    }
    
    public boolean isEmpty() {
        return N == 0;
    }
    
    public int size() {
        return N;
    }
    
    public void insert(Key v) {
        pq[++N] = v;
        swim(N);
    }
    
    public Key delMax() {
        Key max = pq[1];       // 从根结点得到最大元素
        exch(1, N--);          // 将其和最后一个结点交换
        pq[N+1] = null;        // 防止对象游离
        sink(1);               // 恢复堆的有序性
        return max;
    }
    
    private boolean less(int i, int j) {
        return pq[i].compareTo(pq[j]) < 0;
    }
    
    private void exch(int i, int j) {
        Key t = pq[i];
        pq[i] = pq[j];
        pq[j] = t;
    }
    
    private void swim(int k) { /* 见上文 */ }
    private void sink(int k) { /* 见上文 */ }
}
```

**性能分析**：
- **插入元素**：最多需要1 + lg N次比较
- **删除最大元素**：最多需要2 lg N次比较
- **空间复杂度**：O(N)

### 4.5 堆排序

**算法思想**：
1. **堆的构造**：将所有元素组织成一个堆
2. **下沉排序**：重复删除最大元素

**算法实现**：
```java
public class Heap {
    public static void sort(Comparable[] a) {
        int N = a.length;
        
        // 堆的构造
        for (int k = N/2; k >= 1; k--)
            sink(a, k, N);
        
        // 下沉排序
        while (N > 1) {
            exch(a, 1, N--);
            sink(a, 1, N);
        }
    }
    
    private static void sink(Comparable[] a, int k, int N) {
        while (2*k <= N) {
            int j = 2*k;
            if (j < N && less(a, j, j+1)) j++;
            if (!less(a, k, j)) break;
            exch(a, k, j);
            k = j;
        }
    }
    
    private static boolean less(Comparable[] a, int i, int j) {
        return a[i-1].compareTo(a[j-1]) < 0;
    }
    
    private static void exch(Comparable[] a, int i, int j) {
        Comparable t = a[i-1];
        a[i-1] = a[j-1];
        a[j-1] = t;
    }
}
```

**堆排序的两个阶段**：

1. **堆的构造**：
   - 从右至左用sink()函数构造子堆
   - 数组的每个位置都已经是一个子堆的根结点
   - 如果一个结点的两个子结点都已经是堆，那么在该结点上调用sink()可以将它们变成一个堆

2. **下沉排序**：
   - 将堆中的最大元素删除，放到堆缩小后空出的位置

**性能分析**：
- **比较次数**：最多2N lg N + 2N次比较
- **时间复杂度**：O(N log N)
- **空间复杂度**：O(1)（原地排序）
- **不稳定排序**

**堆排序的特点**：
- **最优的时间和空间复杂度**：O(N log N)时间，O(1)空间
- **不稳定**：相等元素的相对顺序可能改变
- **缓存不友好**：很少利用缓存，数组元素很少和相邻的元素比较
- **实际应用较少**：虽然理论上很优秀，但实际性能不如快速排序

## 5. 排序算法总结

### 5.1 各种排序算法的性能特点

| 算法 | 是否稳定 | 是否原地排序 | 时间复杂度（最坏） | 时间复杂度（平均） | 时间复杂度（最好） | 空间复杂度 | 备注 |
|------|---------|-------------|-------------------|-------------------|-------------------|-----------|------|
| 选择排序 | 否 | 是 | N²/2 | N²/2 | N²/2 | 1 | 交换次数最少 |
| 插入排序 | 是 | 是 | N²/2 | N²/4 | N | 1 | 对小数组或部分有序数组很快 |
| 希尔排序 | 否 | 是 | ? | ? | N | 1 | 代码量小，不需要额外空间 |
| 快速排序 | 否 | 是 | N²/2 | 2N ln N | N lg N | lg N | 实际应用中最快 |
| 三向快速排序 | 否 | 是 | N²/2 | 2N ln N | N | lg N | 对重复元素很快 |
| 归并排序 | 是 | 否 | N lg N | N lg N | N lg N | N | 稳定，时间复杂度保证 |
| 堆排序 | 否 | 是 | 2N lg N | 2N lg N | N lg N | 1 | 最优的时间和空间复杂度 |

### 5.2 排序算法的选择

**根据数据特征选择**：
- **小规模数组（N < 15）**：插入排序
- **部分有序数组**：插入排序
- **大规模随机数组**：快速排序
- **大量重复元素**：三向快速排序
- **需要稳定排序**：归并排序
- **空间受限**：堆排序或希尔排序
- **保证最坏情况性能**：归并排序或堆排序

**Java系统排序**：
- **Arrays.sort()**：
  - 原始数据类型：三向切分的快速排序
  - 对象类型：归并排序（保证稳定性）

### 5.3 排序算法的稳定性

**稳定排序的重要性**：
在实际应用中，我们经常需要按照多个键对数据进行排序。稳定排序能够保证第一次排序的结果在第二次排序后仍然有效。

**稳定的排序算法**：
- 插入排序
- 归并排序

**不稳定的排序算法**：
- 选择排序
- 希尔排序
- 快速排序
- 堆排序

### 5.4 排序问题的归约

**归约**：
将一个问题转换为另一个问题来解决。

**常见的归约应用**：
- **找出重复元素**：先排序，然后遍历检查相邻元素
- **排名问题**：先排序，然后返回指定位置的元素
- **优先队列**：使用堆实现
- **中位数和顺序统计**：使用快速选择算法

**快速选择算法**：
找出数组中第k小的元素，平均时间复杂度O(N)。

```java
public static Comparable select(Comparable[] a, int k) {
    StdRandom.shuffle(a);
    int lo = 0, hi = a.length - 1;
    while (hi > lo) {
        int j = partition(a, lo, hi);
        if      (j == k) return a[k];
        else if (j > k)  hi = j - 1;
        else if (j < k)  lo = j + 1;
    }
    return a[k];
}
```

### 5.5 排序应用

**商业计算**：
- 组织和检索信息
- 数据分析和统计

**搜索**：
- 二分查找需要有序数组
- 数据库索引

**运筹学**：
- 调度问题
- 负载均衡

**事件驱动模拟**：
- 按时间顺序处理事件

**数值计算**：
- 寻找中位数和其他统计量

**组合搜索**：
- Prim算法和Dijkstra算法
- Kruskal算法
- Huffman压缩
- 字符串处理
