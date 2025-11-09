---
tags:
  - Knowledge/Algorithm
created: 2025-10-20
author:
  - ln1
status: In Progress
---
## 1.基础编程模型
将描述和实现算法所用到的语言特性、软件库和操作系统特性总称为基础编程模型。

### 1.1 Java语言特性
- **强类型语言**：Java是一种强类型的语言，编译器会检查类型的一致性，在编译时就能发现类型不匹配的错误。
- **数组创建**：因为Java编译器在编译时无法知道应该为数组预留多少空间，所以我们需要在运行时明确地创建数组。
  ```java
  int[] a = new int[N];  // 创建长度为N的整型数组
  ```
- **数组引用**：如果我们将一个数组变量赋予另一个变量，那么两个变量将会指向同一个数组（别名现象）。
  ```java
  int[] a = new int[N];
  int[] b = a;  // b和a指向同一个数组
  ```
  - 因此如果要复制一份数组，应该声明、创建并初始化一个新的数组，然后将原数组中的元素逐个复制到新数组。
  ```java
  int[] b = new int[a.length];
  for (int i = 0; i < a.length; i++) {
      b[i] = a[i];
  }
  ```

### 1.2 静态方法
- **方法签名**：包括方法名和参数类型列表
- **方法重载**：同一个类中可以有多个同名方法，只要参数列表不同
- **递归方法**：方法调用自身来解决问题
### 1.3 递归
递归是一种强大的编程技术，方法调用自身来解决问题。

**递归的三个重要特性**：
1. **基础情况（Base Case）**：递归总有一个最简单的情况——方法的第一条语句总是一个包含return的条件语句，用于终止递归。
2. **递归情况（Recursive Case）**：递归调用总是去尝试解决一个规模更小的子问题，这样递归才能收敛到最简单的情况。
3. **自相似性**：递归的父问题和尝试解决的子问题之间不应该有交集，确保问题能够被正确分解。

**递归示例**：
```java
// 计算阶乘
public static long factorial(int n) {
    if (n <= 1) return 1;        // 基础情况
    return n * factorial(n-1);   // 递归情况
}

// 二分查找
public static int binarySearch(int[] a, int key, int lo, int hi) {
    if (lo > hi) return -1;      // 基础情况
    int mid = lo + (hi - lo) / 2;
    if (key < a[mid]) return binarySearch(a, key, lo, mid-1);
    else if (key > a[mid]) return binarySearch(a, key, mid+1, hi);
    else return mid;
}
```
### 1.4 API（应用程序编程接口）
API的目的是将调用和实现分离：除了API中给出的信息，调用者不需要知道实现的其他细节。

**API的组成部分**：
- **构造函数**：创建对象的方法
- **实例方法**：操作对象数据的方法  
- **静态方法**：不依赖于对象状态的工具方法

**API设计原则**：
- **简洁性**：API应该尽可能简单，只包含必要的方法
- **一致性**：相似的操作应该有相似的接口
- **通用性**：API应该适用于广泛的应用场景
- **易于实现**：API的设计应该便于高效实现

**示例API**：
```java
public class Counter {
    public Counter(String id)           // 构造函数
    public void increment()             // 增加计数器
    public int tally()                  // 返回当前计数
    public String toString()            // 字符串表示
}
```
### 1.5 数据抽象
数据抽象的主要思想是鼓励程序员定义自己的数据类型，而不仅仅是那些操作预定义的数据类型的静态方法。

**数据抽象的核心概念**：
- **抽象数据类型（ADT）**：定义了数据的表示和对数据的操作
- **封装**：将数据表示隐藏在实现内部，只通过API暴露操作
- **接口与实现分离**：用户只需要知道如何使用，不需要了解内部实现

**数据抽象的优势**：
- **模块化**：将复杂问题分解为独立的模块
- **可重用性**：同一个数据类型可以在多个程序中使用
- **可维护性**：修改实现不会影响使用该类型的代码
- **调试简化**：问题可以定位到特定的数据类型实现中
### 1.6 面向对象编程
面向对象编程是一种编程范式，它使用对象来设计应用程序和计算机程序。

**面向对象编程的优势**：
- **代码复用**：允许我们通过模块化编程复用代码，通过继承和组合实现代码重用
- **灵活的数据结构**：使我们可以轻易构造多种所谓的链式数据结构（如链表、树、图），它们比数组更灵活，在许多情况下都是高效算法的基础
- **问题建模**：借助它可以准确地定义所面对的算法问题，将现实世界的概念映射为程序中的对象

**面向对象的核心概念**：
- **封装（Encapsulation）**：将数据和操作数据的方法绑定在一起
- **继承（Inheritance）**：子类可以继承父类的属性和方法
- **多态（Polymorphism）**：同一个接口可以有多种不同的实现
- **抽象（Abstraction）**：隐藏复杂的实现细节，只暴露必要的接口
## 2.数据抽象

### 2.0 数据抽象概述
- **数据抽象**：定义和使用数据类型的过程，是现代编程的基础概念之一
- **抽象数据类型(ADT)**：一种能够对使用者隐藏数据表示的数据类型，只暴露操作接口
- **核心思想**：将数据和函数的实现关联，并将数据的表示方式隐藏起来，实现接口与实现的分离

**数据抽象的层次结构**：
```
应用程序
    ↓
抽象数据类型 API
    ↓  
数据类型实现
    ↓
基础编程模型（Java语言）
```
### 2.1 使用抽象数据类型
- **黑盒使用**：要使用一种数据类型并不一定非得知道它是如何实现的，这是抽象的核心价值
- **API规范**：应用程序编程接口(API)来说明抽象数据类型的行为，定义了可以执行的操作
- **方法分类**：
  - **静态方法**：主要作用是实现函数，不依赖于特定对象的状态
  - **非静态方法（实例方法）**：主要作用是实现数据类型的操作，操作特定对象的数据

