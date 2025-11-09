---
tags:
  - Knowledge/computer-science/CSAPP
  - Knowledge/Algorithm/Bitwise-Operations
created: 2025-08-09
author:
  - ln1
status: In Progress
---
 ## 1. bitXor
``` json
bitXor - x^y using only ~ and &
Example: bitXor(4, 5) = 1
Legal ops: ~ &
Max ops: 14
Rating: 1
```
1. 那么这就是一个简单的通过位运算来实现布尔代数转换的题目，根据离散数学的知识我们知道:异或可以:==`a ^ b =(~a & b) | (a & ~b)`==

2. 但是由于仅仅可以使用`~`和`&`来实现，不能出现`|`因此我们根据摩根定律：==`a | b = ~(~a & ~b)`==

3. 因此我们得到==`a ^ b = ~(~(~a & b) & ~(a & ~b))`==
> 另外`^`还有一个有趣的性质，曾在以往的算法练习中有所涉猎：[[P1469-找筷子#Implementation]]

**代码：**
```c
int bitXor(int x, int y) {
  /*use ~ and & to exec ^(bitXor),so the a^b=~(~(~a&b)&~(a&~b))*/
  return ~(~(~x & y) & ~(x & ~y));
}
```
## 2.tmin

 ```json
 tmin - return minimum two's complement integer
 Legal ops: ! ~ & ^ | + << >>
 Max ops: 4
 Rating: 1
```
1. 这道题就更为简单了，直接实现一个最小的补数即可，那么我们知道在32bit环境下的最小补数
	- tmin = 1000 0000 0000...
2. 也就是说除了第一个数为1以外，其它的数字均为0，而我们又可以使用<<，因此我只需要：
	- `return 1 << 31`
**代码：**
```c
int tmin(void) { return 1 << 31; }
```
## 3.isTmax
```json
 isTmax - returns 1 if x is the maximum, two's complement number,
 and 0 otherwise
 Legal ops: ! ~ & ^ | +
 Max ops: 10
 Rating: 1
```
1. 首先要弄清楚Tmax是什么，再去分析Tmax的性质：
	`Tmax = 0111 1111 1111 1111... `
	-  Tmax的符号为0；
	-  Tmax的剩下的bit均为1；
2. 因为`Tmax = 0111 1111 ...`，因此`Tmax + 1 = 1000 0000 ...`,为Tmin;
	那么综合Tmax和Tmin来看：
	- `~Tmax + 1 = Tmin`，那么`(Tmax + 1) ^ (~Tmin) = 0000 0000 ...`
	事实上我们不容易在有限条件下测试一个数是否等于1111 1111 ....，我们方便的是测试一个数字是否等于0000 0000 ... 因此有关的判定思路的重点应该放在判定是否为0，结合题意我们可以使用`！`这就非常方便了，如果一个数字等于0，那么`！`之后必定为1，反之则不然。当然我们为什么可以使得`(Tmax + 1) ^ (~Tmin) = 0`？
	**这里仍旧是利用了`x ^ x = 0`的性质**：[[P1469-找筷子#Implementation]]
	- 那么仅仅有这个性质还是不够的：`(-1)'s two's complement number = 1111 1111 ...`仍然符合这个性质，因此我们要排除`-1`这个数，那么如何排除呢？
	- 1111 ...有什么特殊的性质吗：我们注意到如果令该数+1 =`0000 0000 ...` `!0000 0000 ... = 1`，有切仅有(-1)是这样的，其它的数字都不可以，因此这就是我们要找的性质。
**总结**：满足两个性质==`(x + 1) ^ (~x) = 0`==  并且 ==`!(x + 1) = 0`== 
```c
int isTmax(int x) {
  int x1 = x + 1;
  int cond1 = !(x1 ^ ~x);
  int cond2 = !(!x1);
  int res = cond1 & cond2;
  return res;
}
```
## 4.allOddBits
```json
allOddBits - return 1 if all odd-numbered bits in word set to 1
where bits are numbered from 0 (least significant) to 31 (most significant)
Examples allOddBits(0xFFFFFFFD) = 0, allOddBits(0xAAAAAAAA) = 1
Legal ops: ! ~ & ^ | + << >>
Max ops: 12
Rating: 2
```
要是实现判断一个数字的奇数位全为1，我目前想到的是将数字进行折半
1. 先令==`x_32 = x`则`x_16 = x_32 >> 16`== 然后==`x_16 = x_16 & x_32`== 这样当且仅当偶数位全为1，则
	- 首先高16位清零，然后低16位的奇数位全为1
2. 然后依次类推 令==`x_8 = x_16 >> 8`==，然后==`x_8 = x_8 & x_16`==,结果仍然是：
	- 首先高8位清零，然后低8位的奇数位全为1
3. 然后是==`x_4 = x_8 >> 4`==,然后==`x_4 = x_4 & x_8`==,结果仍旧
4. ==`x_2 = x_4　>> 2`== then ==`x_2 = x_2 & x_4`==，also
5. 直到`x_1 = x_2 >> 1`  此时x_1仅仅保留了1位，如果此时x_1为1，那么反向推，则奇数数位均为1。
**结论：** 如过经过上述操作x_1 = 1,则可以判定该数字奇数位全为1。
```c
int allOddBits(int x) {
  unsigned int x_32 = x;
  unsigned int x_16 = x_32 >> 16;
  x_16 = x_16 & x_32;
  unsigned int x_8 = x_16 >> 8;
  x_8 = x_8 & x_16;
  unsigned int x_4 = x_8 >> 4;
  x_4 = x_4 & x_8;
  unsigned int x_2 = x_4 >> 2;
  x_2 = x_2 & x_4;
  unsigned int x_1 = x_2 >> 1;
  return x_1 & 1;
}
```
然而这个结果并不能通过.`./dlc -e bits.c` 因为不允许使用`unsigned`，然而按照我的思路必须使用unsigned，因为c语言默认是**算术右移**，而我的思路是建立在**逻辑右移**的基础上的，因此我必须考虑使用别的思路。
1. 我们直接利用掩码来过滤数据，`mask = 0XAAAA AAAA`，但是有数值限制，我最高只能令`mask = 0XAA`那我们如何得到上面的数据呢？
	- 令`mask = mask | (mask << 8)`
	- 令`mask = mask | (mask << 16)`
2. 然后我们直接使用`x & mask`掩去非奇数位的数字。之后如果得到的数字仍然与mask相同，则证明其为allOddBits number;
**代码:**
```c
int allOddBits(int x) {
  // use mask_code to mask the even bits
  int mask = 0xAA;
  mask = mask | (mask << 8);
  mask = mask | (mask << 16);
  // if the masked num == mask code,then the x is allOddBits;
  return !((x & mask) ^ mask);
}
```
>仍旧是关于`^`异或很有意思的一个点，就是它可以高效的判断两个数字是否相等，方法就是如上代码return 的部分`!((x & mask) ^ mask)`通过这个就可以确定x与mask是否相等。
## 5.  negate
```json
negate - return -x
Example: negate(1) = -1.
Legal ops: ! ~ & ^ | + << >>
Max ops: 5
Rating: 2
```
要实现返回一个负数，首先确定的是int使用的是**补码**，那么补码的正负数有什么规律呢？
1. 取反
2. +1
例如`-1`：`1111 1111 1111 1111`->`0000 0000 0000 0000`->`0000 0000 0000 0001`
**代码**：
```C
int negate(int x) {
  int rev_x = ~x;
  int neg_x = rev_x + 1;
  return neg_x;
}
```
## 6.isAsciiDigit
```json
isAsciiDigit - return 1 if 0x30 <= x <= 0x39 (ASCII codes for characters '0'
to '9') Example: isAsciiDigit(0x35) = 1. isAsciiDigit(0x3a) = 0.            isAsciiDigit(0x05) = 0.
   Legal ops: ! ~ & ^ | + << >>
   Max ops: 15
   Rating: 3
```
如果是ASCII的数字，就返回1；不是则返回0；
其实就是判断输入一个数字，数字范围是否是在`0x30`到`0x39`之间。
- 0x30: 0011 0000
- 0x39: 0011 0101
那么我们需要分别判断上下界，假设输入的数字是x
1. 下界lower：`x - 0x30 >= 0`转换为位表示:`x + (-0x30) >= 0`，如何表示`-0x30`：将**求一个正数的相反数的补码**：取反+1:因此`-0x30 = ~0x30+1`。
2. 上界upper:`0x39 - x >= 0`同理:`0x39 + (~x + 1) >=1`。
3. 然后将lower和upper取符号位，即均右移31位，如果右移31位后若两者符号位均为0,则返回1。
**代码**:
```c
int isAsciiDigit(int x) {
  int lower = x + (~0x30 + 1);
  int upper = 0x39 + (~x + 1);
  int lowerOK = !(lower >> 31);
  int upperOK = !(upper >> 31);
  return lowerOK & upperOK;
}
```
## 7.conditional
```json
 conditional - same as x ? y : z
   Example: conditional(2,4,5) = 4
   Legal ops: ! ~ & ^ | + << >>
   Max ops: 16
   Rating: 3
```
用位操作实现三元表达式
- 如果`x != 0`，那么返回`y`
- 如果`x = 0`，那么返回`z`
- 那么我们先把`x`转换成布尔值：通过`!!x`
- 经过这个转换之后，可能为`0000 0001`或`0000 0000`，那么如何做转换呢，我们考虑到可以构造两个掩码`1111 1111`和`0000 0000`来对`y`和`z`分别进行掩码处理。
	- 为了**构建掩码**，我们**考虑`1111 1111`的一个关键特性**，如果`x=0000 0001`,`~x`+`0000 0001`，就会得到`1111 1111`，如果`~x`=`0000 0000`，那么就会得到`0000 0000`。这样就得到了掩码。
- 接下来利用掩码选择：`(y & mask) | (z & ~mask)`。
**代码**：
```C
int conditional(int x, int y, int z) { 
    x = !!x;
    int mask = ~x + 1;
    return (y & mask) | (z & ~mask);
}
```
## 8.isLessOrEqual
```json
isLessOrEqual - if x <= y  then return 1, else return 0
   Example: isLessOrEqual(4,5) = 1.
   Legal ops: ! ~ & ^ | + << >>
   Max ops: 24
   Rating: 3
```
如果`y-x>=0`返回1,反之返回0。
- `y+(-x)>=0`
- 先将`x`转换成`-x`的补码:取反+1
- 然后将`y`同`-x`的补码相加，然后移位判断符号，就可以判断了
**代码**:
```C
int isLessOrEqual(int x, int y) { 
    int temp_x = (~x)+1;
    int res = temp_x + y;
    return !(res>>31);
}
```
## 9.logicalNeg
```json
logicalNeg - implement the ! operator, using all of
              the legal operators except !
   Examples: logicalNeg(3) = 0, logicalNeg(0) = 1
   Legal ops: ~ & ^ | + << >>
   Max ops: 12
   Rating: 4
```
`!`的作用是：
- 当数为0的时候，为1
- 当数不为0的时候，为0
现在考虑当数为0时，`~x`一定为`1111`，若1111 + 1,则为0000
- 那么如果x是0000,那么`-x`和`x`的符号位都为0,当且仅当。
- 而且是算术右移
- 然后我们就可以利用**符号位**来进行判断：
```C
int logicalNeg(int x) {
    return ((x|(~x+1))>>31)+1;
}
```
## 10.howManyBits
```json
howManyBits - return the minimum number of bits required to represent x in
two's complement
   Examples: howManyBits(12) = 5
             howManyBits(298) = 10
             howManyBits(-5) = 4
             howManyBits(0)  = 1
             howManyBits(-1) = 1
             howManyBits(0x80000000) = 32
   Legal ops: ! ~ & ^ | + << >>
   Max ops: 90
   Rating: 4
```
- 考虑将所有的负数先通过`~x`转换为num。
- 那么答案应该是32-(num从左往右数直到第一个1的位数),然后不能用循环，然后又有操作数要求，那么我们使用二分查找的方法进行操作：
	- 共32位，bit_16 = 先右移16位，如果为0，则左边16位无0，有1则为1。这代表着后16位是否一定是存在的。
	- 然后分情况向左移动，如果bit_16 = 1,则向左移16位，如果为0，则不移，那么我们直接根据这个来向左移bit_16 << 4位。
	- 然后bit_8 = 左边8位是否有1，如果有，也是同样的。
- 最后将bit_16 << 4，bit_8 << 3 ...等加起来，就可以得到最终的结果了。
**代码**:
```C
int howManyBits(int x) { 
	int sign_all = x >> 31;
	int num = sign_all ^ x;
	int bit_16 = !!(num >> 16) << 4;
	num >>= bit_16;
	
	int bit_8 = !!(num >> 8) << 3;
	num >>= bit_8;
	
	int bit_4 = !!(num >> 4) << 2;
	num >>= bit_4;
	
	int bit_2 = !!(num >> 2) << 1;
	num >>= bit_2;
	
	int bit_1 = !!(num >> 1);
	num >>= bit_1;
	
	int bit_0 = num & 1;
	
	return bit_16 + bit_8 + bit_4 + bit_2 + bit_1 + bit_0 + 1;
}
```
## 11.floatScale2
```json
floatScale2 - Return bit-level equivalent of expression 2*f for
   floating point argument f.
   Both the argument and result are passed as unsigned int's, but
   they are to be interpreted as the bit-level representation of
   single-precision floating point values.
   When argument is NaN, return argument
   Legal ops: Any integer/unsigned operations incl. ||, &&. also if, while
   Max ops: 30
   Rating: 4
```
求`2✖️f`，那么考虑到[[Chapter-2-信息的表示和处理#4.2.IEEE浮点表示]]
![[Pasted image 20251016180116.png]]
- IEEE有规格化和非规格化之分
![[Pasted image 20250925205147.png]]
- 先考虑特殊情况，如果是无穷大或者NaN(exp=255)，那么直接返回uf。
- 如果是非规格化数(exp=0)，那么将frac = frac << 1。如果frac溢出，则exp = 1，然后直接省略溢出的那一位即可。
- 如果是规格化数，直接让指数exp += 1即可。
**代码**：
```C
unsigned floatScale2(unsigned uf) {
    unsigned sign = uf & 0x80000000;        // 取符号位 (bit 31)
    unsigned exp  = (uf >> 23) & 0xFF;      // 取指数位 (bits 30-23)
    unsigned frac = uf & 0x7FFFFF;          // 取尾数位 (bits 22-0)

    // 情况 1: NaN 或 ±∞，直接返回
    if (exp == 0xFF)
        return uf;

    // 情况 2: 非规范化数 (exp == 0)
    if (exp == 0) {
        frac <<= 1;   // 左移一位，相当于乘2

        // 如果左移后最高位变成1（第23位），则说明要变为规范化数
        if (frac & 0x800000) {
            exp = 1;
            frac &= 0x7FFFFF; // 清除溢出位
        }
    } else {
        // 情况 3: 规范化数
        exp += 1;

        // 如果加1后溢出为255，变为无穷大
        if (exp == 0xFF)
            frac = 0;
    }

    // 重新组合
    return sign | (exp << 23) | frac;
}
```
## 12.floatFloat2Int
```Json
floatFloat2Int - Return bit-level equivalent of expression (int) f
   for floating point argument f.
   Argument is passed as unsigned int, but
   it is to be interpreted as the bit-level representation of a
   single-precision floating point value.
   Anything out of range (including NaN and infinity) should return
   0x80000000u.
   Legal ops: Any integer/unsigned operations incl. ||, &&. also if, while
   Max ops: 30
   Rating: 4
```
- 首先，要将IEEE754浮点数转换成浮点数，同11题，取sign,exp,frac。
- 然后先判断特殊情况，将exp转换为E=exp-127
- 然后先将后23位取出提取到val，然后利用掩码将**省略的1**加上。
- 然后因为默认val就是23位，因此判断E是否大于23，如果大于，则左移(E-23)
- 如果小于，则右移(23-E)
```C
int floatFloat2Int(unsigned uf) {
    unsigned sign = uf & 0x80000000;  // 取符号位 (bit 31)
    unsigned exp = (uf >> 23) & 0xFF; // 取指数位 (bits 30-23)
    unsigned frac = uf & 0x7FFFFF;    // 取尾数位 (bits 22-0)
    int E = (int)(exp - 127);                 // 计算实际指数
    unsigned val;

    if(E>=31) return 0x80000000u; // 超出范围
    if(E<0) return 0; // 绝对值小于1

    frac = frac | 0x800000; // 添加隐含的1
    if(E>23) val = frac << (E - 23); // 左移
    else val = frac >> (23 - E); // 右移
    if(sign) val = -val;
    return val;
}
```
## 13.floatPower2
```Json
floatPower2 - Return bit-level equivalent of the expression 2.0^x
   (2.0 raised to the power x) for any 32-bit integer x.

   The unsigned value that is returned should have the identical bit
   representation as the single-precision floating-point number 2.0^x.
   If the result is too small to be represented as a denorm, return
   1. If too large, return +INF.

   Legal ops: Any integer/unsigned operations incl. ||, &&. Also if, while
   Max ops: 30
   Rating: 4
```
- 首先可以表示的范围是多少，先分析最小，最小的能表示的二的倍数应该是127+(23-1):因为到最后非规格化数的，即exp全为0之后，在frac还可以最多向右移动22位。因此2的-149次方为最小能表示的数字。
- 接下来是直到$2^{-127}都是非规格化的数字，即exp全为0，此时我们根据`x+149`来算出应该向左移的位数即可。
- 然后是规格化数字，我们直接使得exp = x + 127即可。
- 然后是大于127的，直接返回+INF：0x7f800000。
**代码**:
```C
unsigned floatPower2(int x) {
    unsigned exp, frac;
    
    // 情况 1：太小，返回 0
    if (x < -149)
        return 0;

    // 情况 2：非规格化数（denorm）
    else if (x < -126) {
        // 对应的 fraction = 2^(x + 149)
        frac = 1u << (x + 149);
        return frac;
    }

    // 情况 3：规格化数（normalized）
    else if (x <= 127) {
        exp = x + 127;   // 加上偏置
        return exp << 23;
    }

    // 情况 4：太大，返回 +INF
    else
        return 0x7f800000;
}
```