**使用抽象数据类型的步骤**：
1. **声明**：创建该类型的变量
2. **构造**：调用构造函数创建对象
3. **操作**：调用实例方法操作对象
4. **测试**：验证对象的状态和行为
#### 2.1.1 赋值语句
对象的赋值语句不会创建一个新的对象，而只是创建另一个指向同一个对象的变量（引用拷贝）。
```java
Counter c1 = new Counter("steps");
Counter c2 = c1;  // c2和c1指向同一个Counter对象
c2.increment();   // 会影响c1指向的对象
```

#### 2.1.2 将对象作为参数
可以将对象作为参数传递给方法，Java中对象参数是**按值传递**的：
- 传递的是对象引用的副本，而不是对象本身的副本
- 方法无法改变调用端变量的引用值（不能让变量指向新对象）
- 但可以通过引用修改对象的内部状态

```java
public static void increment(Counter c) {
    c.increment();  // 可以修改对象状态
    c = new Counter("new");  // 不会影响调用端的变量
}
```

#### 2.1.3 对象数组
对象数组是一个由对象的引用组成的数组，而非所有对象本身组成的数组。
```java
Counter[] counters = new Counter[N];  // 创建引用数组
for (int i = 0; i < N; i++) {
    counters[i] = new Counter("counter" + i);  // 创建实际对象
}
```
### 2.2 抽象数据类型的实现

#### 2.2.1 实现要点
- **不可变性**：如果值在初始化后不应该再被改变，会使用`final`关键字修饰
- **构造函数**：构造函数的名称总是和类名相同，用于初始化对象状态
- **封装性**：API的作用是将使用和实现分离，以实现模块化编程

#### 2.2.2 实现模板
```java
public class DataType {
    // 实例变量（通常为private）
    private int value;
    private final String name;
    
    // 构造函数
    public DataType(String name, int value) {
        this.name = name;
        this.value = value;
    }
    
    // 实例方法
    public void method1() { /* 实现 */ }
    public int method2() { /* 实现 */ }
    
    // toString方法（推荐实现）
    public String toString() {
        return name + ": " + value;
    }
}
```

#### 2.2.3 设计原则
- **单一职责**：每个类应该只有一个改变的理由
- **开闭原则**：对扩展开放，对修改封闭
- **里氏替换**：子类应该能够替换父类
- **接口隔离**：不应该强迫客户依赖它们不使用的方法
### 2.3 数据类型的设计

#### 2.3.1 封装
使用数据类型的实现封装数据，以简化实现和隔离用例开发。封装实现了模块化编程。

**封装的好处**：
- **信息隐藏**：隐藏实现细节，只暴露必要的接口
- **降低耦合**：减少模块间的依赖关系
- **提高内聚**：相关的数据和操作组织在一起
- **便于维护**：修改实现不影响使用该类型的代码

#### 2.3.2 设计准则
- **API最小化**：只包含客户端需要的操作
- **实现独立性**：多种实现方式应该都能满足同一个API
- **对象不可变性**：尽可能设计不可变对象，提高安全性
- **一致性**：相似的操作应该有相似的名称和行为

#### 2.3.3 Java程序结构
每个**Java**程序都是一组静态方法和一种数据类型的实现的集合。程序的组织方式：
- **库**：静态方法的集合
- **数据类型**：非静态方法的集合，定义数据类型的操作
- **客户端**：使用库和数据类型的程序
### 2.4 接口继承
接口继承使得我们的程序能够通过调用接口中的方法操作实现该接口的任意类型的对象。

**接口继承的优势**：
- **多态性**：同一个接口可以有多种不同的实现
- **代码复用**：可以编写通用的代码来处理实现同一接口的不同类型
- **扩展性**：可以轻松添加新的实现而不影响现有代码

```java
public interface Comparable<Item> {
    public int compareTo(Item that);
}

public class Date implements Comparable<Date> {
    public int compareTo(Date that) {
        // 实现比较逻辑
    }
}
```

### 2.5 封装类型
Java提供了一些内置的引用类型，称为**封装类型**（包装类型）。每个原始数据类型都有一个对应的封装类型：

| 原始类型 | 封装类型 |
|---------|---------|
| boolean | Boolean |
| byte    | Byte    |
| char    | Character |
| double  | Double  |
| float   | Float   |
| int     | Integer |
| long    | Long    |
| short   | Short   |

**封装类型的用途**：
- 在需要对象的地方使用（如泛型、集合）
- 提供有用的静态方法和常量
- 支持自动装箱和拆箱

### 2.6 内存管理
Java具有自动内存管理机制：
- **对象创建**：使用`new`关键字在堆内存中创建对象
- **垃圾回收**：对象在离开作用域后会变成孤儿，无法被引用的对象也会变成孤儿，Java的垃圾回收器会自动清除这些孤儿对象
- **内存泄漏预防**：程序员不需要手动释放内存，但仍需注意避免不必要的对象引用

### 2.7 契约式设计
契约式设计是一种程序设计方法，通过明确的前置条件、后置条件和不变式来规范程序行为。

**实现方式**：
- **异常处理**：使用异常来处理违反前置条件的情况
- **断言**：使用断言来检查程序的内部状态
- **文档**：在API文档中明确说明方法的契约

```java
public void withdraw(double amount) {
    if (amount <= 0) 
        throw new IllegalArgumentException("Amount must be positive");
    if (amount > balance) 
        throw new IllegalArgumentException("Insufficient funds");
    
    balance -= amount;
    
    assert balance >= 0 : "Balance should never be negative";
}
```

大量使用异常和断言是很好的编程实践，有助于：
- **早期发现错误**：在问题发生时立即检测到
- **明确责任**：清楚地定义谁负责检查什么条件
- **提高可靠性**：确保程序在预期的条件下运行
## 3.背包、队列和栈

### 3.0 集合类型概述
背包、队列和栈都是集合类型，它们的不同之处在于删除或者访问对象的顺序不同。

**三种基本集合类型的特点**：
- **背包（Bag）**：只能添加元素，不能删除元素，访问顺序不重要
- **队列（Queue）**：先进先出（FIFO），在一端添加元素，在另一端删除元素
- **栈（Stack）**：后进先出（LIFO），在同一端添加和删除元素

**共同特征**：
- 都是用来存储和组织数据的容器
- 都支持基本的集合操作（添加、删除、检查是否为空等）
- 都可以实现为泛型类型，支持任意数据类型
- 都可以实现迭代功能
### 3.1.API:
#### 3.1.1.泛型：
例如:
`public class Bag<Item> implements Iterable<Item>`:
- 其中Item将定义为一个类型参数，一个象征性的占位符。
#### 3.1.2 自动装箱
在处理赋值语句、方法的参数或逻辑表达式时，Java会自动在引用类型和对应的原始数据类型之间进行切换。

**术语纠正和详细说明**：
- **自动装箱（Autoboxing）**：自动将一个原始数据类型（如int）转换为对应的封装类型（如Integer）
- **自动拆箱（Auto-unboxing）**：自动将一个封装类型（如Integer）转换为对应的原始数据类型（如int）

**示例**：
```java
Stack<Integer> stack = new Stack<Integer>();
stack.push(17);        // 自动装箱：int -> Integer
int i = stack.pop();   // 自动拆箱：Integer -> int

// 等价于：
stack.push(Integer.valueOf(17));
int i = stack.pop().intValue();
```

**注意事项**：
- 自动装箱/拆箱会带来性能开销
- 可能导致意外的NullPointerException
- 在比较时要注意使用equals()而不是==
#### 3.1.3 可迭代的集合类型
如果集合类型是可迭代的，用例可以用一行语句即可遍历所有元素：

```java
for (Transaction t : collection) {
    StdOut.println(t);
}
```

**实现可迭代接口的要求**：
1. 集合类必须实现`Iterable<Item>`接口
2. 必须实现`iterator()`方法，返回一个`Iterator<Item>`对象
3. Iterator必须实现`hasNext()`和`next()`方法

**完整示例**：
```java
import java.util.Iterator;

public class Bag<Item> implements Iterable<Item> {
    // ... 其他代码
    
    public Iterator<Item> iterator() {
        return new ListIterator();
    }
    
    private class ListIterator implements Iterator<Item> {
        private Node current = first;
        
        public boolean hasNext() {
            return current != null;
        }
        
        public Item next() {
            Item item = current.item;
            current = current.next;
            return item;
        }
    }
}
```
#### 3.1.4 背包（Bag）
背包是一种不支持从中删除元素的集合数据类型，与顺序无关。

**背包的特点**：
- **只能添加**：支持添加元素，但不支持删除元素
- **顺序无关**：不关心元素的添加顺序，只关心是否包含某个元素
- **用途**：用于收集元素并迭代处理所有元素，当顺序不重要时使用

**背包API**：
```java
public class Bag<Item> implements Iterable<Item>
    Bag()                    // 创建一个空背包
    void add(Item item)      // 添加一个元素
    boolean isEmpty()        // 背包是否为空
    int size()              // 背包中元素的数量
```

#### 3.1.5 队列（先进先出）
队列遵循FIFO（First In First Out）原则，先进入的元素先被删除。

**队列的特点**：
- **两端操作**：在队尾添加元素（enqueue），在队头删除元素（dequeue）
- **公平性**：保证元素按照到达的顺序被处理
- **应用场景**：任务调度、广度优先搜索、缓冲区管理

**队列API**：
```java
public class Queue<Item> implements Iterable<Item>
    Queue()                     // 创建空队列
    void enqueue(Item item)     // 添加元素到队尾
    Item dequeue()              // 删除并返回队头元素
    boolean isEmpty()           // 队列是否为空
    int size()                 // 队列中元素数量
```

#### 3.1.6 下压栈（Stack）
栈遵循LIFO（Last In First Out）原则，最后进入的元素最先被删除。

**栈的特点**：
- **单端操作**：在同一端（栈顶）进行添加和删除操作
- **递归性质**：天然支持递归算法的实现
- **应用场景**：函数调用、表达式求值、深度优先搜索、撤销操作

**栈API**：
```java
public class Stack<Item> implements Iterable<Item>
    Stack()                  // 创建空栈
    void push(Item item)     // 将元素压入栈顶
    Item pop()              // 删除并返回栈顶元素
    boolean isEmpty()        // 栈是否为空
    int size()              // 栈中元素数量
```
#### 3.1.7 算术表达式求值（Dijkstra的双栈算法）
使用两个栈来计算完全括号化的算术表达式的值。

**算法步骤**：
1. **操作数**：将操作数压入操作数栈
2. **运算符**：将运算符压入运算符栈  
3. **左括号**：忽略左括号
4. **右括号**：在遇到右括号时，弹出一个运算符，弹出所需数量的操作数，并将运算符和操作数的运算结果压入操作数栈

**算法实现示例**：
```java
public class Evaluate {
    public static void main(String[] args) {
        Stack<String> ops = new Stack<String>();
        Stack<Double> vals = new Stack<Double>();
        
        while (!StdIn.isEmpty()) {
            String s = StdIn.readString();
            if      (s.equals("("))               ;
            else if (s.equals("+")) ops.push(s);
            else if (s.equals("-")) ops.push(s);
            else if (s.equals("*")) ops.push(s);
            else if (s.equals("/")) ops.push(s);
            else if (s.equals(")")) {
                String op = ops.pop();
                double v = vals.pop();
                if      (op.equals("+")) v = vals.pop() + v;
                else if (op.equals("-")) v = vals.pop() - v;
                else if (op.equals("*")) v = vals.pop() * v;
                else if (op.equals("/")) v = vals.pop() / v;
                vals.push(v);
            }
            else vals.push(Double.parseDouble(s));
        }
        StdOut.println(vals.pop());
    }
}
```

**示例**：表达式`(1+((2+3)*(4*5)))`的计算过程：
- 遇到数字压入操作数栈
- 遇到运算符压入运算符栈
- 遇到右括号时执行一次运算
### 3.2.集合类数据类型的实现：
#### 3.2.1 定容栈
定容栈使用数组来存储元素，具有固定的容量上限。

**定容栈的特点**：
- **时间复杂度**：push和pop操作所需的时间独立于栈的长度，都是常数时间O(1)
- **空间效率**：使用数组存储，内存使用紧凑
- **容量限制**：需要预先指定最大容量，无法动态扩展

**实现示例**：
```java
public class FixedCapacityStackOfStrings {
    private String[] a;  // 栈元素
    private int N;       // 元素数量
    
    public FixedCapacityStackOfStrings(int cap) {
        a = new String[cap];
    }
    
    public boolean isEmpty() { return N == 0; }
    public int size() { return N; }
    
    public void push(String item) {
        a[N++] = item;
    }
    
    public String pop() {
        return a[--N];
    }
}
```

**缺点**：
- 容量固定，可能浪费空间或容量不足
- 只能存储特定类型（如String），不够通用
#### 3.2.2 泛型
泛型使得我们可以创建适用于任何数据类型的集合类。

**泛型的优势**：
- **类型安全**：在编译时检查类型错误
- **代码复用**：同一个类可以处理不同类型的数据
- **性能提升**：避免了类型转换的开销

**泛型栈实现**：
```java
public class FixedCapacityStack<Item> {
    private Item[] a;    // 栈元素
    private int N;       // 元素数量
    
    @SuppressWarnings("unchecked")
    public FixedCapacityStack(int cap) {
        a = (Item[]) new Object[cap];  // 泛型数组创建的变通方法
    }
    
    public boolean isEmpty() { return N == 0; }
    public int size() { return N; }
    
    public void push(Item item) {
        a[N++] = item;
    }
    
    public Item pop() {
        return a[--N];
    }
}
```

**注意事项**：
- Java不允许创建泛型数组，需要使用类型转换
- 使用`@SuppressWarnings("unchecked")`来抑制编译器警告
#### 3.2.3 调整数组大小
为了解决定容栈的容量限制问题，我们需要实现动态调整数组大小的机制。

**问题分析**：
- **容量估计困难**：选择用数组表示栈内存意味着必须预先估计栈的最大容量
- **内存浪费**：选择大容量的用例在栈为空或者几乎为空时会浪费大量的内存
- **容量不足**：选择小容量可能导致栈溢出

**动态调整策略**：
- **扩容**：在push()中，检查数组是否太小，如果没有多余空间，将数组长度加倍
- **缩容**：在pop()中，检查数组是否太大，当元素数量小于数组长度的四分之一时，将数组长度缩减为原来的一半

**为什么是四分之一而不是一半？**
这是为了避免频繁push()和pop()造成的性能抖动（thrashing）。如果在一半时就缩容，那么在临界点附近的push/pop操作会导致频繁的扩容/缩容。

**实现示例**：
```java
public void push(Item item) {
    if (N == a.length) resize(2 * a.length);  // 扩容
    a[N++] = item;
}

public Item pop() {
    Item item = a[--N];
    a[N] = null;  // 避免对象游离
    if (N > 0 && N == a.length/4) resize(a.length/2);  // 缩容
    return item;
}

private void resize(int max) {
    Item[] temp = (Item[]) new Object[max];
    for (int i = 0; i < N; i++) {
        temp[i] = a[i];
    }
    a = temp;
}
```

**性能分析**：
- **均摊时间复杂度**：虽然单次resize操作需要O(N)时间，但均摊下来每次操作仍是O(1)
- **空间利用率**：数组使用率始终保持在25%到100%之间
#### 3.2.4 对象游离
对象游离是使用数组实现集合类时需要特别注意的内存管理问题。

**问题描述**：
Java的垃圾收集策略是回收所有无法被访问的对象的内存。但是使用简单的pop()实现：
```java
public Item pop() { return a[--N]; }
```
弹出的元素实际上已经是孤儿了，但Java的垃圾收集器没法知道这一点，因为数组中的引用仍然可以让它继续存在，这种情况称为**对象游离**（loitering）。

**问题影响**：
- **内存泄漏**：被弹出的对象无法被垃圾回收，占用内存
- **性能下降**：大量游离对象会影响垃圾收集器的效率
- **内存溢出**：长期运行可能导致内存不足

**解决方案**：
为了处理游离，需要将被弹出的数组元素的值设为null，显式地断开引用：
```java
public Item pop() {
    Item item = a[--N];
    a[N] = null;  // 避免对象游离
    return item;
}
```

**最佳实践**：
- 在删除元素后立即将数组位置设为null
- 在resize()方法中也要注意清理不再使用的数组位置
- 这种做法对于原始数据类型不是必需的，但对于对象引用是必需的
#### 3.2.5 迭代
要使一个类可以迭代，需要实现Java的迭代器模式。

**实现步骤**：
1. **实现接口**：在类声明中加入`implements Iterable<Item>`
2. **添加iterator()方法**：在类中添加一个方法`iterator()`并返回一个迭代器`Iterator<Item>`
3. **实现Iterator接口**：创建一个内部类实现`Iterator<Item>`接口，实现`hasNext()`和`next()`方法

**完整实现示例**：
```java
import java.util.Iterator;

public class ResizingArrayStack<Item> implements Iterable<Item> {
    private Item[] a = (Item[]) new Object[1];
    private int N = 0;
    
    // ... push(), pop(), isEmpty(), size()等方法
    
    public Iterator<Item> iterator() {
        return new ReverseArrayIterator();
    }
    
    private class ReverseArrayIterator implements Iterator<Item> {
        private int i = N;
        
        public boolean hasNext() {
            return i > 0;
        }
        
        public Item next() {
            return a[--i];
        }
        
        public void remove() {
            throw new UnsupportedOperationException();
        }
    }
}
```

**使用示例**：
```java
ResizingArrayStack<String> stack = new ResizingArrayStack<String>();
// ... 添加元素

// 使用增强for循环
for (String s : stack) {
    StdOut.println(s);
}

// 使用迭代器
Iterator<String> iter = stack.iterator();
while (iter.hasNext()) {
    StdOut.println(iter.next());
}
```

**注意事项**：
- 迭代器通常不支持remove()操作，抛出UnsupportedOperationException
- 栈的迭代器按照LIFO顺序返回元素（最近添加的元素最先返回）
- 迭代过程中不应该修改集合，否则可能导致不可预期的行为
### 3.3 链表
链表是一种递归的数据结构，它或者为空(null)，或者是含有泛型元素的结点和指向另一条链表的引用。

**链表的特点**：
- **动态大小**：可以在运行时动态增长和缩小
- **内存效率**：只分配实际需要的内存
- **插入删除高效**：在已知位置插入或删除元素只需常数时间
- **顺序访问**：只能从头开始顺序访问元素，不支持随机访问

**链表 vs 数组**：

| 特性 | 链表 | 数组 |
|------|------|------|
| 内存分配 | 动态 | 静态（或需要resize） |
| 随机访问 | O(N) | O(1) |
| 插入/删除 | O(1) | O(N) |
| 内存开销 | 每个节点额外存储指针 | 紧凑存储 |
| 缓存友好性 | 较差 | 较好 |
#### 3.3.1 结点记录
链表的基本构建块是结点（Node），每个结点包含数据和指向下一个结点的引用。

```java
private class Node {
    Item item;  // 存储的数据
    Node next;  // 指向下一个结点的引用
}
```

**结点的特点**：
- **私有内部类**：通常定义为私有内部类，外部无法直接访问
- **两个字段**：数据字段存储实际内容，引用字段指向下一个结点
- **递归结构**：每个结点都包含指向同类型结点的引用

**内存表示**：
```
[item|next] -> [item|next] -> [item|next] -> null
```

**结点操作的基本模式**：
- **创建结点**：`Node node = new Node();`
- **设置数据**：`node.item = value;`
- **链接结点**：`node.next = otherNode;`
#### 3.3.2 构造链表
手动构造一个包含三个结点的链表示例：

```java
Node first = new Node();
Node second = new Node();
Node third = new Node();

first.item = "to";
second.item = "be";
third.item = "or";

first.next = second;
second.next = third;
third.next = null;  // 最后一个结点指向null
```

**链表结构图**：
```
first -> [to|•] -> [be|•] -> [or|null]
```

**遍历链表**：
```java
for (Node x = first; x != null; x = x.next) {
    // 处理x.item
    StdOut.println(x.item);
}
```

**链表的基本操作模式**：
- **遍历**：从first开始，通过next引用依次访问每个结点
- **查找**：遍历链表直到找到目标元素或到达链表末尾
- **计数**：遍历链表并统计结点数量
#### 3.3.3 链表的基本操作

**在表头插入结点**：
```java
Node oldfirst = first;
first = new Node();
first.item = "new item";
first.next = oldfirst;
```

**从表头删除结点**：
```java
first = first.next;
```

**在表尾插入结点**：
```java
Node oldlast = last;
last = new Node();
last.item = "new item";
last.next = null;
if (isEmpty()) {
    first = last;
} else {
    oldlast.next = last;
}
```

**从表尾删除结点**：
这个操作比较复杂，需要遍历到倒数第二个结点：
```java
if (first.next == null) {
    first = null;  // 只有一个结点
} else {
    Node x = first;
    while (x.next.next != null) {
        x = x.next;  // 找到倒数第二个结点
    }
    x.next = null;  // 删除最后一个结点
}
```

**操作复杂度**：
- 在表头插入/删除：O(1)
- 在表尾插入：O(1)（如果维护last引用）
- 在表尾删除：O(N)（需要遍历到倒数第二个结点）
- 在任意位置插入/删除：O(N)（需要先找到位置）
#### 3.3.4.栈的实现：
将栈保存为一条链表，栈的顶部即为表头，实例变量first指向栈顶。
- push压入元素时，将元素添加到表头；
- pop删除元素时，将元素从表头删除。
**下压堆栈**（链表实现）
```Java
public class Stack<Item> implements Iterable<Item>
{
	private Node first; // 栈顶（最近添加的元素）
	private int N; // 元素数量
	private class Node
	{ // 定义了结点的嵌套类
		Item item;
		Node next;
	}
	public boolean isEmpty() { return first == null; } // 或：N == 0
	public int size() { return N; }	
	public void push(Item item)
	{ // 向栈顶添加元素
		Node oldfirst = first;
		first = new Node();
		first.item = item;
		first.next = oldfirst;
		N++;
	}
	public Item pop()
	{ //从栈顶删除元素
		Item item = first.item;
		first = first.next;
		N--;
		return item;
	}

// iterator()的实现请见算法 1.4

// 测试用例 main()的实现请见本节前面部分

}
```
#### 3.3.5.队列的实现：
先进先出
```Java
public class Queue<Item> implements Iterable<Item>
{
	private Node first; // 指向最早添加的结点的链接
	private Node last; // 指向最近添加的结点的链接
	private int N; // 队列中的元素数量
	private class Node	
	{ // 定义了结点的嵌套类
		Item item;
		Node next;
	}
	public boolean isEmpty() { return first == null; } // 或： N == 0.
	public int size() { return N; }
	public void enqueue(Item item;	
	{ // 向表尾添加元素
		Node oldlast = last;
		last = new Node();
		last.item = item;
		last.next = null;
		if (isEmpty()) first = last;
		else oldlast.next = last;
		N++;
	}
	public Item dequeue()
	{ // 从表头删除元素
		Item item = first.item;
		first = first.next;
		if (isEmpty()) last = null;
		N--;
		return item;
	}
// iterator()的实现请见算法 1.4
// 测试用例 main()的实现请见前面
}
```
在结构化存储数据集时，**链表是数组的一种重要的替代方式**。
#### 3.3.6 背包的实现
用链表数据结构实现Bag API只需要将Stack中的push()改名为add()，然后去掉pop()的实现即可。

**背包的链表实现**：
```java
import java.util.Iterator;

public class Bag<Item> implements Iterable<Item> {
    private Node first;  // 链表的首结点
    private int N;       // 背包中元素的数量
    
    private class Node {
        Item item;
        Node next;
    }
    
    public Bag() {
        first = null;
        N = 0;
    }
    
    public boolean isEmpty() {
        return first == null;
    }
    
    public int size() {
        return N;
    }
    
    public void add(Item item) {
        Node oldfirst = first;
        first = new Node();
        first.item = item;
        first.next = oldfirst;
        N++;
    }
    
    public Iterator<Item> iterator() {
        return new ListIterator();
    }
    
    private class ListIterator implements Iterator<Item> {
        private Node current = first;
        
        public boolean hasNext() {
            return current != null;
        }
        
        public Item next() {
            Item item = current.item;
            current = current.next;
            return item;
        }
        
        public void remove() {
            throw new UnsupportedOperationException();
        }
    }
}
```

**背包的特点**：
- **只能添加**：只提供add()方法，不提供删除方法
- **迭代访问**：可以遍历所有元素，但顺序可能与添加顺序相反
- **简单实现**：比栈和队列更简单，因为不需要删除操作
## 4.算法分析

### 4.1 科学方法
算法分析采用科学方法来研究算法的性能特征。

**科学方法的核心原则**：
- **可重现性**：我们所设计的实验必须是**可重现的**，这样他人可以自己验证假设的真实性
- **可证伪性**：所有的假设必须是**可证伪的**，这样才能确认某个假设是错误的（并需要修正）

**算法分析的科学方法步骤**：
1. **观察**：运行程序，测量运行时间
2. **假设**：基于观察提出数学模型
3. **预测**：使用模型预测未来的性能
4. **验证**：通过实验验证预测的准确性
5. **验证或反驳假设**：根据实验结果确认或修正模型

**实验设计原则**：
- **控制变量**：每次只改变一个变量
- **多次测量**：进行多次实验以减少随机误差
- **数据可视化**：使用图表来展示实验结果
- **统计分析**：使用统计方法分析数据的可靠性
### 4.3 数学模型
数学模型帮助我们理解和预测算法的性能。

**程序运行时间的决定因素**：
一个程序运行的总时间主要和两点有关：
- **执行每条语句的耗时**：不同操作的基本耗时
- **执行每条语句的频率**：每条语句被执行的次数

**数学模型的构建**：
```
总运行时间 = Σ(每条语句的耗时 × 该语句的执行频率)
```

**近似模型**：
在实际分析中，我们通常关注：
- **主导项**：对总运行时间影响最大的项
- **增长数量级**：忽略低阶项和常数因子
- **最坏情况**：算法在最不利输入下的性能

**成本模型**：
为了简化分析，我们通常选择一个或几个基本操作作为成本模型：
- **数组访问**：访问数组元素的次数
- **比较操作**：比较两个元素的次数
- **交换操作**：交换两个元素的次数

**示例 - 三数之和问题**：
```java
int count = 0;
for (int i = 0; i < N; i++)           // 执行 N+1 次
    for (int j = i+1; j < N; j++)     // 执行 ~N²/2 次
        for (int k = j+1; k < N; k++) // 执行 ~N³/6 次
            if (a[i] + a[j] + a[k] == 0)
                count++;
```
- **数组访问次数**：~N³/2（主导项）
- **时间复杂度**：O(N³)
### 4.4 增长数量级的分类
算法的性能通常用增长数量级来分类，这里按照从快到慢的顺序列出：

| 描述 | 增长数量级 | 典型代码框架 | 说明 | 举例 |
|------|------------|--------------|------|------|
| 常数级别 | 1 | a = b + c; | 语句数量固定 | 将两个数相加 |
| 对数级别 | log N | 二分查找 | 将问题规模减半 | 二分查找 |
| 线性级别 | N | 单层循环 | 循环 | 找出最大元素 |
| 线性对数级别 | N log N | 分治算法 | 将问题分解 | 归并排序 |
| 平方级别 | N² | 双层循环 | 双重循环 | 检查所有元素对 |
| 立方级别 | N³ | 三层循环 | 三重循环 | 检查所有三元组 |
| 指数级别 | 2^N | 穷举搜索 | 穷举所有子集 | 检查所有子集 |

**增长数量级的比较**：
当N=1000时的近似值：
- 常数：1
- 对数：10
- 线性：1,000
- 线性对数：10,000
- 平方：1,000,000
- 立方：1,000,000,000
- 指数：2^1000（天文数字）

**实际意义**：
- **常数到线性对数**：通常是可接受的
- **平方级别**：对于小到中等规模的问题可接受
- **立方及以上**：通常只适用于小规模问题
- **指数级别**：通常不实用，除非N很小

**大O记号**：
- O(1)：常数时间
- O(log N)：对数时间
- O(N)：线性时间
- O(N log N)：线性对数时间
- O(N²)：平方时间
- O(N³)：立方时间
- O(2^N)：指数时间
### 4.6 倍率实验
倍率实验是一种实用的性能分析方法，通过观察输入规模加倍时运行时间的变化来确定算法的增长数量级。

**倍率实验的原理**：
如果T(N) ~ aN^b，那么T(2N)/T(N) ~ 2^b

**实验步骤**：
1. **选择基准规模**：选择一个合适的问题规模N
2. **测量运行时间**：记录算法在规模N下的运行时间T(N)
3. **加倍规模**：将问题规模加倍为2N
4. **再次测量**：记录算法在规模2N下的运行时间T(2N)
5. **计算倍率**：计算T(2N)/T(N)
6. **重复实验**：继续加倍规模，重复上述过程

**倍率与增长数量级的关系**：

| 增长数量级 | 倍率 T(2N)/T(N) |
|------------|-----------------|
| 1 | 1 |
| log N | ~1 |
| N | 2 |
| N log N | ~2 |
| N² | 4 |
| N³ | 8 |
| 2^N | T(N) |

**实验示例**：
```java
public class DoublingTest {
    public static double timeTrial(int N) {
        // 生成N个随机数并计算三数之和为0的数量
        int[] a = new int[N];
        for (int i = 0; i < N; i++) {
            a[i] = StdRandom.uniform(-1000000, 1000000);
        }
        Stopwatch timer = new Stopwatch();
        int count = ThreeSum.count(a);
        return timer.elapsedTime();
    }
    
    public static void main(String[] args) {
        for (int N = 250; true; N += N) {
            double time = timeTrial(N);
            StdOut.printf("%6d %7.1f\n", N, time);
        }
    }
}
```

**倍率实验的优势**：
- **简单易行**：不需要复杂的数学分析
- **实际准确**：反映真实的运行环境
- **快速判断**：能够快速确定算法的增长数量级
- **预测性能**：可以预测更大规模问题的运行时间
### 4.9 内存使用分析
了解Java中数据结构的内存使用对于性能优化很重要。

**Java对象的内存开销**：
- **对象头**：每个Java对象都有16字节的对象头（包含类信息、标记字段等）
- **实例变量**：根据类型占用不同字节数
- **填充字节**：Java要求对象大小是8字节的倍数，不足的部分用填充字节补齐
- **引用**：对象的外部引用是8字节（64位系统上的指针）

**基本数据类型的内存占用**：

| 类型 | 字节数 |
|------|--------|
| boolean | 1 |
| byte | 1 |
| char | 2 |
| int | 4 |
| float | 4 |
| long | 8 |
| double | 8 |

**数组的内存使用**：
- **数组对象头**：16字节
- **长度字段**：4字节
- **填充**：4字节（使总大小为8的倍数）
- **数组元素**：N × 每个元素的大小

**示例计算**：
```java
// int数组的内存使用
int[] a = new int[N];
// 总内存 = 16(对象头) + 4(长度) + 4(填充) + N×4(元素) = 24 + 4N 字节
```

**链表结点的内存使用**：
```java
class Node {
    Item item;  // 8字节（引用）
    Node next;  // 8字节（引用）
}
// 每个Node对象：16(对象头) + 8(item) + 8(next) = 32字节
```

**内存使用的影响**：
- **缓存性能**：数组比链表更缓存友好
- **空间效率**：需要考虑对象开销
- **垃圾回收**：更多对象意味着更多的垃圾回收压力

### 4.10 展望：算法设计与分析的指导原则

**性能优化的平衡原则**：

#### 1. 正确性优先原则
**不要过于关注算法优化，首要任务是写出清晰正确的代码。**
- **可读性**：代码应该易于理解和维护
- **正确性**：确保程序逻辑正确，没有bug
- **简洁性**：避免过度复杂的优化
- **专业分工**：仅仅为了提高运行速度而修改程序的事最好留给专家来做

#### 2. 性能意识原则  
**不要完全忽视程序的性能。**
- **了解复杂度**：理解算法的时间和空间复杂度
- **识别瓶颈**：知道程序中哪些部分是性能瓶颈
- **合理优化**：在必要时进行有针对性的优化
- **测量验证**：通过实际测量来验证优化效果

**实践建议**：
- **先实现，后优化**：遵循"过早优化是万恶之源"的原则
- **80/20法则**：80%的时间花在20%的代码上，找到这20%
- **选择合适的数据结构**：根据使用模式选择最适合的数据结构
- **避免明显的低效**：避免不必要的嵌套循环和重复计算

**算法学习的价值**：
- **思维训练**：培养分析问题和解决问题的能力
- **工具箱**：掌握常用算法和数据结构
- **性能直觉**：培养对程序性能的直觉
- **设计能力**：提高系统设计和架构能力
## 5. Union-Find算法

### 5.1 动态连通性问题
Union-Find算法解决的是动态连通性问题：给定一组N个对象，支持两种操作：
- **连接两个对象**：union(p, q)
- **判断两个对象是否连通**：connected(p, q)

**连通性的性质**：
- **自反性**：p和p是连通的
- **对称性**：如果p和q连通，那么q和p也连通  
- **传递性**：如果p和q连通，q和r连通，那么p和r也连通

**应用场景**：
- **网络连通性**：计算机网络中的连通性
- **社交网络**：判断两个人是否在同一个社交圈
- **图像处理**：像素的连通区域
- **编译器**：变量名的等价性

### 5.2 Union-Find API
```java
public class UF {
    UF(int N)                // 初始化N个触点
    void union(int p, int q) // 在p和q之间添加连接
    int find(int p)          // p所在的连通分量的标识符
    boolean connected(int p, int q) // 如果p和q存在于同一个连通分量中则返回true
    int count()              // 连通分量的数量
}
```

### 5.3 Quick-Find算法
**核心思想**：维护一个数组，相同连通分量中的所有触点都有相同的id值。

```java
public class QuickFindUF {
    private int[] id;    // 分量id（以触点作为索引）
    private int count;   // 分量数量
    
    public QuickFindUF(int N) {
        count = N;
        id = new int[N];
        for (int i = 0; i < N; i++)
            id[i] = i;
    }
    
    public int count() { return count; }
    
    public boolean connected(int p, int q) {
        return find(p) == find(q);
    }
    
    public int find(int p) {
        return id[p];
    }
    
    public void union(int p, int q) {
        int pID = find(p);
        int qID = find(q);
        
        if (pID == qID) return;
        
        // 将p的分量重命名为q的分量名称
        for (int i = 0; i < id.length; i++)
            if (id[i] == pID) id[i] = qID;
        count--;
    }
}
```

**性能分析**：
- **find操作**：O(1)
- **union操作**：O(N)
- **总体**：处理N个对象的M次操作需要O(MN)时间

### 5.4 Quick-Union算法
**核心思想**：用森林表示连通分量，每个触点的id值是同一个分量中另一个触点的名称。

```java
public class QuickUnionUF {
    private int[] id;    // 父链接数组（以触点作为索引）
    private int count;   // 分量数量
    
    public QuickUnionUF(int N) {
        count = N;
        id = new int[N];
        for (int i = 0; i < N; i++) id[i] = i;
    }
    
    public int count() { return count; }
    
    public boolean connected(int p, int q) {
        return find(p) == find(q);
    }
    
    public int find(int p) {
        // 找到根触点
        while (p != id[p]) p = id[p];
        return p;
    }
    
    public void union(int p, int q) {
        int pRoot = find(p);
        int qRoot = find(q);
        if (pRoot == qRoot) return;
        
        id[pRoot] = qRoot;
        count--;
    }
}
```

**性能分析**：
- **最好情况**：find和union都是O(1)
- **最坏情况**：树退化为链表，操作变为O(N)

### 5.5 加权Quick-Union算法
**改进思想**：总是将较小的树连接到较大的树的根节点上。

```java
public class WeightedQuickUnionUF {
    private int[] id;    // 父链接数组（以触点作为索引）
    private int[] sz;    // 各个根节点对应的分量的大小（以触点作为索引）
    private int count;   // 分量数量
    
    public WeightedQuickUnionUF(int N) {
        count = N;
        id = new int[N];
        sz = new int[N];
        for (int i = 0; i < N; i++) {
            id[i] = i;
            sz[i] = 1;
        }
    }
    
    public int count() { return count; }
    
    public boolean connected(int p, int q) {
        return find(p) == find(q);
    }
    
    public int find(int p) {
        while (p != id[p]) p = id[p];
        return p;
    }
    
    public void union(int p, int q) {
        int i = find(p);
        int j = find(q);
        if (i == j) return;
        
        // 将小树的根节点连接到大树的根节点
        if (sz[i] < sz[j]) { id[i] = j; sz[j] += sz[i]; }
        else               { id[j] = i; sz[i] += sz[j]; }
        count--;
    }
}
```
![[Pasted image 20251105205758.png]]

**性能分析**：
- **find操作**：O(log N)
- **union操作**：O(log N)
- **总体**：处理N个对象的M次操作需要O(M log N)时间

### 5.6 路径压缩优化
**进一步优化**：在find操作中，将路径上的所有节点都直接连接到根节点。

```java
public int find(int p) {
    if (p != id[p])
        id[p] = find(id[p]);  // 路径压缩
    return id[p];
}
```

**最终性能**：
- 加权quick-union + 路径压缩的均摊时间复杂度接近O(α(N))，其中α是阿克曼函数的反函数，在实际应用中可以认为是常数。
