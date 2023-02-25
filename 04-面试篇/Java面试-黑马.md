# Java面试专题

视频链接：[Java面试题视频教程，黑马程序员](https://www.bilibili.com/video/BV15b4y117RJ?vd_source=6e6b2286ee9a603d7bdb2bc5ba80e449)

+++

## 基础篇

> ***基础篇要点：算法、数据结构、基础设计模式***

### 1. 二分查找

**要求**

* 能够用自己语言描述二分查找算法
* 能够手写二分查找代码
* 能够解答一些变化后的考法

**算法描述**

1. 前提：有已排序数组 A（假设已经做好）

2. 定义左边界 L、右边界 R，确定搜索范围，循环执行二分查找（3、4两步）

3. 获取中间索引 M = Floor((L+R) / 2)

4. 中间索引的值 A[M] 与待搜索的值 T 进行比较

   ① A[M] == T 表示找到，返回中间索引

   ② A[M] > T，中间值右侧的其它元素都大于 T，无需比较，中间索引左边去找，M - 1 设置为右边界，重新查找

   ③ A[M] < T，中间值左侧的其它元素都小于 T，无需比较，中间索引右边去找， M + 1 设置为左边界，重新查找

5. 当 L > R 时，表示没有找到，应结束循环

> *更形象的描述请参考：binary_search.html*

**算法实现**

```java
public static int binarySearch(int[] a, int t) {
    int l = 0, r = a.length - 1, m;
    while (l <= r) {
        m = (l + r) / 2;
        if (a[m] == t) {
            return m;
        } else if (a[m] > t) {
            r = m - 1;
        } else {
            l = m + 1;
        }
    }
    return -1;
}
```

**测试代码**

```java
public static void main(String[] args) {
    int[] array = {1, 5, 8, 11, 19, 22, 31, 35, 40, 45, 48, 49, 50};
    int target = 47;
    int idx = binarySearch(array, target);
    System.out.println(idx);
}
```

**解决整数溢出问题**

当 l 和 r 都较大时，`l + r` 有可能超过整数范围，造成运算错误，解决方法有两种：

```java
int m = l + (r - l) / 2;
```

还有一种是：

```java
int m = (l + r) >>> 1; // 推荐
```

**其它考法**

1. 有一个有序表为 1,5,8,11,19,22,31,35,40,45,48,49,50 当二分查找值为 48 的结点时，查找成功需要比较的次数 

2. 使用二分法在序列 1,4,6,7,15,33,39,50,64,78,75,81,89,96 中查找元素 81 时，需要经过（4）次比较

3. 在拥有128个元素的数组中二分查找一个数，需要比较的次数最多不超过多少次 7

对于前两个题目，记得一个简要判断口诀：***奇数二分取中间，偶数二分取中间靠左***。

对于后一道题目，需要知道公式：$$n = log_2N = log_{10}N/log_{10}2$$

其中 n 为查找次数，N 为元素个数



### 2. 冒泡排序

**要求**

* 能够用自己语言描述冒泡排序算法
* 能够手写冒泡排序代码
* 了解一些冒泡排序的优化手段

**算法描述**

1. 依次比较数组中相邻两个元素大小，若 a[j] > a[j+1]，则交换两个元素，两两都比较一遍称为一轮冒泡，结果是让最大的元素排至最后
2. 重复以上步骤，直到整个数组有序

> *更形象的描述请参考：bubble_sort.html*

**算法实现**

```java
public static void bubble(int[] a) {
    for (int j = 0; j < a.length - 1; j++) {
        // 一轮冒泡
        boolean swapped = false; // 是否发生了交换
        for (int i = 0; i < a.length - 1 - j; i++) {
            System.out.println("比较次数" + i);
            if (a[i] > a[i + 1]) {
                Utils.swap(a, i, i + 1);
                swapped = true;
            }
        }
        System.out.println("第" + j + "轮冒泡"
                           + Arrays.toString(a));
        if (!swapped) {
            break;
        }
    }
}
```

* 优化点1：每经过一轮冒泡，内层循环就可以减少一次
* 优化点2：如果某一轮冒泡没有发生交换，则表示所有数据有序，可以结束外层循环

**进一步优化**

```java
public static void bubble_v2(int[] a) {
    int n = a.length - 1;
    while (true) {
        int last = 0; // 表示最后一次交换索引位置
        for (int i = 0; i < n; i++) {
            System.out.println("比较次数" + i);
            if (a[i] > a[i + 1]) {
                Utils.swap(a, i, i + 1);
                last = i;
            }
        }
        n = last;
        if (n == 0) {
            break;
        }
    }
}
```

* 每轮冒泡时，最后一次交换索引可以作为下一轮冒泡的比较次数，如果这个值为零，表示整个数组有序，直接退出外层循环即可



### 3. 选择排序

**要求**

* 能够用自己语言描述选择排序算法
* 能够比较选择排序与冒泡排序
* 理解非稳定排序与稳定排序

**算法描述**

1. 将数组分为两个子集，排序的和未排序的，每一轮从未排序的子集中选出最小的元素，放入排序子集

2. 重复以上步骤，直到整个数组有序

> *更形象的描述请参考：selection_sort.html*

**算法实现**

```java
public static void selection(int[] a) {
    for (int i = 0; i < a.length - 1; i++) {
        // i 代表每轮选择最小元素要交换到的目标索引
        int s = i; // 代表最小元素的索引
        for (int j = s + 1; j < a.length; j++) {
            if (a[s] > a[j]) { // j 元素比 s 元素还要小, 更新 s
                s = j;
            }
        }
        if (s != i) {
            swap(a, s, i);
        }
        System.out.println(Arrays.toString(a));
    }
}
```

* 优化点：为减少交换次数，每一轮可以先找最小的索引，在每轮最后再交换元素

**与冒泡排序比较**

1. 二者平均时间复杂度都是 $O(n^2)$

2. 选择排序一般要快于冒泡，因为其交换次数少

3. 但如果集合有序度高，冒泡优于选择

4. 冒泡属于稳定排序算法，而选择属于不稳定排序
   * 稳定排序指，按对象中不同字段进行多次排序，不会打乱同值元素的顺序
   * 不稳定排序则反之

**稳定排序与不稳定排序**

```java
System.out.println("=================不稳定================");
Card[] cards = getStaticCards();
System.out.println(Arrays.toString(cards));
selection(cards, Comparator.comparingInt((Card a) -> a.sharpOrder).reversed());
System.out.println(Arrays.toString(cards));
selection(cards, Comparator.comparingInt((Card a) -> a.numberOrder).reversed());
System.out.println(Arrays.toString(cards));

System.out.println("=================稳定=================");
cards = getStaticCards();
System.out.println(Arrays.toString(cards));
bubble(cards, Comparator.comparingInt((Card a) -> a.sharpOrder).reversed());
System.out.println(Arrays.toString(cards));
bubble(cards, Comparator.comparingInt((Card a) -> a.numberOrder).reversed());
System.out.println(Arrays.toString(cards));
```

都是先按照花色排序（♠♥♣♦），再按照数字排序（AKQJ...）

* 不稳定排序算法按数字排序时，会打乱原本同值的花色顺序

  ```
  [[♠7], [♠2], [♠4], [♠5], [♥2], [♥5]]
  [[♠7], [♠5], [♥5], [♠4], [♥2], [♠2]]
  ```

  原来 ♠2 在前 ♥2 在后，按数字再排后，他俩的位置变了

* 稳定排序算法按数字排序时，会保留原本同值的花色顺序，如下所示 ♠2 与 ♥2 的相对位置不变

  ```
  [[♠7], [♠2], [♠4], [♠5], [♥2], [♥5]]
  [[♠7], [♠5], [♥5], [♠4], [♠2], [♥2]]
  ```




### 4. 插入排序

**要求**

* 能够用自己语言描述插入排序算法
* 能够比较插入排序与选择排序

**算法描述**

1. 将数组分为两个区域，排序区域和未排序区域，每一轮从未排序区域中取出第一个元素，插入到排序区域（需保证顺序）

2. 重复以上步骤，直到整个数组有序

> *更形象的描述请参考：insertion_sort.html*

**算法实现**

```java
// 修改了代码与希尔排序一致
public static void insert(int[] a) {
    // i 代表待插入元素的索引
    for (int i = 1; i < a.length; i++) {
        int t = a[i]; // 代表待插入的元素值
        int j = i;
        System.out.println(j);
        while (j >= 1) {
            if (t < a[j - 1]) { // j-1 是上一个元素索引，如果 > t，后移
                a[j] = a[j - 1];
                j--;
            } else { // 如果 j-1 已经 <= t, 则 j 就是插入位置
                break;
            }
        }
        a[j] = t;
        System.out.println(Arrays.toString(a) + " " + j);
    }
}
```

**与选择排序比较**

1. 二者平均时间复杂度都是 $O(n^2)$

2. 大部分情况下，插入都略优于选择

3. 有序集合插入的时间复杂度为 $O(n)$

4. 插入属于稳定排序算法，而选择属于不稳定排序

**提示**

> *插入排序通常被同学们所轻视，其实它的地位非常重要。小数据量排序，都会优先选择插入排序*



### 5. 希尔排序

**要求**

* 能够用自己语言描述希尔排序算法

**算法描述**

1. 首先选取一个间隙序列，如 (n/2，n/4 … 1)，n 为数组长度

2. 每一轮将间隙相等的元素视为一组，对组内元素进行插入排序，目的有二

   ① 少量元素插入排序速度很快

   ② 让组内值较大的元素更快地移动到后方

3. 当间隙逐渐减少，直至为 1 时，即可完成排序

> *更形象的描述请参考：shell_sort.html*

**算法实现**

```java
private static void shell(int[] a) {
    int n = a.length;
    for (int gap = n / 2; gap > 0; gap /= 2) {
        // i 代表待插入元素的索引
        for (int i = gap; i < n; i++) {
            int t = a[i]; // 代表待插入的元素值
            int j = i;
            while (j >= gap) {
                // 每次与上一个间隙为 gap 的元素进行插入排序
                if (t < a[j - gap]) { // j-gap 是上一个元素索引，如果 > t，后移
                    a[j] = a[j - gap];
                    j -= gap;
                } else { // 如果 j-1 已经 <= t, 则 j 就是插入位置
                    break;
                }
            }
            a[j] = t;
            System.out.println(Arrays.toString(a) + " gap:" + gap);
        }
    }
}
```

**参考资料**

* https://en.wikipedia.org/wiki/Shellsort



### 6. 快速排序

**要求**

* 能够用自己语言描述快速排序算法
* 掌握手写单边循环、双边循环代码之一
* 能够说明快排特点
* 了解洛穆托与霍尔两种分区方案的性能比较

**算法描述**

1. 每一轮排序选择一个基准点（pivot）进行分区
   1. 让小于基准点的元素的进入一个分区，大于基准点的元素的进入另一个分区
   2. 当分区完成时，基准点元素的位置就是其最终位置
2. 在子分区内重复以上过程，直至子分区元素个数少于等于 1，这体现的是分而治之的思想（[divide-and-conquer](https://en.wikipedia.org/wiki/Divide-and-conquer_algorithm)）
3. 从以上描述可以看出，一个关键在于分区算法，常见的有洛穆托分区方案、双边循环分区方案、霍尔分区方案

> *更形象的描述请参考：quick_sort.html*



**单边循环快排（lomuto 洛穆托分区方案）**

1. 选择最右元素作为基准点元素

2. j 指针负责找到比基准点小的元素，一旦找到则与 i 进行交换

3. i 指针维护小于基准点元素的边界，也是每次交换的目标索引

4. 最后基准点与 i 交换，i 即为分区位置

```java
public static void quick(int[] a, int l, int h) {
    if (l >= h) {
        return;
    }
    int p = partition(a, l, h); // p 索引值
    quick(a, l, p - 1); // 左边分区的范围确定
    quick(a, p + 1, h); // 左边分区的范围确定
}

private static int partition(int[] a, int l, int h) {
    int pv = a[h]; // 基准点元素
    int i = l;
    for (int j = l; j < h; j++) {
        if (a[j] < pv) {
            if (i != j) {
                swap(a, i, j);
            }
            i++;
        }
    }
    if (i != h) {
        swap(a, h, i);
    }
    System.out.println(Arrays.toString(a) + " i=" + i);
    // 返回值代表了基准点元素所在的正确索引，用它确定下一轮分区的边界
    return i;
}
```



**双边循环快排（不完全等价于 hoare 霍尔分区方案）**

1. 选择最左元素作为基准点元素
2. j 指针负责从右向左找比基准点小的元素，i 指针负责从左向右找比基准点大的元素，一旦找到二者交换，直至 i，j 相交
3. 最后基准点与 i（此时 i 与 j 相等）交换，i 即为分区位置

要点

1. 基准点在左边，并且要先 j 后 i

2. while( **i** **< j** && a[j] > pv ) j-- 
3. while ( **i** **< j** && a[i] **<=** pv ) i++

```java
private static void quick(int[] a, int l, int h) {
    if (l >= h) {
        return;
    }
    int p = partition(a, l, h);
    quick(a, l, p - 1);
    quick(a, p + 1, h);
}

private static int partition(int[] a, int l, int h) {
    int pv = a[l];
    int i = l;
    int j = h;
    while (i < j) {
        // j 从右找小的
        while (i < j && a[j] > pv) {
            j--;
        }
        // i 从左找大的
        while (i < j && a[i] <= pv) {
            i++;
        }
        swap(a, i, j);
    }
    swap(a, l, j);
    System.out.println(Arrays.toString(a) + " j=" + j);
    return j;
}
```



**快排特点**

1. 平均时间复杂度是 $O(nlog_2⁡n )$，最坏时间复杂度 $O(n^2)$

2. 数据量较大时，优势非常明显

3. 属于不稳定排序

**洛穆托分区方案 vs 霍尔分区方案**

* 霍尔的移动次数平均来讲比洛穆托少3倍
* https://qastack.cn/cs/11458/quicksort-partitioning-hoare-vs-lomuto

> ***补充代码说明***
>
> * day01.sort.QuickSort3 演示了空穴法改进的双边快排，比较次数更少
> * day01.sort.QuickSortHoare 演示了霍尔分区的实现
> * day01.sort.LomutoVsHoare 对四种分区实现的移动次数比较



### 7. ArrayList

**要求**

* 掌握 ArrayList 扩容规则

**扩容规则**

1. ArrayList() 会使用长度为零的数组

2. ArrayList(int initialCapacity) 会使用指定容量的数组

3. public ArrayList(Collection<? extends E> c) 会使用 c 的大小作为数组容量

4. add(Object o) 首次扩容为 10，再次扩容为上次容量的 1.5 倍

5. addAll(Collection c)没有元素时，扩容为 Math.max(10, 实际元素个数)，有元素时为 Math.max(原容量 1.5 倍, 实际元素个数)

其中第 4 点必须知道，其它几点视个人情况而定

**提示**

* 测试代码见 `day01.list.TestArrayList` ，这里不再列出
* 要**注意**的是，示例中用反射方式来更直观地反映 ArrayList 的扩容特征，但从 JDK 9 由于模块化的影响，对反射做了较多限制，需要在运行测试代码时添加 VM 参数 `--add-opens java.base/java.util=ALL-UNNAMED` 方能运行通过，后面的例子都有相同问题

> ***代码说明***
>
> * day01.list.TestArrayList#arrayListGrowRule 演示了 add(Object) 方法的扩容规则，输入参数 n 代表打印多少次扩容后的数组长度



### 8. Iterator

**要求**

* 掌握什么是 Fail-Fast、什么是 Fail-Safe

Fail-Fast 与 Fail-Safe

* ArrayList 是 fail-fast 的典型代表，遍历的同时不能修改，尽快失败

* CopyOnWriteArrayList 是 fail-safe 的典型代表，遍历的同时可以修改，原理是读写分离

**提示**

* 测试代码见 `day01.list.FailFastVsFailSafe`，这里不再列出



### 9. LinkedList

**要求**

* 能够说清楚 LinkedList 对比 ArrayList 的区别，并重视纠正部分错误的认知

**LinkedList**

1. 基于双向链表，无需连续内存
2. 随机访问慢（要沿着链表遍历）
3. 头尾插入删除性能高
4. 占用内存多

**ArrayList**

1. 基于数组，需要连续内存
2. 随机访问快（指根据下标访问）
3. 尾部插入、删除性能可以，其它部分插入、删除都会移动数据，因此性能会低
4. 可以利用 cpu 缓存，局部性原理

> ***代码说明***
>
> * day01.list.ArrayListVsLinkedList#randomAccess 对比随机访问性能
> * day01.list.ArrayListVsLinkedList#addMiddle 对比向中间插入性能
> * day01.list.ArrayListVsLinkedList#addFirst 对比头部插入性能
> * day01.list.ArrayListVsLinkedList#addLast 对比尾部插入性能
> * day01.list.ArrayListVsLinkedList#linkedListSize 打印一个 LinkedList 占用内存
> * day01.list.ArrayListVsLinkedList#arrayListSize 打印一个 ArrayList 占用内存



### 10. HashMap

**要求**

* 掌握 HashMap 的基本数据结构
* 掌握树化
* 理解索引计算方法、二次 hash 的意义、容量对索引计算的影响
* 掌握 put 流程、扩容、扩容因子
* 理解并发使用 HashMap 可能导致的问题
* 理解 key 的设计



#### 1）基本数据结构

* 1.7 数组 + 链表
* 1.8 数组 +（链表 | 红黑树）

> 更形象的演示，见资料中的 hash-demo.jar，运行需要 jdk14 以上环境，进入 jar 包目录，执行下面命令
>
> ```
> java -jar --add-exports java.base/jdk.internal.misc=ALL-UNNAMED hash-demo.jar
> ```



#### 2）树化与退化

**树化意义**

* 红黑树用来避免 DoS 攻击，防止链表超长时性能下降，树化应当是偶然情况，是保底策略
  1. hash 表的查找，更新的时间复杂度是 $O(1)$，而红黑树的查找，更新的时间复杂度是 $O(log_2⁡n )$，TreeNode 占用空间也比普通 Node 的大，如非必要，尽量还是使用链表
  2. hash 值如果足够随机，则在 hash 表内按泊松分布，在负载因子 0.75 的情况下，长度超过 8 的链表出现概率是 0.00000006，树化阈值选择 8 就是为了让树化几率足够小


**树化规则**

* 当链表长度超过树化阈值 8 时，先尝试扩容来减少链表长度，如果数组容量已经 >=64，才会进行树化

**退化规则**

* 情况1：在扩容时如果拆分树时，树元素个数 <= 6 则会退化链表
* 情况2：remove 树节点时，若 root、root.left、root.right、root.left.left 有一个为 null ，也会退化为链表



#### 3）索引计算

**索引计算方法**

* 首先，计算对象的 hashCode()
* 再进行调用 HashMap 的 hash() 方法进行二次哈希
  * 二次 hash() 是为了综合高位数据，让哈希分布更为均匀
* 最后 & (capacity – 1) 得到索引

**数组容量为何是 2 的 n 次幂**

1. 计算索引时效率更高：如果是 2 的 n 次幂可以使用位与运算代替取模
2. 扩容时重新计算索引效率更高：hash & oldCap == 0 的元素留在原来位置，否则新位置 = 旧位置 + oldCap

**注意**

* 二次 hash 是为了配合 **容量是 2 的 n 次幂** 这一设计前提，如果 hash 表的容量不是 2 的 n 次幂，则不必二次 hash
* **容量是 2 的 n 次幂** 这一设计计算索引效率更好，但 hash 的分散性就不好，需要二次 hash 来作为补偿，没有采用这一设计的典型例子是 Hashtable

 

#### 4）put 与扩容

**put 流程**

1. HashMap 是懒惰创建数组的，首次使用才创建数组
2. 计算索引（桶下标）
3. 如果桶下标还没人占用，创建 Node 占位返回
4. 如果桶下标已经有人占用
   1. 已经是 TreeNode 走红黑树的添加或更新逻辑
   2. 是普通 Node，走链表的添加或更新逻辑，如果链表长度超过树化阈值，走树化逻辑
5. 返回前检查容量是否超过阈值，一旦超过进行扩容



**1.7 与 1.8 的区别**

1. 链表插入节点时，1.7 是头插法，1.8 是尾插法

2. 1.7 是大于等于阈值且没有空位时才扩容，而 1.8 是大于阈值就扩容

3. 1.8 在扩容计算 Node 索引时，会优化

 

**扩容（加载）因子为何默认是 0.75f**

1. 在空间占用与查询时间之间取得较好的权衡
2. 大于这个值，空间节省了，但链表就会比较长影响性能
3. 小于这个值，冲突减少了，但扩容就会更频繁，空间占用也更多



#### 5）并发问题

**扩容死链（1.7 会存在）**

1.7 源码如下：

```java
void transfer(Entry[] newTable, boolean rehash) {
    int newCapacity = newTable.length;
    for (Entry<K,V> e : table) {
        while(null != e) {
            Entry<K,V> next = e.next;
            if (rehash) {
                e.hash = null == e.key ? 0 : hash(e.key);
            }
            int i = indexFor(e.hash, newCapacity);
            e.next = newTable[i];  
            newTable[i] = e;
            e = next;
        }
    }
}
```

* e 和 next 都是局部变量，用来指向当前节点和下一个节点
* 线程1（绿色）的临时变量 e 和 next 刚引用了这俩节点，还未来得及移动节点，发生了线程切换，由线程2（蓝色）完成扩容和迁移

![image-20210831084325075](./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210831084325075.png)

* 线程2 扩容完成，由于头插法，链表顺序颠倒。但线程1 的临时变量 e 和 next 还引用了这俩节点，还要再来一遍迁移

![image-20210831084723383](./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210831084723383.png)

* 第一次循环
  * 循环接着线程切换前运行，注意此时 e 指向的是节点 a，next 指向的是节点 b
  * e 头插 a 节点，注意图中画了两份 a 节点，但事实上只有一个（为了不让箭头特别乱画了两份）
  * 当循环结束是 e 会指向 next 也就是 b 节点

![image-20210831084855348](./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210831084855348.png)

* 第二次循环
  * next 指向了节点 a
  * e 头插节点 b
  * 当循环结束时，e 指向 next 也就是节点 a

![image-20210831085329449](./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210831085329449.png)

* 第三次循环
  * next 指向了 null
  * e 头插节点 a，**a 的 next 指向了 b**（之前 a.next 一直是 null），b 的 next 指向 a，死链已成
  * 当循环结束时，e 指向 next 也就是 null，因此第四次循环时会正常退出

![image-20210831085543224](./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210831085543224.png)

**数据错乱（1.7，1.8 都会存在）**

* 代码参考 `day01.map.HashMapMissData`，具体调试步骤参考视频

> ***补充代码说明***
>
> * day01.map.HashMapDistribution 演示 map 中链表长度符合泊松分布
> * day01.map.DistributionAffectedByCapacity 演示容量及 hashCode 取值对分布的影响
>   * day01.map.DistributionAffectedByCapacity#hashtableGrowRule 演示了 Hashtable 的扩容规律
>   * day01.sort.Utils#randomArray 如果 hashCode 足够随机，容量是否是 2 的 n 次幂影响不大
>   * day01.sort.Utils#lowSameArray 如果 hashCode 低位一样的多，容量是 2 的 n 次幂会导致分布不均匀
>   * day01.sort.Utils#evenArray 如果 hashCode 偶数的多，容量是 2 的 n 次幂会导致分布不均匀
>   * 由此得出对于容量是 2 的 n 次幂的设计来讲，二次 hash 非常重要
> * day01.map.HashMapVsHashtable 演示了对于同样数量的单词字符串放入 HashMap 和 Hashtable 分布上的区别



#### 6）key 的设计

**key 的设计要求**

1. HashMap 的 key 可以为 null，但 Map 的其他实现则不然
2. 作为 key 的对象，必须实现 hashCode 和 equals，并且 key 的内容不能修改（不可变）
3. key 的 hashCode 应该有良好的散列性

如果 key 可变，例如修改了 age 会导致再次查询时查询不到

```java
public class HashMapMutableKey {
    public static void main(String[] args) {
        HashMap<Student, Object> map = new HashMap<>();
        Student stu = new Student("张三", 18);
        map.put(stu, new Object());

        System.out.println(map.get(stu)); // 找到

        stu.age = 19;
        System.out.println(map.get(stu));  // 找不到了
    }

    static class Student {
        String name;
        int age;

        public Student(String name, int age) {
            this.name = name;
            this.age = age;
        }

        public String getName() {
            return name;
        }

        public void setName(String name) {
            this.name = name;
        }

        public int getAge() {
            return age;
        }

        public void setAge(int age) {
            this.age = age;
        }

        @Override
        public boolean equals(Object o) {
            if (this == o) return true;
            if (o == null || getClass() != o.getClass()) return false;
            Student student = (Student) o;
            return age == student.age && Objects.equals(name, student.name);
        }

        @Override
        public int hashCode() {
            return Objects.hash(name, age);
        }
    }
}
```



**String 对象的 hashCode() 设计**

* 目标是达到较为均匀的散列效果，每个字符串的 hashCode 足够独特
* 字符串中的每个字符都可以表现为一个数字，称为 $S_i$，其中 i 的范围是 0 ~ n-1
* 散列公式为： $S_0∗31^{(n-1)}+ S_1∗31^{(n-2)}+ … S_i ∗ 31^{(n-1-i)}+ …S_{(n-1)}∗31^0$
* 31 代入公式有较好的散列特性，并且 31 * h 可以被优化为
  * 即 $32 ∗h -h $
  * 即 $2^5  ∗h -h$
  * 即 $h≪5  -h$



### 11. 单例模式

**要求**

* 掌握五种单例模式的实现方式
* 理解为何 DCL 实现时要使用 volatile 修饰静态变量
* 了解 jdk 中用到单例的场景

**饿汉式**

```java
public class Singleton1 implements Serializable {
    private Singleton1() {
        if (INSTANCE != null) {
            throw new RuntimeException("单例对象不能重复创建");
        }
        System.out.println("private Singleton1()");
    }

    private static final Singleton1 INSTANCE = new Singleton1();

    public static Singleton1 getInstance() {
        return INSTANCE;
    }

    public static void otherMethod() {
        System.out.println("otherMethod()");
    }

    public Object readResolve() {
        return INSTANCE;
    }
}
```

* 构造方法抛出异常是防止反射破坏单例
* `readResolve()`是防止反序列化破坏单例

**枚举饿汉式**

```java
public enum Singleton2 {
    INSTANCE;

    private Singleton2() {
        System.out.println("private Singleton2()");
    }

    @Override
    public String toString() {
        return getClass().getName() + "@" + Integer.toHexString(hashCode());
    }

    public static Singleton2 getInstance() {
        return INSTANCE;
    }

    public static void otherMethod() {
        System.out.println("otherMethod()");
    }
}
```

* 枚举饿汉式能天然防止反射、反序列化破坏单例

**懒汉式**

```java
public class Singleton3 implements Serializable {
    private Singleton3() {
        System.out.println("private Singleton3()");
    }

    private static Singleton3 INSTANCE = null;

    // Singleton3.class
    public static synchronized Singleton3 getInstance() {
        if (INSTANCE == null) {
            INSTANCE = new Singleton3();
        }
        return INSTANCE;
    }

    public static void otherMethod() {
        System.out.println("otherMethod()");
    }
}
```

* 其实只有首次创建单例对象时才需要同步，但该代码实际上每次调用都会同步
* 因此有了下面的双检锁改进

**双检锁懒汉式**

```java
public class Singleton4 implements Serializable {
    private Singleton4() {
        System.out.println("private Singleton4()");
    }

    private static volatile Singleton4 INSTANCE = null; // volatile解决的是可见性，有序性问题

    public static Singleton4 getInstance() {
        if (INSTANCE == null) {
            synchronized (Singleton4.class) {
                if (INSTANCE == null) {
                    INSTANCE = new Singleton4();
                }
            }
        }
        return INSTANCE;
    }

    public static void otherMethod() {
        System.out.println("otherMethod()");
    }
}
```

为何必须加 volatile：

* `INSTANCE = new Singleton4()`不是原子的，分成 3 步：创建对象、调用构造、给静态变量赋值，其中后两步可能被指令重排序优化，变成先赋值、再调用构造
* 如果线程1 先执行了赋值，线程2 执行到第一个 `INSTANCE == null` 时发现 INSTANCE 已经不为 null，此时就会返回一个未完全构造的对象

**内部类懒汉式**

```java
public class Singleton5 implements Serializable {
    private Singleton5() {
        System.out.println("private Singleton5()");
    }

    private static class Holder {
        static Singleton5 INSTANCE = new Singleton5();
    }

    public static Singleton5 getInstance() {
        return Holder.INSTANCE;
    }

    public static void otherMethod() {
        System.out.println("otherMethod()");
    }
}
```

* 避免了双检锁的缺点

**JDK 中单例的体现**

* Runtime 体现了饿汉式单例
* System类的 Console 体现了双检锁懒汉式单例
* Collections 中的 EmptyNavigableSet 内部类懒汉式单例
* ReverseComparator.REVERSE_ORDER 内部类懒汉式单例
* Comparators.NaturalOrderComparator.INSTANCE 枚举饿汉式单例

+++

## 并发篇

### 1. 线程状态

**要求**

* 掌握 Java 线程六种状态
* 掌握 Java 线程状态转换
* 能理解五种状态与六种状态两种说法的区别

**六种状态及转换**

![image-20210831090722658](./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210831090722658.png)

分别是

* 新建
  * 当一个线程对象被创建，但还未调用 start 方法时处于**新建**状态
  * 此时未与操作系统底层线程关联
* 可运行
  * 调用了 start 方法，就会由**新建**进入**可运行**
  * 此时与底层线程关联，由操作系统调度执行
* 终结
  * 线程内代码已经执行完毕，由**可运行**进入**终结**
  * 此时会取消与底层线程关联
* 阻塞
  * 当获取锁失败后，由**可运行**进入 Monitor 的阻塞队列**阻塞**，此时不占用 cpu 时间
  * 当持锁线程释放锁时，会按照一定规则唤醒阻塞队列中的**阻塞**线程，唤醒后的线程进入**可运行**状态
* 等待
  * 当获取锁成功后，但由于条件不满足，调用了 wait() 方法，此时从**可运行**状态释放锁进入 Monitor 等待集合**等待**，同样不占用 cpu 时间
  * 当其它持锁线程调用 notify() 或 notifyAll() 方法，会按照一定规则唤醒等待集合中的**等待**线程，恢复为**可运行**状态
* 有时限等待
  * 当获取锁成功后，但由于条件不满足，调用了 wait(long) 方法，此时从**可运行**状态释放锁进入 Monitor 等待集合进行**有时限等待**，同样不占用 cpu 时间
  * 当其它持锁线程调用 notify() 或 notifyAll() 方法，会按照一定规则唤醒等待集合中的**有时限等待**线程，恢复为**可运行**状态，并重新去竞争锁
  * 如果等待超时，也会从**有时限等待**状态恢复为**可运行**状态，并重新去竞争锁
  * 还有一种情况是调用 sleep(long) 方法也会从**可运行**状态进入**有时限等待**状态，但与 Monitor 无关，不需要主动唤醒，超时时间到自然恢复为**可运行**状态

> ***其它情况（只需了解）***
>
> * 可以用 interrupt() 方法打断**等待**、**有时限等待**的线程，让它们恢复为**可运行**状态
> * park，unpark 等方法也可以让线程等待和唤醒

**五种状态**

五种状态的说法来自于操作系统层面的划分

![image-20210831092652602](./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210831092652602.png)

* 运行态：分到 cpu 时间，能真正执行线程内代码的
* 就绪态：有资格分到 cpu 时间，但还未轮到它的
* 阻塞态：没资格分到 cpu 时间的
  * 涵盖了 java 状态中提到的**阻塞**、**等待**、**有时限等待**
  * 多出了阻塞 I/O，指线程在调用阻塞 I/O 时，实际活由 I/O 设备完成，此时线程无事可做，只能干等
* 新建与终结态：与 java 中同名状态类似，不再啰嗦



### 2. 线程池

**要求**

* 掌握线程池的 7 大核心参数

**七大参数**

1. corePoolSize 核心线程数目 - 池中会保留的最多线程数
2. maximumPoolSize 最大线程数目 - 核心线程+救急线程的最大数目
3. keepAliveTime 生存时间 - 救急线程的生存时间，生存时间内没有新任务，此线程资源会释放
4. unit 时间单位 - 救急线程的生存时间单位，如秒、毫秒等
5. workQueue - 当没有空闲核心线程时，新来任务会加入到此队列排队，队列满会创建救急线程执行任务
6. threadFactory 线程工厂 - 可以定制线程对象的创建，例如设置线程名字、是否是守护线程等
7. handler 拒绝策略 - 当所有线程都在繁忙，workQueue 也放满时，会触发拒绝策略
   1. 抛异常 java.util.concurrent.ThreadPoolExecutor.AbortPolicy
   2. 由调用者执行任务 java.util.concurrent.ThreadPoolExecutor.CallerRunsPolicy
   3. 丢弃任务 java.util.concurrent.ThreadPoolExecutor.DiscardPolicy
   4. 丢弃最早排队任务 java.util.concurrent.ThreadPoolExecutor.DiscardOldestPolicy

![image-20210831093204388](./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210831093204388.png)

> ***代码说明***
>
> day02.TestThreadPoolExecutor 以较为形象的方式演示了线程池的核心组成



### 3. wait vs sleep

**要求**

* 能够说出二者区别

**一个共同点，三个不同点**

共同点

* wait() ，wait(long) 和 sleep(long) 的效果都是让当前线程暂时放弃 CPU 的使用权，进入阻塞状态

不同点

* 方法归属不同
  * sleep(long) 是 Thread 的静态方法
  * 而 wait()，wait(long) 都是 Object 的成员方法，每个对象都有

* 醒来时机不同
  * 执行 sleep(long) 和 wait(long) 的线程都会在等待相应毫秒后醒来
  * wait(long) 和 wait() 还可以被 notify 唤醒，wait() 如果不唤醒就一直等下去
  * 它们都可以被打断唤醒

* 锁特性不同（重点）
  * wait 方法的调用必须先获取 wait 对象的锁，而 sleep 则无此限制
  * wait 方法执行后会释放对象锁，允许其它线程获得该对象锁（我放弃 cpu，但你们还可以用）
  * 而 sleep 如果在 synchronized 代码块中执行，并不会释放对象锁（我放弃 cpu，你们也用不了）



### 4. lock vs synchronized

**要求**

* 掌握 lock 与 synchronized 的区别
* 理解 ReentrantLock 的公平、非公平锁
* 理解 ReentrantLock 中的条件变量

**三个层面**

不同点

* 语法层面
  * synchronized 是关键字，源码在 jvm 中，用 c++ 语言实现
  * Lock 是接口，源码由 jdk 提供，用 java 语言实现
  * 使用 synchronized 时，退出同步代码块锁会自动释放，而使用 Lock 时，需要手动调用 unlock 方法释放锁
* 功能层面
  * 二者均属于悲观锁、都具备基本的互斥、同步、锁重入功能
  * Lock 提供了许多 synchronized 不具备的功能，例如获取等待状态、公平锁、可打断、可超时、多条件变量
  * Lock 有适合不同场景的实现，如 ReentrantLock， ReentrantReadWriteLock
* 性能层面
  * 在没有竞争时，synchronized 做了很多优化，如偏向锁、轻量级锁，性能不赖
  * 在竞争激烈时，Lock 的实现通常会提供更好的性能

**公平锁**

* 公平锁的公平体现
  * **已经处在阻塞队列**中的线程（不考虑超时）始终都是公平的，先进先出
  * 公平锁是指**未处于阻塞队列**中的线程来争抢锁，如果队列不为空，则老实到队尾等待
  * 非公平锁是指**未处于阻塞队列**中的线程来争抢锁，与队列头唤醒的线程去竞争，谁抢到算谁的
* 公平锁会降低吞吐量，一般不用

**条件变量**

* ReentrantLock 中的条件变量功能类似于普通 synchronized 的 wait，notify，用在当线程获得锁后，发现条件不满足时，临时等待的链表结构
* 与 synchronized 的等待集合不同之处在于，ReentrantLock 中的条件变量可以有多个，可以实现更精细的等待、唤醒控制

> ***代码说明***
>
> * day02.TestReentrantLock 用较为形象的方式演示 ReentrantLock 的内部结构



### 5. volatile

**要求**

* 掌握线程安全要考虑的三个问题
* 掌握 volatile 能解决哪些问题

**原子性**

* 起因：多线程下，不同线程的**指令发生了交错**导致的共享变量的读写混乱
* 解决：用悲观锁或乐观锁解决，volatile 并不能解决原子性

**可见性**

* 起因：由于**编译器优化、或缓存优化、或 CPU 指令重排序优化**导致的对共享变量所做的修改另外的线程看不到
* 解决：用 volatile 修饰共享变量，能够防止编译器等优化发生，让一个线程对共享变量的修改对另一个线程可见

**有序性**

* 起因：由于**编译器优化、或缓存优化、或 CPU 指令重排序优化**导致指令的实际执行顺序与编写顺序不一致
* 解决：用 volatile 修饰共享变量会在读、写共享变量时加入不同的屏障，阻止其他读写操作越过屏障，从而达到阻止重排序的效果
* 注意：
  * **volatile 变量写**加的屏障是阻止上方其它写操作越过屏障排到 **volatile 变量写**之下
  * **volatile 变量读**加的屏障是阻止下方其它读操作越过屏障排到 **volatile 变量读**之上
  * volatile 读写加入的屏障只能防止同一线程内的指令重排

> ***代码说明***
>
> * day02.threadsafe.AddAndSubtract 演示原子性
> * day02.threadsafe.ForeverLoop 演示可见性
>   * 注意：本例经实践检验是编译器优化导致的可见性问题
> * day02.threadsafe.Reordering 演示有序性
>   * 需要打成 jar 包后测试
> * 请同时参考视频讲解



### 6. 悲观锁 vs 乐观锁

**要求**

* 掌握悲观锁和乐观锁的区别

**对比悲观锁与乐观锁**

* 悲观锁的代表是 synchronized 和 Lock 锁
  * 其核心思想是【线程只有占有了锁，才能去操作共享变量，每次只有一个线程占锁成功，获取锁失败的线程，都得停下来等待】
  * 线程从运行到阻塞、再从阻塞到唤醒，涉及线程上下文切换，如果频繁发生，影响性能
  * 实际上，线程在获取 synchronized 和 Lock 锁时，如果锁已被占用，都会做几次重试操作，减少阻塞的机会

* 乐观锁的代表是 AtomicInteger，使用 cas 来保证原子性
  * 其核心思想是【无需加锁，每次只有一个线程能成功修改共享变量，其它失败的线程不需要停止，不断重试直至成功】
  * 由于线程一直运行，不需要阻塞，因此不涉及线程上下文切换
  * 它需要多核 cpu 支持，且线程数不应超过 cpu 核数

> ***代码说明***
>
> * day02.SyncVsCas 演示了分别使用乐观锁和悲观锁解决原子赋值
> * 请同时参考视频讲解



### 7. Hashtable vs ConcurrentHashMap

**要求**

* 掌握 Hashtable 与 ConcurrentHashMap 的区别
* 掌握 ConcurrentHashMap 在不同版本的实现区别

> 更形象的演示，见资料中的 hash-demo.jar，运行需要 jdk14 以上环境，进入 jar 包目录，执行下面命令
>
> ```
> java -jar --add-exports java.base/jdk.internal.misc=ALL-UNNAMED hash-demo.jar
> ```

**Hashtable 对比 ConcurrentHashMap**

* Hashtable 与 ConcurrentHashMap 都是线程安全的 Map 集合
* Hashtable 并发度低，整个 Hashtable 对应一把锁，同一时刻，只能有一个线程操作它
* ConcurrentHashMap 并发度高，整个 ConcurrentHashMap 对应多把锁，只要线程访问的是不同锁，那么不会冲突

**ConcurrentHashMap 1.7**

* 数据结构：`Segment(大数组) + HashEntry(小数组) + 链表`，每个 Segment 对应一把锁，如果多个线程访问不同的 Segment，则不会冲突
* 并发度：Segment 数组大小即并发度，决定了同一时刻最多能有多少个线程并发访问。Segment 数组不能扩容，意味着并发度在 ConcurrentHashMap 创建时就固定了
* 索引计算
  * 假设大数组长度是 $2^m$，key 在大数组内的索引是 key 的二次 hash 值的高 m 位
  * 假设小数组长度是 $2^n$，key 在小数组内的索引是 key 的二次 hash 值的低 n 位
* 扩容：每个小数组的扩容相对独立，小数组在超过扩容因子时会触发扩容，每次扩容翻倍
* Segment[0] 原型：首次创建其它小数组时，会以此原型为依据，数组长度，扩容因子都会以原型为准

**ConcurrentHashMap 1.8**

* 数据结构：`Node 数组 + 链表或红黑树`，数组的每个头节点作为锁，如果多个线程访问的头节点不同，则不会冲突。首次生成头节点时如果发生竞争，利用 cas 而非 syncronized，进一步提升性能
* 并发度：Node 数组有多大，并发度就有多大，与 1.7 不同，Node 数组可以扩容
* 扩容条件：Node 数组满 3/4 时就会扩容
* 扩容单位：以链表为单位从后向前迁移链表，迁移完成的将旧数组头节点替换为 ForwardingNode
* 扩容时并发 get
  * 根据是否为 ForwardingNode 来决定是在新数组查找还是在旧数组查找，不会阻塞
  * 如果链表长度超过 1，则需要对节点进行复制（创建新节点），怕的是节点迁移后 next 指针改变
  * 如果链表最后几个元素扩容后索引不变，则节点无需复制
* 扩容时并发 put
  * 如果 put 的线程与扩容线程操作的链表是同一个，put 线程会阻塞
  * 如果 put 的线程操作的链表还未迁移完成，即头节点不是 ForwardingNode，则可以并发执行
  * 如果 put 的线程操作的链表已经迁移完成，即头结点是 ForwardingNode，则可以协助扩容
* 与 1.7 相比是懒惰初始化
* capacity 代表预估的元素个数，capacity / factory 来计算出初始数组大小，需要贴近 $2^n$ 
* loadFactor 只在计算初始数组大小时被使用，之后扩容固定为 3/4
* 超过树化阈值时的扩容问题，如果容量已经是 64，直接树化，否则在原来容量基础上做 3 轮扩容



### 8. ThreadLocal

**要求**

* 掌握 ThreadLocal 的作用与原理
* 掌握 ThreadLocal 的内存释放时机

**作用**

* ThreadLocal 可以实现【资源对象】的线程隔离，让每个线程各用各的【资源对象】，避免争用引发的线程安全问题
* ThreadLocal 同时实现了线程内的资源共享

**原理**

每个线程内有一个 ThreadLocalMap 类型的成员变量，用来存储资源对象

* 调用 set 方法，就是以 ThreadLocal 自己作为 key，资源对象作为 value，放入当前线程的 ThreadLocalMap 集合中
* 调用 get 方法，就是以 ThreadLocal 自己作为 key，到当前线程中查找关联的资源值
* 调用 remove 方法，就是以 ThreadLocal 自己作为 key，移除当前线程关联的资源值

ThreadLocalMap 的一些特点

* key 的 hash 值统一分配
* 初始容量 16，扩容因子 2/3，扩容容量翻倍
* key 索引冲突后用开放寻址法解决冲突

**弱引用 key**

ThreadLocalMap 中的 key 被设计为弱引用，原因如下

* Thread 可能需要长时间运行（如线程池中的线程），如果 key 不再使用，需要在内存不足（GC）时释放其占用的内存

**内存释放时机**

* 被动 GC 释放 key
  * 仅是让 key 的内存释放，关联 value 的内存并不会释放
* 懒惰被动释放 value
  * get key 时，发现是 null key，则释放其 value 内存
  * set key 时，会使用启发式扫描，清除临近的 null key 的 value 内存，启发次数与元素个数，是否发现 null key 有关
* 主动 remove 释放 key，value
  * 会同时释放 key，value 的内存，也会清除临近的 null key 的 value 内存
  * 推荐使用它，因为一般使用 ThreadLocal 时都把它作为静态变量（即强引用），因此无法被动依靠 GC 回收

+++

## 虚拟机篇

### 1. JVM 内存结构

**要求**

* 掌握 JVM 内存结构划分
* 尤其要知道方法区、永久代、元空间的关系

**结合一段 java 代码的执行理解内存划分**

![image-20210831165728217](./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210831165728217.png)

* 执行 javac 命令编译源代码为字节码
* 执行 java 命令
  1. 创建 JVM，调用类加载子系统加载 class，将类的信息存入**方法区**
  2. 创建 main 线程，使用的内存区域是 **JVM 虚拟机栈**，开始执行 main 方法代码
  3. 如果遇到了未见过的类，会继续触发类加载过程，同样会存入**方法区**
  4. 需要创建对象，会使用**堆**内存来存储对象
  5. 不再使用的对象，会由**垃圾回收器**在内存不足时回收其内存
  6. 调用方法时，方法内的局部变量、方法参数所使用的是  **JVM 虚拟机栈**中的栈帧内存
  7. 调用方法时，先要到**方法区**获得到该方法的字节码指令，由**解释器**将字节码指令解释为机器码执行
  8. 调用方法时，会将要执行的指令行号读到**程序计数器**，这样当发生了线程切换，恢复时就可以从中断的位置继续
  9. 对于非 java 实现的方法调用，使用内存称为**本地方法栈**（见说明）
  10. 对于热点方法调用，或者频繁的循环代码，由 **JIT 即时编译器**将这些代码编译成机器码缓存，提高执行性能

说明

* 加粗字体代表了 JVM 虚拟机组件
* 对于 Oracle 的 Hotspot 虚拟机实现，不区分虚拟机栈和本地方法栈

**会发生内存溢出的区域**

* 不会出现内存溢出的区域 – 程序计数器
* 出现 OutOfMemoryError 的情况
  * 堆内存耗尽 – 对象越来越多，又一直在使用，不能被垃圾回收
  * 方法区内存耗尽 – 加载的类越来越多，很多框架都会在运行期间动态产生新的类
  * 虚拟机栈累积 – 每个线程最多会占用 1 M 内存，线程个数越来越多，而又长时间运行不销毁时
* 出现 StackOverflowError 的区域
  * JVM 虚拟机栈，原因有方法递归调用未正确结束、反序列化 json 时循环引用

**方法区、永久代、元空间**

* **方法区**是 JVM 规范中定义的一块内存区域，用来存储类元数据、方法字节码、即时编译器需要的信息等
* **永久代**是 Hotspot 虚拟机对 JVM 规范的实现（1.8 之前）
* **元空间**是 Hotspot 虚拟机对 JVM 规范的另一种实现（1.8 以后），使用本地内存作为这些信息的存储空间

![image-20210831170457337](./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210831170457337.png)

从这张图学到三点

* 当第一次用到某个类是，由类加载器将 class 文件的类元信息读入，并存储于元空间
* X，Y 的类元信息是存储于元空间中，无法直接访问
* 可以用 X.class，Y.class 间接访问类元信息，它们俩属于 java 对象，我们的代码中可以使用

![image-20210831170512418](./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210831170512418.png)

从这张图可以学到

* 堆内存中：当一个**类加载器对象**，这个类加载器对象加载的所有**类对象**，这些类对象对应的所有**实例对象**都没人引用时，GC 时就会对它们占用的对内存进行释放
* 元空间中：内存释放**以类加载器为单位**，当堆中类加载器内存释放时，对应的元空间中的类元信息也会释放



### 2. JVM 内存参数

**要求** 

* 熟悉常见的 JVM 参数，尤其和大小相关的

**堆内存，按大小设置**

![image-20210831173130717](./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210831173130717.png)

解释：

* -Xms 最小堆内存（包括新生代和老年代）
* -Xmx 最大对内存（包括新生代和老年代）
* 通常建议将 -Xms 与 -Xmx 设置为大小相等，即不需要保留内存，不需要从小到大增长，这样性能较好
* -XX:NewSize 与 -XX:MaxNewSize 设置新生代的最小与最大值，但一般不建议设置，由 JVM 自己控制
* -Xmn 设置新生代大小，相当于同时设置了 -XX:NewSize 与 -XX:MaxNewSize 并且取值相等
* 保留是指，一开始不会占用那么多内存，随着使用内存越来越多，会逐步使用这部分保留内存。下同



**堆内存，按比例设置**

![image-20210831173045700](./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210831173045700.png)

解释：

* -XX:NewRatio=2:1 表示老年代占两份，新生代占一份
* -XX:SurvivorRatio=4:1 表示新生代分成六份，伊甸园占四份，from 和 to 各占一份



**元空间内存设置**

![image-20210831173118634](./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210831173118634.png)

解释：

* class space 存储类的基本信息，最大值受 -XX:CompressedClassSpaceSize 控制
* non-class space 存储除类的基本信息以外的其它信息（如方法字节码、注解等）
* class space 和 non-class space 总大小受 -XX:MaxMetaspaceSize 控制

注意：

* 这里 -XX:CompressedClassSpaceSize 这段空间还与是否开启了指针压缩有关，这里暂不深入展开，可以简单认为指针压缩默认开启



**代码缓存内存设置**

![image-20210831173148816](./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210831173148816.png)

解释：

* 如果 -XX:ReservedCodeCacheSize < 240m，所有优化机器代码不加区分存在一起
* 否则，分成三个区域（图中笔误 mthod 拼写错误，少一个 e）
  * non-nmethods - JVM 自己用的代码
  * profiled nmethods - 部分优化的机器码
  * non-profiled nmethods - 完全优化的机器码



**线程内存设置**

![image-20210831173155481](./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210831173155481.png)

> ***官方参考文档***
>
> * https://docs.oracle.com/en/java/javase/11/tools/java.html#GUID-3B1CE181-CD30-4178-9602-230B800D4FAE



### 3. JVM 垃圾回收

**要求**

* 掌握垃圾回收算法
* 掌握分代回收思想
* 理解三色标记及漏标处理
* 了解常见垃圾回收器

**三种垃圾回收算法**

标记清除法

![image-20210831211008162](./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210831211008162.png)

解释：

1. 找到 GC Root 对象，即那些一定不会被回收的对象，如正执行方法内局部变量引用的对象、静态变量引用的对象
2. 标记阶段：沿着 GC Root 对象的引用链找，直接或间接引用到的对象加上标记
3. 清除阶段：释放未加标记的对象占用的内存

要点：

* 标记速度与存活对象线性关系
* 清除速度与内存大小线性关系
* 缺点是会产生内存碎片



标记整理法

![image-20210831211641241](./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210831211641241.png)

解释：

1. 前面的标记阶段、清理阶段与标记清除法类似
2. 多了一步整理的动作，将存活对象向一端移动，可以避免内存碎片产生

特点：

* 标记速度与存活对象线性关系

* 清除与整理速度与内存大小成线性关系
* 缺点是性能上较慢



标记复制法

![image-20210831212125813](./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210831212125813.png)

解释：

1. 将整个内存分成两个大小相等的区域，from 和 to，其中 to 总是处于空闲，from 存储新创建的对象
2. 标记阶段与前面的算法类似
3. 在找出存活对象后，会将它们从 from 复制到 to 区域，复制的过程中自然完成了碎片整理
4. 复制完成后，交换 from 和 to 的位置即可

特点：

* 标记与复制速度与存活对象成线性关系
* 缺点是会占用成倍的空间



**GC 与分代回收算法**

GC 的目的在于实现无用对象内存自动释放，减少内存碎片、加快分配速度

GC 要点：

* 回收区域是**堆内存**，不包括虚拟机栈
* 判断无用对象，使用**可达性分析算法**，**三色标记法**标记存活对象，回收未标记对象
* GC 具体的实现称为**垃圾回收器**
* GC 大都采用了**分代回收思想**
  * 理论依据是大部分对象朝生夕灭，用完立刻就可以回收，另有少部分对象会长时间存活，每次很难回收
  * 根据这两类对象的特性将回收区域分为**新生代**和**老年代**，新生代采用标记复制法、老年代一般采用标记整理法
* 根据 GC 的规模可以分成 **Minor GC**，**Mixed GC**，**Full GC**



**分代回收**

1. 伊甸园 eden，最初对象都分配到这里，与幸存区 survivor（分成 from 和 to）合称新生代，

![image-20210831213622704](./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210831213622704.png)

2. 当伊甸园内存不足，标记伊甸园与 from（现阶段没有）的存活对象

![image-20210831213640110](./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210831213640110.png)

3. 将存活对象采用复制算法复制到 to 中，复制完毕后，伊甸园和 from 内存都得到释放

![image-20210831213657861](./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210831213657861.png)

4. 将 from 和 to 交换位置

![image-20210831213708776](./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210831213708776.png)

5. 经过一段时间后伊甸园的内存又出现不足

![image-20210831213724858](./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210831213724858.png)

6. 标记伊甸园与 from（现阶段没有）的存活对象

![image-20210831213737669](./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210831213737669.png)

7. 将存活对象采用复制算法复制到 to 中

![image-20210831213804315](./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210831213804315.png)

8. 复制完毕后，伊甸园和 from 内存都得到释放

![image-20210831213815371](./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210831213815371.png)

9. 将 from 和 to 交换位置

![image-20210831213826017](./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210831213826017.png)

10. 老年代 old，当幸存区对象熬过几次回收（最多15次），晋升到老年代（幸存区内存不足或大对象会导致提前晋升）



**GC 规模**

* Minor GC 发生在新生代的垃圾回收，暂停时间短

* Mixed GC 新生代 + 老年代部分区域的垃圾回收，G1 收集器特有

* Full GC 新生代 + 老年代完整垃圾回收，暂停时间长，**应尽力避免**



**三色标记**

即用三种颜色记录对象的标记状态

* 黑色 – 已标记
* 灰色 – 标记中
* 白色 – 还未标记



1. 起始的三个对象还未处理完成，用灰色表示

<img src="./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210831215016566.png" alt="image-20210831215016566" style="zoom:50%;" />

2. 该对象的引用已经处理完成，用黑色表示，黑色引用的对象变为灰色

<img src="./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210831215033510.png" alt="image-20210831215033510" style="zoom:50%;" />

3. 依次类推

<img src="./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210831215105280.png" alt="image-20210831215105280" style="zoom:50%;" />

4. 沿着引用链都标记了一遍

<img src="./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210831215146276.png" alt="image-20210831215146276" style="zoom:50%;" />

5. 最后为标记的白色对象，即为垃圾

<img src="./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210831215158311.png" alt="image-20210831215158311" style="zoom:50%;" />

**并发漏标问题**

比较先进的垃圾回收器都支持**并发标记**，即在标记过程中，用户线程仍然能工作。但这样带来一个新的问题，如果用户线程修改了对象引用，那么就存在漏标问题。例如：

1. 如图所示标记工作尚未完成

<img src="./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210831215846876.png" alt="image-20210831215846876" style="zoom:50%;" />

2. 用户线程同时在工作，断开了第一层 3、4 两个对象之间的引用，这时对于正在处理 3 号对象的垃圾回收线程来讲，它会将 4 号对象当做是白色垃圾

<img src="./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210831215904073.png" alt="image-20210831215904073" style="zoom:50%;" />

3. 但如果其他用户线程又建立了 2、4 两个对象的引用，这时因为 2 号对象是黑色已处理对象了，因此垃圾回收线程不会察觉到这个引用关系的变化，从而产生了漏标

<img src="./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210831215919493.png" alt="image-20210831215919493" style="zoom:50%;" />

4. 如果用户线程让黑色对象引用了一个新增对象，一样会存在漏标问题

<img src="./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210831220004062.png" alt="image-20210831220004062" style="zoom:50%;" />

因此对于**并发标记**而言，必须解决漏标问题，也就是要记录标记过程中的变化。有两种解决方法：

1. Incremental Update 增量更新法，CMS 垃圾回收器采用
   * 思路是拦截每次赋值动作，只要赋值发生，被赋值的对象就会被记录下来，在重新标记阶段再确认一遍
2. Snapshot At The Beginning，SATB 原始快照法，G1 垃圾回收器采用
   * 思路也是拦截每次赋值动作，不过记录的对象不同，也需要在重新标记阶段对这些对象二次处理
   * 新加对象会被记录
   * 被删除引用关系的对象也被记录



**垃圾回收器 - Parallel GC**

* eden 内存不足发生 Minor GC，采用标记复制算法，需要暂停用户线程
* old 内存不足发生 Full GC，采用标记整理算法，需要暂停用户线程

* **注重吞吐量**

**垃圾回收器 - ConcurrentMarkSweep GC**

* 它是工作在 old 老年代，支持**并发标记**的一款回收器，采用**并发清除**算法
  * 并发标记时不需暂停用户线程
  * 重新标记时仍需暂停用户线程

* 如果并发失败（即回收速度赶不上创建新对象速度），会触发 Full GC

* **注重响应时间**

**垃圾回收器 - G1 GC**

* **响应时间与吞吐量兼顾**
* 划分成多个区域，每个区域都可以充当 eden，survivor，old， humongous，其中 humongous 专为大对象准备
* 分成三个阶段：新生代回收、并发标记、混合收集
* 如果并发失败（即回收速度赶不上创建新对象速度），会触发 Full GC



**G1 回收阶段 - 新生代回收**

1. 初始时，所有区域都处于空闲状态

<img src="./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210831222639754.png" alt="image-20210831222639754" style="zoom:50%;" />

2. 创建了一些对象，挑出一些空闲区域作为伊甸园区存储这些对象

<img src="./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210831222653802.png" alt="image-20210831222653802" style="zoom:50%;" />

3. 当伊甸园需要垃圾回收时，挑出一个空闲区域作为幸存区，用复制算法复制存活对象，需要暂停用户线程

<img src="./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210831222705814.png" alt="image-20210831222705814" style="zoom:50%;" />

4. 复制完成，将之前的伊甸园内存释放

<img src="./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210831222724999.png" alt="image-20210831222724999" style="zoom:50%;" />

5. 随着时间流逝，伊甸园的内存又有不足

<img src="./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210831222737928.png" alt="image-20210831222737928" style="zoom:50%;" />

6. 将伊甸园以及之前幸存区中的存活对象，采用复制算法，复制到新的幸存区，其中较老对象晋升至老年代

<img src="./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210831222752787.png" alt="image-20210831222752787" style="zoom:50%;" />

7. 释放伊甸园以及之前幸存区的内存

<img src="./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210831222803281.png" alt="image-20210831222803281" style="zoom:50%;" />

**G1 回收阶段 - 并发标记与混合收集**

1. 当老年代占用内存超过阈值后，触发并发标记，这时无需暂停用户线程

<img src="./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210831222813959.png" alt="image-20210831222813959" style="zoom:50%;" />

2. 并发标记之后，会有重新标记阶段解决漏标问题，此时需要暂停用户线程。这些都完成后就知道了老年代有哪些存活对象，随后进入混合收集阶段。此时不会对所有老年代区域进行回收，而是根据**暂停时间目标**优先回收价值高（存活对象少）的区域（这也是 Gabage First 名称的由来）。

<img src="./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210831222828104.png" alt="image-20210831222828104" style="zoom:50%;" />

3. 混合收集阶段中，参与复制的有 eden、survivor、old，下图显示了伊甸园和幸存区的存活对象复制

<img src="./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210831222841096.png" alt="image-20210831222841096" style="zoom:50%;" />

4. 下图显示了老年代和幸存区晋升的存活对象的复制

<img src="./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210831222859760.png" alt="image-20210831222859760" style="zoom:50%;" />

5. 复制完成，内存得到释放。进入下一轮的新生代回收、并发标记、混合收集

<img src="./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210831222919182.png" alt="image-20210831222919182" style="zoom:50%;" />

### 4. 内存溢出

**要求**

* 能够说出几种典型的导致内存溢出的情况



**典型情况**

* 误用线程池导致的内存溢出
  * 参考 day03.TestOomThreadPool
* 查询数据量太大导致的内存溢出
  * 参考 day03.TestOomTooManyObject
* 动态生成类导致的内存溢出
  * 参考 day03.TestOomTooManyClass



### 5. 类加载

**要求**

* 掌握类加载阶段
* 掌握类加载器
* 理解双亲委派机制



**类加载过程的三个阶段**

1. 加载

   1. 将类的字节码载入方法区，并创建类.class 对象

   2. 如果此类的父类没有加载，先加载父类
   3. 加载是懒惰执行

2. 链接
   1. 验证 – 验证类是否符合 Class 规范，合法性、安全性检查
   2. 准备 – 为 static 变量分配空间，设置默认值
   3. 解析 – 将常量池的符号引用解析为直接引用

3. 初始化
   1. 静态代码块、static 修饰的变量赋值、static final 修饰的引用类型变量赋值，会被合并成一个 `<cinit>` 方法，在初始化时被调用
   2. static final 修饰的基本类型变量赋值，在链接阶段就已完成
   3. 初始化是懒惰执行

> ***验证手段***
>
> * 使用 jps 查看进程号
> * 使用 jhsdb 调试，执行命令 `jhsdb.exe hsdb` 打开它的图形界面
>   * Class Browser 可以查看当前 jvm 中加载了哪些类
>   * 控制台的 universe 命令查看堆内存范围
>   * 控制台的 g1regiondetails 命令查看 region 详情
>   * `scanoops 起始地址 结束地址 对象类型` 可以根据类型查找某个区间内的对象地址
>   * 控制台的 `inspect 地址` 指令能够查看这个地址对应的对象详情
> * 使用 javap 命令可以查看 class 字节码



>***代码说明***
>
>* day03.loader.TestLazy - 验证类的加载是懒惰的，用到时才触发类加载
>* day03.loader.TestFinal - 验证使用 final 修饰的变量不会触发类加载



**jdk 8 的类加载器**

| **名称**                | **加载哪的类**        | **说明**                       |
| ----------------------- | --------------------- | ------------------------------ |
| Bootstrap ClassLoader   | JAVA_HOME/jre/lib     | 无法直接访问                   |
| Extension ClassLoader   | JAVA_HOME/jre/lib/ext | 上级为 Bootstrap，显示为  null |
| Application ClassLoader | classpath             | 上级为 Extension               |
| 自定义类加载器          | 自定义                | 上级为 Application             |



**双亲委派机制**

所谓的双亲委派，就是指优先委派上级类加载器进行加载，如果上级类加载器

* 能找到这个类，由上级加载，加载后该类也对下级加载器可见
* 找不到这个类，则下级类加载器才有资格执行加载

双亲委派的目的有两点

1. 让上级类加载器中的类对下级共享（反之不行），即能让你的类能依赖到 jdk 提供的核心类

2. 让类的加载有优先次序，保证核心类优先加载



**对双亲委派的误解**

下面面试题的回答是错误的

![image-20210901110910016](./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210901110910016.png)

错在哪了？

* 自己编写类加载器就能加载一个假冒的 java.lang.System 吗? 答案是不行。

* 假设你自己的类加载器用双亲委派，那么优先由启动类加载器加载真正的 java.lang.System，自然不会加载假冒的

* 假设你自己的类加载器不用双亲委派，那么你的类加载器加载假冒的 java.lang.System 时，它需要先加载父类 java.lang.Object，而你没有用委派，找不到 java.lang.Object 所以加载会失败

* **以上也仅仅是假设**。事实上操作你就会发现，自定义类加载器加载以 java. 打头的类时，会抛安全异常，在 jdk9 以上版本这些特殊包名都与模块进行了绑定，更连编译都过不了



>***代码说明***
>
>* day03.loader.TestJdk9ClassLoader - 演示类加载器与模块的绑定关系



### 6. 四种引用

**要求**

* 掌握四种引用



**强引用**

1. 普通变量赋值即为强引用，如 A a = new A();

2. 通过 GC Root 的引用链，如果强引用不到该对象，该对象才能被回收

<img src="./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210901111903574.png" alt="image-20210901111903574" style="zoom:80%;" />

**软引用（SoftReference）**

1. 例如：SoftReference a = new SoftReference(new A());

2. 如果仅有软引用该对象时，首次垃圾回收不会回收该对象，如果内存仍不足，再次回收时才会释放对象

3. 软引用自身需要配合引用队列来释放

4. 典型例子是反射数据

<img src="./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210901111957328.png" alt="image-20210901111957328" style="zoom:80%;" />



**弱引用（WeakReference）**

1. 例如：WeakReference a = new WeakReference(new A());

2. 如果仅有弱引用引用该对象时，只要发生垃圾回收，就会释放该对象

3. 弱引用自身需要配合引用队列来释放

4. 典型例子是 ThreadLocalMap 中的 Entry 对象

<img src="./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210901112107707.png" alt="image-20210901112107707" style="zoom:80%;" />



**虚引用（PhantomReference）**

1. 例如： PhantomReference a = new PhantomReference(new A(), referenceQueue);

2. 必须配合引用队列一起使用，当虚引用所引用的对象被回收时，由 Reference Handler 线程将虚引用对象入队，这样就可以知道哪些对象被回收，从而对它们关联的资源做进一步处理

3. 典型例子是 Cleaner 释放 DirectByteBuffer 关联的直接内存

<img src="./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210901112157901.png" alt="image-20210901112157901" style="zoom:80%;" />





>***代码说明***
>
>* day03.reference.TestPhantomReference - 演示虚引用的基本用法
>* day03.reference.TestWeakReference - 模拟 ThreadLocalMap, 采用引用队列释放 entry 内存



### 7. finalize

**要求**

* 掌握 finalize 的工作原理与缺点



**finalize**

* 它是 Object 中的一个方法，如果子类重写它，垃圾回收时此方法会被调用，可以在其中进行资源释放和清理工作
* 将资源释放和清理放在 finalize 方法中非常不好，非常影响性能，严重时甚至会引起 OOM，从 Java9 开始就被标注为 @Deprecated，不建议被使用了



**finalize 原理**

1. 对 finalize 方法进行处理的核心逻辑位于 java.lang.ref.Finalizer 类中，它包含了名为 unfinalized 的静态变量（双向链表结构），Finalizer 也可被视为另一种引用对象（地位与软、弱、虚相当，只是不对外，无法直接使用）
2. 当重写了 finalize 方法的对象，在构造方法调用之时，JVM 都会将其包装成一个 Finalizer 对象，并加入 unfinalized 链表中

![image-20210901121032813](./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210901121032813.png)

3. Finalizer 类中还有另一个重要的静态变量，即 ReferenceQueue 引用队列，刚开始它是空的。当狗对象可以被当作垃圾回收时，就会把这些狗对象对应的 Finalizer 对象加入此引用队列
4. 但此时 Dog 对象还没法被立刻回收，因为 unfinalized -> Finalizer 这一引用链还在引用它嘛，为的是【先别着急回收啊，等我调完 finalize 方法，再回收】
5. FinalizerThread 线程会从 ReferenceQueue 中逐一取出每个 Finalizer 对象，把它们从链表断开并真正调用 finallize 方法

![image-20210901122228916](./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210901122228916.png)

6. 由于整个 Finalizer 对象已经从 unfinalized 链表中断开，这样没谁能引用到它和狗对象，所以下次 gc 时就被回收了



**finalize 缺点**

* 无法保证资源释放：FinalizerThread 是守护线程，代码很有可能没来得及执行完，线程就结束了
* 无法判断是否发生错误：执行 finalize 方法时，会吞掉任意异常（Throwable）
* 内存释放不及时：重写了 finalize 方法的对象在第一次被 gc 时，并不能及时释放它占用的内存，因为要等着 FinalizerThread 调用完 finalize，把它从 unfinalized 队列移除后，第二次 gc 时才能真正释放内存
* 有的文章提到【Finalizer 线程会和我们的主线程进行竞争，不过由于它的优先级较低，获取到的CPU时间较少，因此它永远也赶不上主线程的步伐】这个显然是错误的，FinalizerThread 的优先级较普通线程更高，原因应该是 finalize 串行执行慢等原因综合导致

> ***代码说明***
>
> * day03.reference.TestFinalize - finalize 的测试代码

+++

## 框架篇

### 1. Spring refresh 流程

**要求**

* 掌握 refresh 的 12 个步骤

**Spring refresh 概述**

refresh 是 AbstractApplicationContext 中的一个方法，负责初始化 ApplicationContext 容器，容器必须调用 refresh 才能正常工作。它的内部主要会调用 12 个方法，我们把它们称为 refresh 的 12 个步骤：

1. prepareRefresh

2. obtainFreshBeanFactory

3. prepareBeanFactory

4. postProcessBeanFactory

5. invokeBeanFactoryPostProcessors

6. registerBeanPostProcessors

7. initMessageSource

8. initApplicationEventMulticaster

9. onRefresh

10. registerListeners

11. finishBeanFactoryInitialization

12. finishRefresh

> ***功能分类***
>
> * 1 为准备环境
>
> * 2 3 4 5 6 为准备 BeanFactory
>
> * 7 8 9 10 12 为准备 ApplicationContext
>
> * 11 为初始化 BeanFactory 中非延迟单例 bean



**1. prepareRefresh**

* 这一步创建和准备了 Environment 对象，它作为 ApplicationContext 的一个成员变量

* Environment 对象的作用之一是为后续 @Value，值注入时提供键值
* Environment 分成三个主要部分
  * systemProperties - 保存 java 环境键值
  * systemEnvironment - 保存系统环境键值
  * 自定义 PropertySource - 保存自定义键值，例如来自于 *.properties 文件的键值

![image-20210902181639048](./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210902181639048.png)

**2. obtainFreshBeanFactory**

* 这一步获取（或创建） BeanFactory，它也是作为 ApplicationContext 的一个成员变量
* BeanFactory 的作用是负责 bean 的创建、依赖注入和初始化，bean 的各项特征由 BeanDefinition 定义
  * BeanDefinition 作为 bean 的设计蓝图，规定了 bean 的特征，如单例多例、依赖关系、初始销毁方法等
  * BeanDefinition 的来源有多种多样，可以是通过 xml 获得、配置类获得、组件扫描获得，也可以是编程添加
* 所有的 BeanDefinition 会存入 BeanFactory 中的 beanDefinitionMap 集合

![image-20210902182004819](./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210902182004819.png)

**3. prepareBeanFactory**

* 这一步会进一步完善 BeanFactory，为它的各项成员变量赋值
* beanExpressionResolver 用来解析 SpEL，常见实现为 StandardBeanExpressionResolver
* propertyEditorRegistrars 会注册类型转换器
  * 它在这里使用了 ResourceEditorRegistrar 实现类
  * 并应用 ApplicationContext 提供的 Environment 完成 ${ } 解析
* registerResolvableDependency 来注册 beanFactory 以及 ApplicationContext，让它们也能用于依赖注入
* beanPostProcessors 是 bean 后处理器集合，会工作在 bean 的生命周期各个阶段，此处会添加两个：
  * ApplicationContextAwareProcessor 用来解析 Aware 接口
  * ApplicationListenerDetector 用来识别容器中 ApplicationListener 类型的 bean

![image-20210902182541925](./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210902182541925.png)

**4. postProcessBeanFactory**

* 这一步是空实现，留给子类扩展。
  * 一般 Web 环境的 ApplicationContext 都要利用它注册新的 Scope，完善 Web 下的 BeanFactory
* 这里体现的是模板方法设计模式

**5. invokeBeanFactoryPostProcessors**

* 这一步会调用 beanFactory 后处理器
* beanFactory 后处理器，充当 beanFactory 的扩展点，可以用来补充或修改 BeanDefinition
* 常见的 beanFactory 后处理器有
  * ConfigurationClassPostProcessor – 解析 @Configuration、@Bean、@Import、@PropertySource 等
  * PropertySourcesPlaceHolderConfigurer – 替换 BeanDefinition 中的 ${ }
  * MapperScannerConfigurer – 补充 Mapper 接口对应的 BeanDefinition

![image-20210902183232114](./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210902183232114.png)

**6. registerBeanPostProcessors**

* 这一步是继续从 beanFactory 中找出 bean 后处理器，添加至 beanPostProcessors 集合中
* bean 后处理器，充当 bean 的扩展点，可以工作在 bean 的实例化、依赖注入、初始化阶段，常见的有：
  * AutowiredAnnotationBeanPostProcessor 功能有：解析 @Autowired，@Value 注解
  * CommonAnnotationBeanPostProcessor 功能有：解析 @Resource，@PostConstruct，@PreDestroy
  * AnnotationAwareAspectJAutoProxyCreator 功能有：为符合切点的目标 bean 自动创建代理

![image-20210902183520307](./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210902183520307.png)

**7. initMessageSource**

* 这一步是为 ApplicationContext 添加 messageSource 成员，实现国际化功能
* 去 beanFactory 内找名为 messageSource 的 bean，如果没有，则提供空的 MessageSource 实现

![image-20210902183819984](./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210902183819984.png)

**8. initApplicationContextEventMulticaster**

* 这一步为 ApplicationContext 添加事件广播器成员，即 applicationContextEventMulticaster
* 它的作用是发布事件给监听器
* 去 beanFactory 找名为 applicationEventMulticaster 的 bean 作为事件广播器，若没有，会创建默认的事件广播器
* 之后就可以调用 ApplicationContext.publishEvent(事件对象) 来发布事件

![image-20210902183943469](./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210902183943469.png)

**9. onRefresh**

* 这一步是空实现，留给子类扩展
  * SpringBoot 中的子类在这里准备了 WebServer，即内嵌 web 容器
* 体现的是模板方法设计模式

**10. registerListeners**

* 这一步会从多种途径找到事件监听器，并添加至 applicationEventMulticaster
* 事件监听器顾名思义，用来接收事件广播器发布的事件，有如下来源
  * 事先编程添加的
  * 来自容器中的 bean
  * 来自于 @EventListener 的解析
* 要实现事件监听器，只需要实现 ApplicationListener 接口，重写其中 onApplicationEvent(E e) 方法即可

![image-20210902184343872](./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210902184343872.png)

**11. finishBeanFactoryInitialization**

* 这一步会将 beanFactory 的成员补充完毕，并初始化所有非延迟单例 bean
* conversionService 也是一套转换机制，作为对 PropertyEditor 的补充
* embeddedValueResolvers 即内嵌值解析器，用来解析 @Value 中的 ${ }，借用的是 Environment 的功能
* singletonObjects 即单例池，缓存所有单例对象
  * 对象的创建都分三个阶段，每一阶段都有不同的 bean 后处理器参与进来，扩展功能

![image-20210902184641623](./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210902184641623.png)

**12. finishRefresh**

* 这一步会为 ApplicationContext 添加 lifecycleProcessor 成员，用来控制容器内需要生命周期管理的 bean
* 如果容器中有名称为 lifecycleProcessor 的 bean 就用它，否则创建默认的生命周期管理器
* 准备好生命周期管理器，就可以实现
  * 调用 context 的 start，即可触发所有实现 LifeCycle 接口 bean 的 start
  * 调用 context 的 stop，即可触发所有实现 LifeCycle 接口 bean 的 stop
* 发布 ContextRefreshed 事件，整个 refresh 执行完成

![image-20210902185052433](./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210902185052433.png)



### 2. Spring bean 生命周期

**要求**

* 掌握 Spring bean 的生命周期

**bean 生命周期 概述**

bean 的生命周期从调用 beanFactory 的 getBean 开始，到这个 bean 被销毁，可以总结为以下七个阶段：

1. 处理名称，检查缓存
2. 处理父子容器
3. 处理 dependsOn
4. 选择 scope 策略
5. 创建 bean
6. 类型转换处理
7. 销毁 bean

> ***注意***
>
> * 划分的阶段和名称并不重要，重要的是理解整个过程中做了哪些事情



**1. 处理名称，检查缓存**

* 这一步会处理别名，将别名解析为实际名称
* 对 FactoryBean 也会特殊处理，如果以 & 开头表示要获取 FactoryBean 本身，否则表示要获取其产品
* 这里针对单例对象会检查一级、二级、三级缓存
  * singletonFactories 三级缓存，存放单例工厂对象
  * earlySingletonObjects 二级缓存，存放单例工厂的产品对象
    * 如果发生循环依赖，产品是代理；无循环依赖，产品是原始对象
  * singletonObjects 一级缓存，存放单例成品对象

**2. 处理父子容器**

* 如果当前容器根据名字找不到这个 bean，此时若父容器存在，则执行父容器的 getBean 流程
* 父子容器的 bean 名称可以重复

**3. 处理 dependsOn**

* 如果当前 bean 有通过 dependsOn 指定了非显式依赖的 bean，这一步会提前创建这些 dependsOn 的 bean 
* 所谓非显式依赖，就是指两个 bean 之间不存在直接依赖关系，但需要控制它们的创建先后顺序

**4. 选择 scope 策略**

* 对于 singleton scope，首先到单例池去获取 bean，如果有则直接返回，没有再进入创建流程
* 对于 prototype scope，每次都会进入创建流程
* 对于自定义 scope，例如 request，首先到 request 域获取 bean，如果有则直接返回，没有再进入创建流程

**5.1 创建 bean - 创建 bean 实例**

| **要点**                             | **总结**                                                     |
| ------------------------------------ | ------------------------------------------------------------ |
| 有自定义 TargetSource 的情况         | 由 AnnotationAwareAspectJAutoProxyCreator 创建代理返回       |
| Supplier 方式创建 bean 实例          | 为 Spring 5.0 新增功能，方便编程方式创建  bean  实例         |
| FactoryMethod 方式  创建 bean  实例  | ① 分成静态工厂与实例工厂；② 工厂方法若有参数，需要对工厂方法参数进行解析，利用  resolveDependency；③ 如果有多个工厂方法候选者，还要进一步按权重筛选 |
| AutowiredAnnotationBeanPostProcessor | ① 优先选择带  @Autowired  注解的构造；② 若有唯一的带参构造，也会入选 |
| mbd.getPreferredConstructors         | 选择所有公共构造，这些构造之间按权重筛选                     |
| 采用默认构造                         | 如果上面的后处理器和 BeanDefiniation 都没找到构造，采用默认构造，即使是私有的 |

**5.2 创建 bean - 依赖注入**

| **要点**                             | **总结**                                                     |
| ------------------------------------ | ------------------------------------------------------------ |
| AutowiredAnnotationBeanPostProcessor | 识别   @Autowired  及 @Value  标注的成员，封装为  InjectionMetadata 进行依赖注入 |
| CommonAnnotationBeanPostProcessor    | 识别   @Resource  标注的成员，封装为  InjectionMetadata 进行依赖注入 |
| resolveDependency                    | 用来查找要装配的值，可以识别：① Optional；② ObjectFactory 及 ObjectProvider；③ @Lazy  注解；④ @Value  注解（${  }, #{ }, 类型转换）；⑤ 集合类型（Collection，Map，数组等）；⑥ 泛型和  @Qualifier（用来区分类型歧义）；⑦ primary  及名字匹配（用来区分类型歧义） |
| AUTOWIRE_BY_NAME                     | 根据成员名字找 bean 对象，修改 mbd 的 propertyValues，不会考虑简单类型的成员 |
| AUTOWIRE_BY_TYPE                     | 根据成员类型执行 resolveDependency 找到依赖注入的值，修改  mbd 的 propertyValues |
| applyPropertyValues                  | 根据 mbd 的 propertyValues 进行依赖注入（即xml中 `<property name ref|value/>`） |

**5.3 创建 bean - 初始化**

| **要点**              | **总结**                                                     |
| --------------------- | ------------------------------------------------------------ |
| 内置 Aware 接口的装配 | 包括 BeanNameAware，BeanFactoryAware 等                      |
| 扩展 Aware 接口的装配 | 由 ApplicationContextAwareProcessor 解析，执行时机在  postProcessBeforeInitialization |
| @PostConstruct        | 由 CommonAnnotationBeanPostProcessor 解析，执行时机在  postProcessBeforeInitialization |
| InitializingBean      | 通过接口回调执行初始化                                       |
| initMethod            | 根据 BeanDefinition 得到的初始化方法执行初始化，即 `<bean init-method>` 或 @Bean(initMethod) |
| 创建 aop 代理         | 由 AnnotationAwareAspectJAutoProxyCreator 创建，执行时机在  postProcessAfterInitialization |

**5.4 创建 bean - 注册可销毁 bean**

在这一步判断并登记可销毁 bean

* 判断依据
  * 如果实现了 DisposableBean 或 AutoCloseable 接口，则为可销毁 bean
  * 如果自定义了 destroyMethod，则为可销毁 bean
  * 如果采用 @Bean 没有指定 destroyMethod，则采用自动推断方式获取销毁方法名（close，shutdown）
  * 如果有 @PreDestroy 标注的方法
* 存储位置
  * singleton scope 的可销毁 bean 会存储于 beanFactory 的成员当中
  * 自定义 scope 的可销毁 bean 会存储于对应的域对象当中
  * prototype scope 不会存储，需要自己找到此对象销毁
* 存储时都会封装为 DisposableBeanAdapter 类型对销毁方法的调用进行适配

**6. 类型转换处理**

* 如果 getBean 的 requiredType 参数与实际得到的对象类型不同，会尝试进行类型转换

**7. 销毁 bean**

* 销毁时机
  * singleton bean 的销毁在 ApplicationContext.close 时，此时会找到所有 DisposableBean 的名字，逐一销毁
  * 自定义 scope bean 的销毁在作用域对象生命周期结束时
  * prototype bean 的销毁可以通过自己手动调用 AutowireCapableBeanFactory.destroyBean 方法执行销毁
* 同一 bean 中不同形式销毁方法的调用次序
  * 优先后处理器销毁，即 @PreDestroy
  * 其次 DisposableBean 接口销毁
  * 最后 destroyMethod 销毁（包括自定义名称，推断名称，AutoCloseable 接口 多选一）



### 3. Spring bean 循环依赖

**要求**

* 掌握单例 set 方式循环依赖的原理
* 掌握其它循环依赖的解决方法

**循环依赖的产生**

* 首先要明白，bean 的创建要遵循一定的步骤，必须是创建、注入、初始化三步，这些顺序不能乱

<img src="./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210903085238916.png" alt="image-20210903085238916" style="zoom:50%;" />

* set 方法（包括成员变量）的循环依赖如图所示

  * 可以在【a 创建】和【a set 注入 b】之间加入 b 的整个流程来解决
  * 【b set 注入 a】 时可以成功，因为之前 a 的实例已经创建完毕

  * a 的顺序，及 b 的顺序都能得到保障

<img src="./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210903085454603.png" alt="image-20210903085454603" style="zoom: 33%;" />

* 构造方法的循环依赖如图所示，显然无法用前面的方法解决

<img src="./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210903085906315.png" alt="image-20210903085906315" style="zoom: 50%;" />

**构造循环依赖的解决**

* 思路1
  * a 注入 b 的代理对象，这样能够保证 a 的流程走通
  * 后续需要用到 b 的真实对象时，可以通过代理间接访问

<img src="./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210903091627659.png" alt="image-20210903091627659" style="zoom: 50%;" />

* 思路2
  * a 注入 b 的工厂对象，让 b 的实例创建被推迟，这样能够保证 a 的流程先走通
  * 后续需要用到 b 的真实对象时，再通过 ObjectFactory 工厂间接访问

<img src="./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210903091743366.png" alt="image-20210903091743366" style="zoom:50%;" />

* 示例1：用 @Lazy 为构造方法参数生成代理

```java
public class App60_1 {

    static class A {
        private static final Logger log = LoggerFactory.getLogger("A");
        private B b;

        public A(@Lazy B b) {
            log.debug("A(B b) {}", b.getClass());
            this.b = b;
        }

        @PostConstruct
        public void init() {
            log.debug("init()");
        }
    }

    static class B {
        private static final Logger log = LoggerFactory.getLogger("B");
        private A a;

        public B(A a) {
            log.debug("B({})", a);
            this.a = a;
        }

        @PostConstruct
        public void init() {
            log.debug("init()");
        }
    }

    public static void main(String[] args) {
        GenericApplicationContext context = new GenericApplicationContext();
        context.registerBean("a", A.class);
        context.registerBean("b", B.class);
        AnnotationConfigUtils.registerAnnotationConfigProcessors(context.getDefaultListableBeanFactory());
        context.refresh();
        System.out.println();
    }
}
```

* 示例2：用 ObjectProvider 延迟依赖对象的创建

```java
public class App60_2 {

    static class A {
        private static final Logger log = LoggerFactory.getLogger("A");
        private ObjectProvider<B> b;

        public A(ObjectProvider<B> b) {
            log.debug("A({})", b);
            this.b = b;
        }

        @PostConstruct
        public void init() {
            log.debug("init()");
        }
    }

    static class B {
        private static final Logger log = LoggerFactory.getLogger("B");
        private A a;

        public B(A a) {
            log.debug("B({})", a);
            this.a = a;
        }

        @PostConstruct
        public void init() {
            log.debug("init()");
        }
    }

    public static void main(String[] args) {
        GenericApplicationContext context = new GenericApplicationContext();
        context.registerBean("a", A.class);
        context.registerBean("b", B.class);
        AnnotationConfigUtils.registerAnnotationConfigProcessors(context.getDefaultListableBeanFactory());
        context.refresh();

        System.out.println(context.getBean(A.class).b.getObject());
        System.out.println(context.getBean(B.class));
    }
}
```

* 示例3：用 @Scope 产生代理

```java
public class App60_3 {

    public static void main(String[] args) {
        GenericApplicationContext context = new GenericApplicationContext();
        ClassPathBeanDefinitionScanner scanner = new ClassPathBeanDefinitionScanner(context.getDefaultListableBeanFactory());
        scanner.scan("com.itheima.app60.sub");
        context.refresh();
        System.out.println();
    }
}
```



```java
@Component
class A {
    private static final Logger log = LoggerFactory.getLogger("A");
    private B b;

    public A(B b) {
        log.debug("A(B b) {}", b.getClass());
        this.b = b;
    }

    @PostConstruct
    public void init() {
        log.debug("init()");
    }
}
```



```java
@Scope(proxyMode = ScopedProxyMode.TARGET_CLASS)
@Component
class B {
    private static final Logger log = LoggerFactory.getLogger("B");
    private A a;

    public B(A a) {
        log.debug("B({})", a);
        this.a = a;
    }

    @PostConstruct
    public void init() {
        log.debug("init()");
    }
}
```



* 示例4：用 Provider 接口解决，原理上与 ObjectProvider 一样，Provider 接口是独立的 jar 包，需要加入依赖

```xml
<dependency>
    <groupId>javax.inject</groupId>
    <artifactId>javax.inject</artifactId>
    <version>1</version>
</dependency>
```



```java
public class App60_4 {

    static class A {
        private static final Logger log = LoggerFactory.getLogger("A");
        private Provider<B> b;

        public A(Provider<B> b) {
            log.debug("A({}})", b);
            this.b = b;
        }

        @PostConstruct
        public void init() {
            log.debug("init()");
        }
    }

    static class B {
        private static final Logger log = LoggerFactory.getLogger("B");
        private A a;

        public B(A a) {
            log.debug("B({}})", a);
            this.a = a;
        }

        @PostConstruct
        public void init() {
            log.debug("init()");
        }
    }

    public static void main(String[] args) {
        GenericApplicationContext context = new GenericApplicationContext();
        context.registerBean("a", A.class);
        context.registerBean("b", B.class);
        AnnotationConfigUtils.registerAnnotationConfigProcessors(context.getDefaultListableBeanFactory());
        context.refresh();

        System.out.println(context.getBean(A.class).b.get());
        System.out.println(context.getBean(B.class));
    }
}
```



#### 解决 set 循环依赖的原理

**一级缓存**

<img src="./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210903100752165.png" alt="image-20210903100752165" style="zoom:80%;" />

作用是保证单例对象仅被创建一次

* 第一次走 `getBean("a")` 流程后，最后会将成品 a 放入 singletonObjects 一级缓存
* 后续再走 `getBean("a")` 流程时，先从一级缓存中找，这时已经有成品 a，就无需再次创建

**一级缓存与循环依赖**

<img src="./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210903100914140.png" alt="image-20210903100914140" style="zoom:80%;" />

一级缓存无法解决循环依赖问题，分析如下

* 无论是获取 bean a 还是获取 bean b，走的方法都是同一个 getBean 方法，假设先走 `getBean("a")`
* 当 a 的实例对象创建，接下来执行 `a.setB()` 时，需要走 `getBean("b")` 流程，红色箭头 1
* 当 b 的实例对象创建，接下来执行 `b.setA()` 时，又回到了 `getBean("a")` 的流程，红色箭头 2
* 但此时 singletonObjects 一级缓存内没有成品的 a，陷入了死循环

**二级缓存**

<img src="./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210903101849924.png" alt="image-20210903101849924" style="zoom:80%;" />

解决思路如下：

* 再增加一个 singletonFactories 缓存
* 在依赖注入前，即 `a.setB()` 以及 `b.setA()` 将 a 及 b 的半成品对象（未完成依赖注入和初始化）放入此缓存
* 执行依赖注入时，先看看 singletonFactories 缓存中是否有半成品的对象，如果有拿来注入，顺利走完流程

对于上面的图

* `a = new A()` 执行之后就会把这个半成品的 a 放入 singletonFactories 缓存，即 `factories.put(a)`
* 接下来执行 `a.setB()`，走入 `getBean("b")` 流程，红色箭头 3
* 这回再执行到 `b.setA()` 时，需要一个 a 对象，有没有呢？有！
* `factories.get()` 在 singletonFactories  缓存中就可以找到，红色箭头 4 和 5
* b 的流程能够顺利走完，将 b 成品放入 singletonObject 一级缓存，返回到 a 的依赖注入流程，红色箭头 6

**二级缓存与创建代理**

<img src="./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210903103030877.png" alt="image-20210903103030877" style="zoom:80%;" />

二级缓存无法正确处理循环依赖并且包含有代理创建的场景，分析如下

* spring 默认要求，在 `a.init` 完成之后才能创建代理 `pa = proxy(a)`
* 由于 a 的代理创建时机靠后，在执行 `factories.put(a)` 向 singletonFactories 中放入的还是原始对象
* 接下来箭头 3、4、5 这几步 b 对象拿到和注入的都是原始对象

**三级缓存**

![image-20210903103628639](./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210903103628639.png)

简单分析的话，只需要将代理的创建时机放在依赖注入之前即可，但 spring 仍然希望代理的创建时机在 init 之后，只有出现循环依赖时，才会将代理的创建时机提前。所以解决思路稍显复杂：

* 图中 `factories.put(fa)` 放入的既不是原始对象，也不是代理对象而是工厂对象 fa
* 当检查出发生循环依赖时，fa 的产品就是代理 pa，没有发生循环依赖，fa 的产品是原始对象 a
* 假设出现了循环依赖，拿到了 singletonFactories 中的工厂对象，通过在依赖注入前获得了 pa，红色箭头 5
* 这回 `b.setA()` 注入的就是代理对象，保证了正确性，红色箭头 7
* 还需要把 pa 存入新加的 earlySingletonObjects 缓存，红色箭头 6
* `a.init` 完成后，无需二次创建代理，从哪儿找到 pa 呢？earlySingletonObjects 已经缓存，蓝色箭头 9

当成品对象产生，放入 singletonObject 后，singletonFactories 和 earlySingletonObjects 就中的对象就没有用处，清除即可



### 4. Spring 事务失效

**要求**

* 掌握事务失效的八种场景

**1. 抛出检查异常导致事务不能正确回滚**

```java
@Service
public class Service1 {

    @Autowired
    private AccountMapper accountMapper;

    @Transactional
    public void transfer(int from, int to, int amount) throws FileNotFoundException {
        int fromBalance = accountMapper.findBalanceBy(from);
        if (fromBalance - amount >= 0) {
            accountMapper.update(from, -1 * amount);
            new FileInputStream("aaa");
            accountMapper.update(to, amount);
        }
    }
}
```

* 原因：Spring 默认只会回滚非检查异常

* 解法：配置 rollbackFor 属性
  * `@Transactional(rollbackFor = Exception.class)`



**2. 业务方法内自己 try-catch 异常导致事务不能正确回滚**

```java
@Service
public class Service2 {

    @Autowired
    private AccountMapper accountMapper;

    @Transactional(rollbackFor = Exception.class)
    public void transfer(int from, int to, int amount)  {
        try {
            int fromBalance = accountMapper.findBalanceBy(from);
            if (fromBalance - amount >= 0) {
                accountMapper.update(from, -1 * amount);
                new FileInputStream("aaa");
                accountMapper.update(to, amount);
            }
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        }
    }
}
```

* 原因：事务通知只有捉到了目标抛出的异常，才能进行后续的回滚处理，如果目标自己处理掉异常，事务通知无法知悉

* 解法1：异常原样抛出
  * 在 catch 块添加 `throw new RuntimeException(e);`

* 解法2：手动设置 TransactionStatus.setRollbackOnly()
  * 在 catch 块添加 `TransactionInterceptor.currentTransactionStatus().setRollbackOnly();`



**3. aop 切面顺序导致导致事务不能正确回滚**

```java
@Service
public class Service3 {

    @Autowired
    private AccountMapper accountMapper;

    @Transactional(rollbackFor = Exception.class)
    public void transfer(int from, int to, int amount) throws FileNotFoundException {
        int fromBalance = accountMapper.findBalanceBy(from);
        if (fromBalance - amount >= 0) {
            accountMapper.update(from, -1 * amount);
            new FileInputStream("aaa");
            accountMapper.update(to, amount);
        }
    }
}
```



```java
@Aspect
public class MyAspect {
    @Around("execution(* transfer(..))")
    public Object around(ProceedingJoinPoint pjp) throws Throwable {
        LoggerUtils.get().debug("log:{}", pjp.getTarget());
        try {
            return pjp.proceed();
        } catch (Throwable e) {
            e.printStackTrace();
            return null;
        }
    }
}
```

* 原因：事务切面优先级最低，但如果自定义的切面优先级和他一样，则还是自定义切面在内层，这时若自定义切面没有正确抛出异常…

* 解法1、2：同情况2 中的解法:1、2
* 解法3：调整切面顺序，在 MyAspect 上添加 `@Order(Ordered.LOWEST_PRECEDENCE - 1)` （不推荐）



**4. 非 public 方法导致的事务失效**

```java
@Service
public class Service4 {

    @Autowired
    private AccountMapper accountMapper;

    @Transactional
    void transfer(int from, int to, int amount) throws FileNotFoundException {
        int fromBalance = accountMapper.findBalanceBy(from);
        if (fromBalance - amount >= 0) {
            accountMapper.update(from, -1 * amount);
            accountMapper.update(to, amount);
        }
    }
}
```

* 原因：Spring 为方法创建代理、添加事务通知、前提条件都是该方法是 public 的

* 解法1：改为 public 方法
* 解法2：添加 bean 配置如下（不推荐）

```java
@Bean
public TransactionAttributeSource transactionAttributeSource() {
    return new AnnotationTransactionAttributeSource(false);
}
```



**5. 父子容器导致的事务失效**

```java
package day04.tx.app.service;

// ...

@Service
public class Service5 {

    @Autowired
    private AccountMapper accountMapper;

    @Transactional(rollbackFor = Exception.class)
    public void transfer(int from, int to, int amount) throws FileNotFoundException {
        int fromBalance = accountMapper.findBalanceBy(from);
        if (fromBalance - amount >= 0) {
            accountMapper.update(from, -1 * amount);
            accountMapper.update(to, amount);
        }
    }
}
```

控制器类

```java
package day04.tx.app.controller;

// ...

@Controller
public class AccountController {

    @Autowired
    public Service5 service;

    public void transfer(int from, int to, int amount) throws FileNotFoundException {
        service.transfer(from, to, amount);
    }
}
```

App 配置类

```java
@Configuration
@ComponentScan("day04.tx.app.service")
@EnableTransactionManagement
// ...
public class AppConfig {
    // ... 有事务相关配置
}
```

Web 配置类

```java
@Configuration
@ComponentScan("day04.tx.app")
// ...
public class WebConfig {
    // ... 无事务配置
}
```

现在配置了父子容器，WebConfig 对应子容器，AppConfig 对应父容器，发现事务依然失效

* 原因：子容器扫描范围过大，把未加事务配置的 service 扫描进来

* 解法1：各扫描各的，不要图简便

* 解法2：不要用父子容器，所有 bean 放在同一容器



**6. 调用本类方法导致传播行为失效**

```java
@Service
public class Service6 {

    @Transactional(propagation = Propagation.REQUIRED, rollbackFor = Exception.class)
    public void foo() throws FileNotFoundException {
        LoggerUtils.get().debug("foo");
        bar();
    }

    @Transactional(propagation = Propagation.REQUIRES_NEW, rollbackFor = Exception.class)
    public void bar() throws FileNotFoundException {
        LoggerUtils.get().debug("bar");
    }
}
```

* 原因：本类方法调用不经过代理，因此无法增强

* 解法1：依赖注入自己（代理）来调用

* 解法2：通过 AopContext 拿到代理对象，来调用

* 解法3：通过 CTW，LTW 实现功能增强

解法1

```java
@Service
public class Service6 {

	@Autowired
	private Service6 proxy; // 本质上是一种循环依赖

    @Transactional(propagation = Propagation.REQUIRED, rollbackFor = Exception.class)
    public void foo() throws FileNotFoundException {
        LoggerUtils.get().debug("foo");
		System.out.println(proxy.getClass());
		proxy.bar();
    }

    @Transactional(propagation = Propagation.REQUIRES_NEW, rollbackFor = Exception.class)
    public void bar() throws FileNotFoundException {
        LoggerUtils.get().debug("bar");
    }
}
```

解法2，还需要在 AppConfig 上添加 `@EnableAspectJAutoProxy(exposeProxy = true)`

```java
@Service
public class Service6 {
    
    @Transactional(propagation = Propagation.REQUIRED, rollbackFor = Exception.class)
    public void foo() throws FileNotFoundException {
        LoggerUtils.get().debug("foo");
        ((Service6) AopContext.currentProxy()).bar();
    }

    @Transactional(propagation = Propagation.REQUIRES_NEW, rollbackFor = Exception.class)
    public void bar() throws FileNotFoundException {
        LoggerUtils.get().debug("bar");
    }
}
```



**7. @Transactional 没有保证原子行为**

```java
@Service
public class Service7 {

    private static final Logger logger = LoggerFactory.getLogger(Service7.class);

    @Autowired
    private AccountMapper accountMapper;

    @Transactional(rollbackFor = Exception.class)
    public void transfer(int from, int to, int amount) {
        int fromBalance = accountMapper.findBalanceBy(from);
        logger.debug("更新前查询余额为: {}", fromBalance);
        if (fromBalance - amount >= 0) {
            accountMapper.update(from, -1 * amount);
            accountMapper.update(to, amount);
        }
    }

    public int findBalance(int accountNo) {
        return accountMapper.findBalanceBy(accountNo);
    }
}
```

上面的代码实际上是有 bug 的，假设 from 余额为 1000，两个线程都来转账 1000，可能会出现扣减为负数的情况

* 原因：事务的原子性仅涵盖 insert、update、delete、select … for update 语句，select 方法并不阻塞

<img src="./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210903120436365.png" alt="image-20210903120436365" style="zoom: 50%;" />

* 如上图所示，红色线程和蓝色线程的查询都发生在扣减之前，都以为自己有足够的余额做扣减



**8. @Transactional 方法导致的 synchronized 失效**

针对上面的问题，能否在方法上加 synchronized 锁来解决呢？

```java
@Service
public class Service7 {

    private static final Logger logger = LoggerFactory.getLogger(Service7.class);

    @Autowired
    private AccountMapper accountMapper;

    @Transactional(rollbackFor = Exception.class)
    public synchronized void transfer(int from, int to, int amount) {
        int fromBalance = accountMapper.findBalanceBy(from);
        logger.debug("更新前查询余额为: {}", fromBalance);
        if (fromBalance - amount >= 0) {
            accountMapper.update(from, -1 * amount);
            accountMapper.update(to, amount);
        }
    }

    public int findBalance(int accountNo) {
        return accountMapper.findBalanceBy(accountNo);
    }
}
```

答案是不行，原因如下：

* synchronized 保证的仅是目标方法的原子性，环绕目标方法的还有 commit 等操作，它们并未处于 sync 块内
* 可以参考下图发现，蓝色线程的查询只要在红色线程提交之前执行，那么依然会查询到有 1000 足够余额来转账

![image-20210903120800185](./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210903120800185.png)

* 解法1：synchronized 范围应扩大至代理方法调用

* 解法2：使用 select … for update 替换 select



### 5. Spring MVC 执行流程

**要求**

* 掌握 Spring MVC 的执行流程
* 了解 Spring MVC 的重要组件的作用

**概要**

我把整个流程分成三个阶段

* 准备阶段
* 匹配阶段
* 执行阶段

**准备阶段**

1. 在 Web 容器第一次用到 DispatcherServlet 的时候，会创建其对象并执行 init 方法

2. init 方法内会创建 Spring Web 容器，并调用容器 refresh 方法

3. refresh 过程中会创建并初始化 SpringMVC 中的重要组件， 例如 MultipartResolver，HandlerMapping，HandlerAdapter，HandlerExceptionResolver、ViewResolver 等

4. 容器初始化后，会将上一步初始化好的重要组件，赋值给 DispatcherServlet 的成员变量，留待后用

<img src="./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210903140657163.png" alt="image-20210903140657163" style="zoom: 80%;" />

**匹配阶段**

1. 用户发送的请求统一到达前端控制器 DispatcherServlet

2. DispatcherServlet 遍历所有 HandlerMapping ，找到与路径匹配的处理器

   ① HandlerMapping 有多个，每个 HandlerMapping 会返回不同的处理器对象，谁先匹配，返回谁的处理器。其中能识别 @RequestMapping 的优先级最高

   ② 对应 @RequestMapping 的处理器是 HandlerMethod，它包含了控制器对象和控制器方法信息

   ③ 其中路径与处理器的映射关系在 HandlerMapping 初始化时就会建立好

<img src="./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210903141017502.png" alt="image-20210903141017502" style="zoom:80%;" />

3. 将 HandlerMethod 连同匹配到的拦截器，生成调用链对象 HandlerExecutionChain 返回

<img src="./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210903141124911.png" alt="image-20210903141124911" style="zoom:80%;" />

4. 遍历HandlerAdapter 处理器适配器，找到能处理 HandlerMethod 的适配器对象，开始调用

<img src="./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210903141204799.png" alt="image-20210903141204799" style="zoom:80%;" />

**调用阶段**

1. 执行拦截器 preHandle

<img src="./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210903141445870.png" alt="image-20210903141445870" style="zoom: 67%;" />

2. 由 HandlerAdapter 调用 HandlerMethod

   ① 调用前处理不同类型的参数

   ② 调用后处理不同类型的返回值

<img src="./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210903141658199.png" alt="image-20210903141658199" style="zoom:67%;" />

3. 第 2 步没有异常

   ① 返回 ModelAndView

   ② 执行拦截器 postHandle 方法

   ③ 解析视图，得到 View 对象，进行视图渲染

<img src="./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210903141749830.png" alt="image-20210903141749830" style="zoom:67%;" />

4. 第 2 步有异常，进入 HandlerExceptionResolver 异常处理流程

<img src="./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210903141844185.png" alt="image-20210903141844185" style="zoom:67%;" />

5. 最后都会执行拦截器的 afterCompletion 方法

6. 如果控制器方法标注了 @ResponseBody 注解，则在第 2 步，就会生成 json 结果，并标记 ModelAndView 已处理，这样就不会执行第 3 步的视图渲染



### 6. Spring 注解

**要求**

* 掌握 Spring 常见注解

> ***提示***
>
> * 注解的详细列表请参考：面试题-spring-注解.xmind
> * 下面列出了视频中重点提及的注解，考虑到大部分注解同学们已经比较熟悉了，仅对个别的作简要说明



**事务注解**

* @EnableTransactionManagement，会额外加载 4 个 bean
  * BeanFactoryTransactionAttributeSourceAdvisor 事务切面类
  * TransactionAttributeSource 用来解析事务属性
  * TransactionInterceptor 事务拦截器
  * TransactionalEventListenerFactory 事务监听器工厂
* @Transactional

**核心**

* @Order

**切面**

* @EnableAspectJAutoProxy
  * 会加载 AnnotationAwareAspectJAutoProxyCreator，它是一个 bean 后处理器，用来创建代理
  * 如果没有配置 @EnableAspectJAutoProxy，又需要用到代理（如事务）则会使用 InfrastructureAdvisorAutoProxyCreator 这个 bean 后处理器

**组件扫描与配置类**

* @Component

* @Controller

* @Service

* @Repository

* @ComponentScan

* @Conditional 

* @Configuration

  * 配置类其实相当于一个工厂, 标注 @Bean 注解的方法相当于工厂方法
  * @Bean 不支持方法重载, 如果有多个重载方法, 仅有一个能入选为工厂方法
  * @Configuration 默认会为标注的类生成代理, 其目的是保证 @Bean 方法相互调用时, 仍然能保证其单例特性
  * @Configuration 中如果含有 BeanFactory 后处理器, 则实例工厂方法会导致 MyConfig 提前创建, 造成其依赖注入失败，解决方法是改用静态工厂方法或直接为 @Bean 的方法参数依赖注入, 针对 Mapper 扫描可以改用注解方式

* @Bean

* @Import 

  * 四种用法

    ① 引入单个 bean

    ② 引入一个配置类

    ③ 通过 Selector 引入多个类

    ④ 通过 beanDefinition 注册器

  * 解析规则

    * 同一配置类中, @Import 先解析  @Bean 后解析
    * 同名定义, 默认后面解析的会覆盖前面解析的
    * 不允许覆盖的情况下, 如何能够让 MyConfig(主配置类) 的配置优先? (虽然覆盖方式能解决)
    * 采用 DeferredImportSelector，因为它最后工作, 可以简单认为先解析 @Bean, 再 Import

* @Lazy

  * 加在类上，表示此类延迟实例化、初始化
  * 加在方法参数上，此参数会以代理方式注入

* @PropertySource

**依赖注入**

* @Autowired
* @Qualifier
* @Value

**mvc mapping**

* @RequestMapping，可以派生多个注解如 @GetMapping 等

**mvc rest**

* @RequestBody
* @ResponseBody，组合 @Controller =>  @RestController
* @ResponseStatus

**mvc 统一处理**

* @ControllerAdvice，组合 @ResponseBody => @RestControllerAdvice
* @ExceptionHandler

**mvc 参数**

* @PathVariable

**mvc ajax**

* @CrossOrigin

**boot auto**

* @SpringBootApplication
* @EnableAutoConfiguration
* @SpringBootConfiguration

**boot condition**

* @ConditionalOnClass，classpath 下存在某个 class 时，条件才成立
* @ConditionalOnMissingBean，beanFactory 内不存在某个 bean 时，条件才成立
* @ConditionalOnProperty，配置文件中存在某个 property（键、值）时，条件才成立

**boot properties**

* @ConfigurationProperties，会将当前 bean 的属性与配置文件中的键值进行绑定
* @EnableConfigurationProperties，会添加两个较为重要的 bean
  * ConfigurationPropertiesBindingPostProcessor，bean 后处理器，在 bean 初始化前调用下面的 binder
  * ConfigurationPropertiesBinder，真正执行绑定操作



### 7. SpringBoot 自动配置原理

**要求**

* 掌握 SpringBoot 自动配置原理

**自动配置原理**

@SpringBootConfiguration 是一个组合注解，由 @ComponentScan、@EnableAutoConfiguration 和 @SpringBootConfiguration 组成

1. @SpringBootConfiguration 与普通 @Configuration 相比，唯一区别是前者要求整个 app 中只出现一次
2. @ComponentScan
   * excludeFilters - 用来在组件扫描时进行排除，也会排除自动配置类

3. @EnableAutoConfiguration 也是一个组合注解，由下面注解组成
   * @AutoConfigurationPackage – 用来记住扫描的起始包
   * @Import(AutoConfigurationImportSelector.class) 用来加载 `META-INF/spring.factories` 中的自动配置类

**为什么不使用 @Import 直接引入自动配置类**

有两个原因：

1. 让主配置类和自动配置类变成了强耦合，主配置类不应该知道有哪些从属配置
2. 直接用 `@Import(自动配置类.class)`，引入的配置解析优先级较高，自动配置类的解析应该在主配置没提供时作为默认配置

因此，采用了 `@Import(AutoConfigurationImportSelector.class)`

* 由 `AutoConfigurationImportSelector.class` 去读取 `META-INF/spring.factories` 中的自动配置类，实现了弱耦合。
* 另外 `AutoConfigurationImportSelector.class` 实现了 DeferredImportSelector 接口，让自动配置的解析晚于主配置的解析



### 8. Spring 中的设计模式

**要求**

* 掌握 Spring 中常见的设计模式

**1. Spring 中的 Singleton**

请大家区分 singleton pattern 与 Spring 中的 singleton bean

* 根据单例模式的目的 *Ensure a class only has one instance, and provide a global point of access to it* 
* 显然 Spring 中的 singleton bean 并非实现了单例模式，singleton bean 只能保证每个容器内，相同 id 的 bean 单实例
* 当然 Spring 中也用到了单例模式，例如
  * org.springframework.transaction.TransactionDefinition#withDefaults
  * org.springframework.aop.TruePointcut#INSTANCE
  * org.springframework.aop.interceptor.ExposeInvocationInterceptor#ADVISOR
  * org.springframework.core.annotation.AnnotationAwareOrderComparator#INSTANCE
  * org.springframework.core.OrderComparator#INSTANCE

**2. Spring 中的 Builder**

定义 *Separate the construction of a complex object from its representation so that the same construction process can create different representations* 

它的主要亮点有三处：

1. 较为灵活的构建产品对象

2. 在不执行最后 build 方法前，产品对象都不可用

3. 构建过程采用链式调用，看起来比较爽

Spring 中体现 Builder 模式的地方：

* org.springframework.beans.factory.support.BeanDefinitionBuilder

* org.springframework.web.util.UriComponentsBuilder

* org.springframework.http.ResponseEntity.HeadersBuilder

* org.springframework.http.ResponseEntity.BodyBuilder

**3. Spring 中的 Factory Method**

定义 *Define an interface for creating an object, but let subclasses decide which class to instantiate. Factory Method lets a class defer instantiation to subclasses* 

根据上面的定义，Spring 中的 ApplicationContext 与 BeanFactory 中的 getBean 都可以视为工厂方法，它隐藏了 bean （产品）的创建过程和具体实现

Spring 中其它工厂：

* org.springframework.beans.factory.FactoryBean

* @Bean 标注的静态方法及实例方法

* ObjectFactory 及 ObjectProvider

前两种工厂主要封装第三方的 bean 的创建过程，后两种工厂可以推迟 bean 创建，解决循环依赖及单例注入多例等问题

**4. Spring 中的 Adapter**

定义 *Convert the interface of a class into another interface clients expect. Adapter lets classes work together that couldn't otherwise because of incompatible interfaces* 

典型的实现有两处：

* org.springframework.web.servlet.HandlerAdapter – 因为控制器实现有各种各样，比如有
  * 大家熟悉的 @RequestMapping 标注的控制器实现
  * 传统的基于 Controller 接口（不是 @Controller注解啊）的实现
  * 较新的基于 RouterFunction 接口的实现
  * 它们的处理方法都不一样，为了统一调用，必须适配为 HandlerAdapter 接口
* org.springframework.beans.factory.support.DisposableBeanAdapter – 因为销毁方法多种多样，因此都要适配为 DisposableBean 来统一调用销毁方法 

**5. Spring 中的 Composite**

定义 *Compose objects into tree structures to represent part-whole hierarchies. Composite lets clients treat individual objects and compositions of objects uniformly* 

典型实现有：

* org.springframework.web.method.support.HandlerMethodArgumentResolverComposite
* org.springframework.web.method.support.HandlerMethodReturnValueHandlerComposite
* org.springframework.web.servlet.handler.HandlerExceptionResolverComposite
* org.springframework.web.servlet.view.ViewResolverComposite

composite 对象的作用是，将分散的调用集中起来，统一调用入口，它的特征是，与具体干活的实现实现同一个接口，当调用 composite 对象的接口方法时，其实是委托具体干活的实现来完成

**6. Spring 中的 Decorator**

定义 *Attach additional responsibilities to an object dynamically. Decorators provide a flexible alternative to subclassing for extending functionality* 

典型实现：

* org.springframework.web.util.ContentCachingRequestWrapper

**7. Spring 中的 Proxy**

定义 *Provide a surrogate or placeholder for another object to control access to it* 

装饰器模式注重的是功能增强，避免子类继承方式进行功能扩展，而代理模式更注重控制目标的访问

典型实现：

* org.springframework.aop.framework.JdkDynamicAopProxy
* org.springframework.aop.framework.ObjenesisCglibAopProxy

**8. Spring 中的 Chain of Responsibility**

定义 *Avoid coupling the sender of a request to its receiver by giving more than one object a chance to handle the request. Chain the receiving objects and pass the request along the chain until an object handles it* 

典型实现：

* org.springframework.web.servlet.HandlerInterceptor

**9. Spring 中的 Observer**

定义 *Define a one-to-many dependency between objects so that when one object changes state, all its dependents are notified and updated automatically* 

典型实现：

* org.springframework.context.ApplicationListener
* org.springframework.context.event.ApplicationEventMulticaster
* org.springframework.context.ApplicationEvent

**10. Spring 中的 Strategy**

定义 *Define a family of algorithms, encapsulate each one, and make them interchangeable. Strategy lets the algorithm vary independently from clients that use it* 

典型实现：

* org.springframework.beans.factory.support.InstantiationStrategy
* org.springframework.core.annotation.MergedAnnotations.SearchStrategy
* org.springframework.boot.autoconfigure.condition.SearchStrategy

**11. Spring 中的 Template Method**

定义 *Define the skeleton of an algorithm in an operation, deferring some steps to subclasses. Template Method lets subclasses redefine certain steps of an algorithm without changing the algorithm's structure* 

典型实现：

* 大部分以 Template 命名的类，如 JdbcTemplate，TransactionTemplate
* 很多以 Abstract 命名的类，如 AbstractApplicationContext

+++

## 数据库篇

### 1. 隔离级别

**要求**

* 掌握四种隔离级别与相关的错误现象



**未提交读**

* 读到其它事务未提交的数据（最新的版本）

* 错误现象：有脏读、不可重复读、幻读现象

**脏读现象**

| **tx1**                                                   | **tx2**                                              |
| --------------------------------------------------------- | ---------------------------------------------------- |
| set session transaction isolation level read uncommitted; |                                                      |
| start transaction;                                        |                                                      |
| select * from account;  /*两个账户都为 1000*/             |                                                      |
|                                                           | start transaction;                                   |
|                                                           | update account set balance = 2000 where accountNo=1; |
| select * from account; /*1号账户2000, 2号账户1000*/       |                                                      |

* tx2 未提交的情况下，tx1 仍然读取到了它的更改



**提交读（RC）**

* 读到其它事务已提交的数据（最新已提交的版本）

* 错误现象：有不可重复读、幻读现象

* 使用场景：希望看到最新的有效值

**不可重复读现象**

| **tx1**                                                  | **tx2**                                               |
| -------------------------------------------------------- | ----------------------------------------------------- |
| set  session transaction isolation level read committed; |                                                       |
| start  transaction;                                      |                                                       |
| select  * from account; /*两个账户都为 1000*/            |                                                       |
|                                                          | update  account set balance = 2000 where accountNo=1; |
| select  * from account; /*1号账户2000, 2号账户1000*/     |                                                       |

* tx1 在同一事务内，两次读取的结果不一致，当然，此时 tx2 的事务已提交



**可重复读（RR）** 

* 在事务范围内，多次读能够保证一致性（快照建立时最新已提交版本）

* 错误现象：有幻读现象，可以用加锁避免

* 使用场景：事务内要求更强的一致性，但看到的未必是最新的有效值

**幻读现象**

| **tx1**                                                      | **tx2**                              |
| ------------------------------------------------------------ | ------------------------------------ |
| set session transaction isolation level repeatable read;     |                                      |
| start transaction;                                           |                                      |
| select * from account; /*存在 1,2 两个账户*/                 |                                      |
|                                                              | insert into account values(3, 1000); |
| select * from account; /*发现还是只有 1,2 两个账户*/         |                                      |
| insert into account values(3, 5000);  /* ERROR  1062 (23000): Duplicate entry '3' for key 'PRIMARY'  */ |                                      |

* tx1 查询时并没有发现 3 号账户，执行插入时却发现主键冲突异常，就好像出现了幻觉一样

**加锁避免幻读**

| **tx1**                                                  | **tx2**                                          |
| -------------------------------------------------------- | ------------------------------------------------ |
| set session transaction isolation level repeatable read; |                                                  |
| start transaction;                                       |                                                  |
| select * from account; /*存在 1,2 两个账户*/             |                                                  |
| select * from account where accountNo=3 for update;      |                                                  |
|                                                          | insert into account values(3, 1000);  /* 阻塞 */ |
| insert into account values(3, 5000);                     |                                                  |

* 在 for update 这行语句执行时，虽然此时 3 号账户尚不存在，但 MySQL 在 repeatable read 隔离级别下会用间隙锁，锁住 2 号记录与正无穷大之间的间隙
* 此时 tx2 想插入 3 号记录就不行了，被间隙锁挡住了



**串行读** 

* 在事务范围内，仅有读读可以并发，读写或写写会阻塞其它事务，用这种办法保证更强的一致性

* 错误现象：无

**串行读避免幻读**

| **tx1**                                               | **tx2**                                           |
| ----------------------------------------------------- | ------------------------------------------------- |
| set session transaction isolation level serializable; |                                                   |
| start transaction;                                    |                                                   |
| select  * from account; /* 存在 1,2 两个账户 */       |                                                   |
|                                                       | insert  into account values(3, 1000);  /* 阻塞 */ |
| insert  into account values(3, 5000);                 |                                                   |

* 串行读隔离级别下，普通的 select 也会加共享读锁，其它事务的查询可以并发，但增删改就只能阻塞了



### 2. 快照读与当前读

**要求**

* 理解快照读与当前读
* 了解快照产生的时机

**当前读**

即读取最新提交的数据

* select … for update
* select ... lock in share mode
* insert、update、delete，都会按最新提交的数据进行操作

当前读本质上是基于锁的并发读操作

**快照读**

读取某一个快照建立时（可以理解为某一时间点）的数据，也称为一致性读。快照读主要体现在 select 时，而不同隔离级别下，select 的行为不同

* 在 Serializable 隔离级别下 - 普通 select 也变成当前读，即加共享读锁

* 在 RC 隔离级别下 - 每次 select 都会建立新的快照

* 在 RR 隔离级别下
  * 事务启动后，首次 select 会建立快照
  * 如果事务启动选择了 with consistent snapshot，事务启动时就建立快照
  * 基于旧数据的修改操作，会重新建立快照

快照读本质上读取的是历史数据（原理是回滚段），属于无锁查询

**RR 下，快照建立时机 - 第一次 select 时**

| **tx1**                                                      | **tx2**                                               |
| ------------------------------------------------------------ | ----------------------------------------------------- |
| set  session transaction isolation level repeatable read;    |                                                       |
| start  transaction;                                          |                                                       |
| select  * from account;  /* 此时建立快照，两个账户为 1000  */ |                                                       |
|                                                              | update  account set balance = 2000 where accountNo=1; |
| select  * from account;  /* 两个账户仍为 1000 */             |                                                       |

* 快照一旦建立，以后的查询都基于此快照，因此 tx1 中第二次 select 仍然得到 1 号账户余额为 1000

如果 tx2 的 update 先执行

| **tx1**                                                      | **tx2**                                               |
| ------------------------------------------------------------ | ----------------------------------------------------- |
| set  session transaction isolation level repeatable read;    |                                                       |
| start  transaction;                                          |                                                       |
|                                                              | update  account set balance = 2000 where accountNo=1; |
| select  * from account; /* 此时建立快照，1号余额已经为2000 */ |                                                       |

**RR 下，快照建立时机 - 事务启动时**

如果希望事务启动时就建立快照，可以添加 with consistent snapshot 选项

| **tx1**                                                      | **tx2**                                               |
| ------------------------------------------------------------ | ----------------------------------------------------- |
| set  session transaction isolation level repeatable read;    |                                                       |
| start  transaction with consistent snapshot; /* 此时建立快照，两个账户为 1000  */ |                                                       |
|                                                              | update  account set balance = 2000 where accountNo=1; |
| select  * from account; /* 两个账户仍为 1000 */              |                                                       |

**RR 下，快照建立时机 - 修改数据时**

| **tx1**                                                     | **tx2**                                                     |
| ----------------------------------------------------------- | ----------------------------------------------------------- |
| set  session transaction isolation level repeatable read;   |                                                             |
| start  transaction;                                         |                                                             |
| select  * from account; /* 此时建立快照，两个账户为 1000 */ |                                                             |
|                                                             | update  account set balance=balance+1000 where accountNo=1; |
| update  account set balance=balance+1000 where accountNo=1; |                                                             |
| select  * from account; /* 1号余额为3000 */                 |                                                             |

* tx1 内的修改必须重新建立快照，否则，就会发生丢失更新的问题



### 3. InnoDB vs MyISAM

**要求**

* 掌握 InnoDB 与 MyISAM 的主要区别
* 尤其注意它们在索引结构上的区别

**InnoDB**

* 索引分为聚簇索引与二级索引
  * 聚簇索引：主键值作为索引数据，叶子节点还包含了所有字段数据，索引和数据是存储在一起的
  * 二级索引：除主键外的其它字段建立的索引称为二级索引。被索引的字段值作为索引数据，叶子节点还包含了主键值

* 支持事务
  * 通过 undo log 支持事务回滚、当前读（多版本查询）
  * 通过 redo log 实现持久性
  * 通过两阶段提交实现一致性
  * 通过当前读、锁实现隔离性

* 支持行锁、间隙锁

* 支持外键

**MyISAM**

* 索引只有一种
  * 被索引字段值作为索引数据，叶子节点还包含了该记录数据页地址，数据和索引是分开存储的
* 不支持事务，没有 undo log 和 redo log

* 仅支持表锁

* 不支持外键

* 会保存表的总行数



**InnoDB 索引特点**

聚簇索引：主键值作为索引数据，叶子节点还包含了所有字段数据，索引和数据是存储在一起的

![image-20210901155308778](./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210901155308778.png)

* 主键即 7369、7499、7521 等

二级索引：除主键外的其它字段建立的索引称为二级索引。被索引的字段值作为索引数据，叶子节点还包含了主键值

![image-20210901155317460](./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210901155317460.png)

* 上图中 800、950、1100 这些是工资字段的值，根据它们建立了二级索引

![image-20210901155327838](./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210901155327838.png)

* 上图中，如果执行查询 `select empno, ename, sal from emp where sal = 800`，这时候可以利用二级索引定位到 800 这个工资，同时还能知道主键值 7369
* 但 select 字句中还出现了 ename 字段，在二级索引中不存在，因此需要根据主键值 7369 查询聚簇索引来获取 ename 的信息，这个过程俗称**回表**



**MyISAM 索引特点**

被索引字段值作为索引数据，叶子节点还包含了该记录数据页地址，数据和索引是分开存储的

![image-20210901155226621](./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210901155226621.png)



### 4. 索引

**要求**

* 了解常见索引与它们的适用场景，尤其是 B+Tree 索引的特点
* 掌握索引用于排序，以及失效情况
* 掌握索引用于筛选，以及失效情况
* 理解索引条件下推
* 理解二级索引覆盖



#### 索引基础

**常见索引**

* 哈希索引
  * 理想时间复杂度为 $O(1)$
  * 适用场景：适用于等值查询的场景，内存数据的索引
  * 典型实现：Redis，MySQL 的 memory 引擎
* 平衡二叉树索引 
  * 查询和更新的时间复杂度都是 $O(log_2(n))$
  * 适用场景：适用于等值查询以及范围查询；适合内存数据的索引，但不适合磁盘数据的索引，可以认为**树的高度决定了磁盘 I/O 的次数**，百万数据树高约为 20
* BTree 索引
  * BTree 其实就是 n 叉树，分叉多意味着节点中的孩子（key）多，树高自然就降低了
  * 分叉数由页大小和行（包括 key 与 value）大小决定
    * 假设页大小为 16k，每行 40 个字节，那么分叉数就为 16k / 40 ≈ 410
    * 而分叉为 410，则百万数据树高约为3，仅 3 次 I/O 就能找到所需数据 
  * **局部性原理**：每次 I/O 按页为单位读取数据，把多个 **key 相邻**的行放在同一页中（每页就是树上一个节点），能进一步减少 I/O

* B+ 树索引 
  * 在 BTree 的基础上做了改进，索引上只存储 key，这样能进一步增加分叉数，假设 key 占 13 个字节，那么一页数据分叉数可以到 1260，树高可以进一步下降为 2

> ***树高计算公式***
>
> * $log_{10}(N) /  log_{10}(M)$ 其中 N 为数据行数，M 为分叉数



**BTree vs B+Tree**

* 无论 BTree 还是 B+Tree，每个叶子节点到根节点距离都相同
* BTree key 及 value 在每个节点上，无论叶子还是非叶子节点

<img src="./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210901170943656.png" alt="image-20210901170943656" style="zoom:80%;" />

* B+Tree 普通节点只存 key，叶子节点才存储 key 和 value，因此分叉数可以更多
  * 不过也请注意，普通节点上的 key 有的会与叶子节点的 key 重复
* B+Tree 必须到达叶子节点才能找到 value
* B+Tree 叶子节点用链表连接，可以方便范围查询及全表遍历

<img src="./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210901170954328.png" alt="image-20210901170954328" style="zoom:80%;" />

> 注：这两张图都是仅画了 key，未画 value



**B+Tree 新增 key**

假设阶数（m）为5

1. 若为空树，那么直接创建一个节点，插入 key 即可，此时这个叶子结点也是根结点。例如，插入 5

   ![image-20210901174939408](./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210901174939408.png)

2. 插入时，若当前结点 key 的个数小于阶数，则插入结束

3. 依次插入 8、10、15，按 key 大小升序

   ![image-20210901175021697](./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210901175021697.png)

4. 插入 16，这时到达了阶数限制，所以要进行分裂

   ![image-20210901175057315](./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210901175057315.png)

5. **叶子节点分裂规则**：将这个叶子结点分裂成左右两个叶子结点，左叶子结点包含前 m/2 个（2个）记录，右结点包含剩下的记录，将中间的 key 进位到父结点中。**注意**：中间的 key 仍会保留在叶子节点一份

   ![image-20210901175106713](./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210901175106713.png)

6. 插入 17

   ![image-20210901175333804](./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210901175333804.png)

7. 插入 18，这时当前结点的 key 个数到达 5，进行分裂

   ![image-20210901175352807](./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210901175352807.png)

8. 分裂成两个结点，左结点 2 个记录，右结点 3 个记录，key 16 进位到父结点中

   ![image-20210901175413305](./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210901175413305.png)

9. 插入 19、20、21、22、6、9

   ![image-20210901175440205](./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210901175440205.png)

10. 插入 7，当前结点的 key 个数到达 5，需要分裂

    ![image-20210901175518737](./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210901175518737-16304901199481.png)

11. 分裂后 key 7 进入到父结点中，这时父节点 key 个数也到达 5

    ![image-20210901175544893](./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210901175544893.png)

12. **非叶子节点分裂规则**：左子结点包含前 (m-1)/2 个 key，将中间的 key 进位到父结点中（**不保留**），右子节点包含剩余的 key

    ![image-20210901175617464](./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210901175617464.png)



**B+Tree 查询 key**

以查询 15 为例

* 第一次 I/O

  ![image-20210901175721826](./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210901175721826.png)

* 第二次 I/O

  ![image-20230222195056033](./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20230222195056033.png)

* 第三次 I/O

  ![image-20210901175801859](./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210901175801859.png)



**B+Tree 删除叶子节点 key**

1. 初始状态

   ![image-20210901180320860](./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210901180320860.png)

2. **删完有富余**。即删除后结点的key的个数 > m/2 – 1，删除操作结束，例如删除 22

   ![image-20210901180331158](./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210901180331158.png)

3. **删完没富余，但兄弟节点有富余**。即兄弟结点 key 有富余（ > m/2 – 1 ），向兄弟结点借一个记录，同时替换父节点，例如删除 15

   ![image-20210901180356515](./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210901180356515.png)

4. **兄弟节点也不富余，合并兄弟叶子节点**。即兄弟节点合并成一个新的叶子结点，并删除父结点中的key，将当前结点指向父结点，例如删除 7

   ![image-20210901180405393](./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210901180405393.png)

5. 也需要删除非叶子节点中的 7，并替换父节点保证区间仍有效

   ![image-20210901180422491](./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210901180422491.png)

6. 左右兄弟都不够借，合并

   ![image-20230222194718193](./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20230222194718193.png)



**B+Tree 删除非叶子节点 key**

接着上面的操作

1. 非叶子节点 key 的个数 > m/2 – 1，则删除操作结束，否则执行 2

2. 若**兄弟结点有富余**，父结点 key 下移，兄弟结点 key 上移，删除结束，否则执行 3

3. 若**兄弟节点没富余**，当前结点和兄弟结点及父结点合并成一个新的结点。重复 1

   ![image-20230222195926733](./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20230222195926733.png)

   ![image-20210901180516139](./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210901180516139.png)



#### 命中索引

> **准备数据**
>
> 1. 修改 MySQL 配置文件，在 [mysqld] 下添加 `secure_file_priv=` 重启 MySQL 服务器，让选项生效
>
> 2. 执行 db.sql 内的脚本，建表
>
> 3. 执行 `LOAD DATA INFILE 'D:\\big_person.txt' INTO TABLE big_person;` 注意实际路径根据情况修改
>    * 测试表 big_person（此表数据量较大，如果与其它表数据一起提供不好管理，故单独提供），数据行数 100 万条，列个数 15 列。为了更快速导入数据，这里采用了 load data infile 命令配合 *.txt 格式数据

**索引用于排序**

```sql
/* 测试单列索引并不能在多列排序时加速 */
create index first_idx on big_person(first_name);
create index last_idx on big_person(last_name);
explain select * from big_person order by last_name, first_name limit 10; 

/* 多列排序需要用组合索引 */
alter table big_person drop index first_idx;
alter table big_person drop index last_idx;
create index last_first_idx on big_person(last_name,first_name);

/* 多列排序需要遵循最左前缀原则, 第1个查询可以利用索引，第2,3查询不能利用索引 */
explain select * from big_person order by last_name, first_name limit 10; 
explain select * from big_person order by first_name, last_name limit 10; 
explain select * from big_person order by first_name limit 10; 

/* 多列排序升降序需要一致，查询1可以利用索引，查询2不能利用索引*/
explain select * from big_person order by last_name desc, first_name desc limit 10; 
explain select * from big_person order by last_name desc, first_name asc limit 10; 
```

> ***最左前缀原则***
>
> 若建立组合索引 (a,b,c)，则可以**利用**到索引的排序条件是：
>
> * order by a
> * order by a, b
> * order by a, b, c



**索引用于 where 筛选**

* 参考 https://dev.mysql.com/doc/refman/8.0/en/multiple-column-indexes.html

```sql
/* 模糊查询需要遵循字符串最左前缀原则，查询2可以利用索引，查询1,3不能利用索引 */
explain SELECT * FROM big_person WHERE first_name LIKE 'dav%' LIMIT 5;
explain SELECT * FROM big_person WHERE last_name LIKE 'dav%' LIMIT 5;
explain SELECT * FROM big_person WHERE last_name LIKE '%dav' LIMIT 5;

/* 组合索引需要遵循最左前缀原则，查询1,2可以利用索引，查询3,4不能利用索引 */
create index province_city_county_idx on big_person(province,city,county);
explain SELECT * FROM big_person WHERE province = '上海' AND city='宜兰县' AND county='中西区';
explain SELECT * FROM big_person WHERE county='中西区' AND city='宜兰县' AND province = '上海';
explain SELECT * FROM big_person WHERE city='宜兰县' AND county='中西区';
explain SELECT * FROM big_person WHERE county='中西区';

/* 函数及计算问题，一旦在字段上应用了计算或函数，都会造成索引失效。查询2可以利用索引，查询1不能利用索引 */
create index birthday_idx on big_person(birthday);
explain SELECT * FROM big_person WHERE ADDDATE(birthday,1)='2005-02-10';
explain SELECT * FROM big_person WHERE birthday=ADDDATE('2005-02-10',-1);

/* 隐式类型转换问题 phone为char类型
	* 查询1会发生隐式类型转换等价于在phone上应用了函数，造成索引失效
	* 查询2字段与值类型相同不会类型转换，可以利用索引
*/
create index phone_idx on big_person(phone);
explain SELECT * FROM big_person WHERE phone = 13000013934;
explain SELECT * FROM big_person WHERE phone = '13000013934';
```

> ***最左前缀原则（leftmost prefix）***
>
> 若建立组合索引 (a,b,c)，则可以**利用**到索引的查询条件是：
>
> * where a = ?
> * where a = ? and b = ? （注意与条件的先后次序无关，也可以是 where b = ? and a = ?，只要出现即可）
> * where a = ? and b = ? and c = ? （注意事项同上）
>
> **不能利用**的例子：
>
> * where b = ?
> * where b = ? and c = ?
> * where c = ?
>
> 特殊情况：
>
> * where a = ? and c = ?（a = ? 会利用索引，但 c = ? 不能利用索引加速，会触发索引条件下推）



**索引条件下推**

* 参考 https://dev.mysql.com/doc/refman/8.0/en/index-condition-pushdown-optimization.html

```sql
/* 查询 1,2,3,4 都能利用索引，但 4 相当于部分利用了索引，会触发索引条件下推 */
explain SELECT * FROM big_person WHERE province = '上海';
explain SELECT * FROM big_person WHERE province = '上海' AND city='嘉兴市';
explain SELECT * FROM big_person WHERE province = '上海' AND city='嘉兴市' AND county='中西区';
explain SELECT * FROM big_person WHERE province = '上海' AND county='中西区';
```

> ***索引条件下推***
>
> * MySQL 执行条件判断的时机有两处：
>   * 服务层（上层，不包括索引实现）
>   * 引擎层（下层，包括了索引实现，可以利用）
>   * 上面查询 4 中有 province 条件能够利用索引，在引擎层执行，但 county 条件仍然要交给服务层处理
> * 在 5.6 之前，服务层需要判断所有记录的 county 条件，性能非常低
> * 5.6 以后，引擎层会先根据 province 条件过滤，满足条件的记录才在服务层处理 county 条件

我们现在用的是 5.6 以上版本，所以没有体会，可以用下面的语句关闭索引下推优化，再测试一下性能

```sql
SET optimizer_switch = 'index_condition_pushdown=off';
SELECT * FROM big_person WHERE province = '上海' AND county='中西区';
```



**二级索引覆盖**

```sql
explain SELECT * FROM big_person WHERE province = '上海' AND city='宜兰县' AND county= '中西区';
explain SELECT id,province,city,county FROM big_person WHERE province = '上海' AND city='宜兰县' AND county='中西区';
```

根据查询条件查询 1，2 都会先走二级索引，但是二级索引仅包含了 (province, city, county) 和 id 信息

* 查询 1 是 select *，因此还有一些字段二级索引中没有，需要回表（查询聚簇索引）来获取其它字段信息
* 查询 2 的 select 中明确指出了需要哪些字段，这些字段在二级索引都有，就避免了回表查询



**其它注意事项**

* 表连接需要在连接字段上建立索引
* 不要迷信网上说法，具体情况具体分析

例如：

```sql
create index first_idx on big_person(first_name);

/* 不会利用索引，因为优化器发现查询记录数太多，还不如直接全表扫描 */
explain SELECT * FROM big_person WHERE first_name > 'Jenni';

/* 会利用索引，因为优化器发现查询记录数不太多 */
explain SELECT * FROM big_person WHERE first_name > 'Willia';

/* 同一字段的不同值利用 or 连接，会利用索引 */
explain select * from big_person where id = 1 or id = 190839;

/* 不同字段利用 or 连接，会利用索引(底层分别用了两个索引) */
explain select * from big_person where first_name = 'David' or last_name = 'Thomas';

/* in 会利用索引 */
explain select * from big_person where first_name in ('Mark', 'Kevin','David'); 

/* not in 不会利用索引的情况 */
explain select * from big_person where first_name not in ('Mark', 'Kevin','David');

/* not in 会利用索引的情况 */
explain select id from big_person where first_name not in ('Mark', 'Kevin','David');
```

* 以上实验基于 5.7.27，其它如 !=、is null、is not null 是否使用索引都会跟版本、实际数据相关，以优化器结果为准



### 5. 查询语句执行流程

**要求**

* 了解查询语句执行流程



**执行 SQL 语句 `select * from user where id = 1` 时发生了什么**

![image-20210902082718756](./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210902082718756.png)

1. 连接器：负责建立连接、检查权限、连接超时时间由 wait_timeout 控制，默认 8 小时

2. 查询缓存：会将 SQL 和查询结果以键值对方式进行缓存，修改操作会以表单位导致缓存失效

3. 分析器：词法、语法分析

4. 优化器：决定用哪个索引，决定表的连接顺序等

5. 执行器：根据存储引擎类型，调用存储引擎接口

6. 存储引擎：数据的读写接口，索引、表都在此层实现



### 6. undo log 与 redo log

**要求**

* 理解 undo log 的作用
* 理解 redo log 的作用

**undo log**

* 回滚数据，以行为单位，记录数据每次的变更，一行记录有多个版本并存
* 多版本并发控制，即快照读（也称为一致性读），让查询操作可以去访问历史版本

![image-20210902083051903](./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210902083051903.png)

1. 每个事务会按照开始时间，分配一个单调递增的事务编号 trx id
2. 每次事务的改动都会以行为单位记入回滚日志，包括当时的事务编号，改动的值等
3. 查询操作，事务编号大于自己的数据是不可见的，事务编号小于等于自己的数据才是可见的
   * 例如图中红色事务看不到 trx id=102 以及 trx id=101 的数据，只有 trx id=99 的数据对它可见



**redo log**

redo log 的作用主要是实现 ACID 中的持久性，保证提交的数据不丢失

* 它记录了事务提交的变更操作，服务器意外宕机重启时，利用 redo log 进行回放，重新执行已提交的变更操作
* 事务提交时，首先将变更写入 redo log，事务就视为成功。至于数据页（表、索引）上的变更，可以放在后面慢慢做
  * 数据页上的变更宕机丢失也没事，因为 redo log 里已经记录了
  * 数据页在磁盘上位置随机，写入速度慢，redo log 的写入是顺序的速度快

它由两部分组成，内存中的 redo log buffer，磁盘上的 redo log file

* redo log file 由一组文件组成，当写满了会循环覆盖较旧的日志，这意味着不能无限依赖 redo log，更早的数据恢复需要 binlog 
* buffer 和 file 两部分组成意味着，写入了文件才真正安全，同步策略由参数 innodb_flush_log_at_trx_commit  控制
  * 0 - 每隔 1s 将日志 write and flush 到磁盘 
  * 1 - 每次事务提交将日志 write and flush（默认值）
  * 2 - 每次事务提交将日志 write，每隔 1s flush 到磁盘，意味着 write 意味着写入操作系统缓存，如果 MySQL 挂了，而操作系统没挂，那么数据不会丢失



### 7. 锁

**要求**

* 了解全局锁
* 了解表级锁
* 掌握行级锁



**全局锁**

用作全量备份时，保证**表与表之间的数据一致性**

如果不加任何包含，数据备份时就可能产生不一致的情况，如下图所示

![image-20210902090302805](./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210902090302805.png)

全局锁的语法：

```sql
flush tables with read lock;	
```

* 使用全局读锁锁定所有数据库的所有表。这时会阻塞其它所有 DML 以及 DDL 操作，这样可以避免备份过程中的数据不一致。接下来可以执行备份，最后用 unlock tables 来解锁

> ***注意***
>
> 但 flush tables 属于比较重的操作，可以使用 --single-transaction 参数来完成不加锁的一致性备份（仅针对 InnoDB 引擎的表）
>
> ```sql
> mysqldump --single-transaction -uroot -p test > 1.sql
> ```



**表级锁 - 表锁**

* 语法：加锁 lock tables 表名 read/write，解锁 unlock tables
* 缺点：粒度较粗，在 InnoDB 引擎很少使用



**表级锁 - 元数据锁**

* 即 metadata-lock（MDL），主要是为**了避免 DML 与 DDL 冲突**，DML 的元数据锁之间不互斥

* 加元数据锁的几种情况
  * `lock tables read/write`，类型为 SHARED_READ_ONLY 和 SHARED_NO_READ_WRITE
  * `alter table`，类型为 EXCLUSIVE，与其它 MDL 都互斥
  * `select，select … lock in share mode`，类型为 SHARED_READ
  * `insert，update，delete，select for update`，类型为 SHARED_WRITE 

* 查看元数据锁（适用于 MySQL 8.0 以上版本）
  * `select object_type,object_schema,object_name,lock_type,lock_duration from performance_schema.metadata_locks;`



**表级锁 - IS（意向共享） 与 IX（意向排他）**

* 主要是**避免 DML 与表锁冲突**，DML 主要目的是加行锁，为了让表锁不用检查每行数据是否加锁，加意向锁（表级）来减少表锁的判断，意向锁之间不会互斥
* 加意向表锁的几种情况
  * `select  … lock in share mode` 会加 IS 锁
  * `insert，update，delete， select … for update` 会加 IX 锁
* 查看意向表锁（适用于 MySQL 8.0 以上版本）
  * `select object_schema,object_name,index_name,lock_type,lock_mode,lock_data from performance_schema.data_locks;`



**行级锁**

* 种类
  * 行锁 – 在 RC 下，锁住的是行，防止其他事务对此行 update 或 delete
  * 间隙锁 – 在 RR 下，锁住的是间隙，防止其他事务在这个间隙 insert 产生幻读
  * 临键锁 – 在 RR 下，锁住的是前面间隙+行，特定条件下可优化为行锁

* 查看行级锁
  * `select object_schema,object_name,index_name,lock_type,lock_mode,lock_data from performance_schema.data_locks where object_name='表名';`

> ***注意***
>
> * 它们锁定的其实都是**索引**上的行与间隙，根据索引的有序性来确定间隙



测试数据

```sql
create table t (id int primary key, name varchar(10),age int, key (name)); 
insert into t values(1, 'zhangsan',18); 
insert into t values(2, 'lisi',20); 
insert into t values(3, 'wangwu',21); 
insert into t values(4, 'zhangsan', 17); 
insert into t values(8,'zhang',18);
insert into t values(12,'zhang',20);
```

> **说明**
>
> * 1,2,3,4 之间其实并不可能有间隙
> * 4 与 8 之间有间隙
> * 8 与 12 之间有间隙
> * 12 与正无穷大之间有间隙
> * 其实我们的例子中还有负无穷大与 1 之间的间隙，想避免负数可以通过建表时选择数据类型为 unsigned int



间隙锁例子

事务1：

```sql
begin;
select * from t where id = 9 for update; /* 锁住的是 8 与 12 之间的间隙 */
```

事务2：

```sql
update t set age=100 where id = 8; /* 不会阻塞 */
update t set age=100 where id = 12; /* 不会阻塞 */
insert into t values(10,'aaa',18); /* 会阻塞 */
```



临键锁和记录锁例子

事务1：

```sql
begin;
select * from t where id >= 8 for update;
```

* 临键锁锁定的是左开右闭的区间，与上条查询条件相关的区间有 (4,8]，(8,12]，(12,+∞)
* 临键锁在某些条件下可以被优化为记录锁，例如 (4,8] 被优化为只针对 8 的记录锁，前面的区间不会锁住

事务2：

```sql
insert into t values(7,'aaa',18); /* 不会阻塞 */
update t set age=100 where id = 8; /* 会阻塞 */
insert into t values(10,'aaa',18); /* 会阻塞 */
update t set age=100 where id = 12; /* 会阻塞 */
insert into t values(13,'aaa',18); /* 会阻塞 */
```

+++

## 缓存篇

### 1. Redis 数据类型

**要求**

* 掌握常见数据类型的底层结构

**概述**

数据类型实际描述的是 value 的类型，key 都是 string，常见数据类型（value）有

1. string（embstr、raw、int）
2. list（quicklist，由多个 ziplist 双向链表组成）
3. hash（ziplist、hashtable）
4. set（intset、hashtable）
5. sorted set（ziplist、skiplist）
6. bitmap
7. hyperloglog

每一种类型都用 redisObject 结构体来表示，每种类型根据情况不同，有不同的编码 encoding（即底层数据结构）



**String**

1. 如果字符串保存的是整数值，则底层编码为 **int**，实际使用 long 来存储

2. 如果字符串保存的是非整数值（浮点数字或其它字符）又分两种情况
   1. 长度 <= 39 字节，使用 **embstr** 编码来保存，即将 redisObject 和 sdshdr 结构体保存在一起，分配内存只需一次
   2. 长度 > 39 字节，使用 **raw** 编码来保存，即 redisObject 结构体分配一次内存，sdshdr 结构体分配一次内存，用指针相连
3. sdshdr 称为**简单动态字符串**，实现上有点类似于 java 中的 StringBuilder，有如下特性
   1. 单独存储字符长度，相比 char* 获取长度效率高（char* 是 C 语言原生字符串表示）
   2. 支持动态扩容，方便字符串拼接操作
   3. 预留空间，减少内存分配、释放次数（< 1M 时容量是字符串实际长度 2 倍，>= 1M 时容量是原有容量 + 1M）
   4. 二进制安全，例如传统 char* 以 \0 作为结束字符，这样就不能保存视频、图片等二进制数据，而 sds 以长度来进行读取

**List**

1. 3.2 开始，Redis 采用 quicklist 作为其编码方式，它是一个双向链表，节点元素是 ziplist
   1. 由于是链表，**内存上不连续**
   2. **操作头尾效率高**，时间复杂度 O(1)
   3. 链表中 ziplist 的大小和元素个数都可以设置，其中大小默认 8kb

2. ziplist 用一块**连续的内存**存储数据，设计目标是让数据存储更紧凑，减少碎片开销，节约内存，它的结构如下 
   1. zlbytes – 记录整个 ziplist 占用字节数
   2. zltail-offset – 记录尾节点偏移量
   3. zllength – 记录节点数量
   4. entry – 节点，1 ~ N 个，每个 entry 记录了前一 entry 长度，本 entry 的编码、长度、实际数据，为了节省内存，根据**实际数据长度不同，用于记录长度的字节数也不同**，例如前一 entry 长度是 253 时，需要用 1 个字节，但超过了 253，需要用 5 个字节
   5. zlend – 结束标记

3. ziplist **适合存储少量元素，否则查询效率不高，并且长度可变的设计会带来连锁更新问题**

**Hash**

1. 在数据量较小时，采用 ziplist 作为其编码，当键或值长度过大（64）或个数过多（512）时，转为 hashtable 编码

2. hashtable 编码

   * hash 函数，Redis 5.0 采用了 SipHash 算法

   * 采用拉链法解决 key 冲突

   * **rehash 时机**

     ① 当元素数 < 1 * 桶个数时，不扩容

     ② 当元素数 > 5 * 桶个数时，一定扩容

     ③ 当 1 * 桶个数 <= 元素数 <= 5 * 桶个数时，如果此时没有进行 AOF 或 RDB 操作时

     ④ 当元素数 < 桶个数 / 10 时，缩容

   * **rehash 要点**

     ① 每个字典有两个哈希表，桶个数为 $2^n$，平时使用 ht[0]，ht[1] 开始为 null，扩容时新数组大小为元素个数 * 2

     ② **渐进式 rehash**，即不是一次将所有桶都迁移过去，每次对这张表 CRUD 仅迁移一个桶

     ③ **active rehash**，server 的主循环中，每 100 ms 里留出 1s 进行主动迁移

     ④ rehash 过程中，新增操作 ht[1] ，其它操作先操作 ht[0]，若没有，再操作 ht[1]

     ⑤ redis 所有 CRUD 都是单线程，因此 rehash 一定是线程安全的

**Sorted Set**

1. 在数据量较小时，采用 ziplist 作为其编码，按 score 有序，当键或值长度过大（64）或个数过多（128）时，转为 skiplist + hashtable 编码，同时采用的理由是
   * 只用 hashtable，CRUD 是 O(1)，但要执行有序操作，需要排序，带来额外时间空间复杂度
   * 只用 skiplist，虽然范围操作优点保留，但时间复杂度上升
   * 虽然同时采用了两种结构，但由于采用了指针，元素并不会占用双份内存

2. skiplist 要点：多层链表、排序规则、 backward、level（span，forward）

![image-20210902094835557](./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210902094835557.png)

* score 存储分数、member 存储数据、按 score 排序，如果 score 相同再按 member 排序
* backward 存储上一个节点指针
* 每个节点中会存储层级信息（level），同一个节点可能会有多层，每个 level 有属性：
  * foward 同层中下一个节点指针
  * span 跨度，用于计算排名，不是所有跳表都实现了跨度，Redis 实现特有

3. 多层链表可以加速查询，规则为，从顶层开始

   1. 大于同层右边的，继续在同层向右找

   2. 相等找到了

   3. 小于同层右边的或右边为 NULL，下一层，重复 1、2 步骤

![image-20210902094835557](./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210902094835557.png)

* 以查找【崔八】为例
  1. 从顶层（4）层向右找到【王五】节点，22 > 7 继续向右找，但右侧是 NULL，下一层
  2. 在【王五】节点的第 3 层向右找到【孙二】节点，22 < 37，下一层
  3. 在【王五】节点的第 2 层向右找到【赵六】节点，22 > 19，继续向右找到【孙二】节点，22 < 37，下一层
  4. 在【赵六】节点的第 1 层向右找到【崔八】节点，22 = 22，返回

> ***注意***
>
> * 数据量较小时，不能体现跳表的性能提升，跳表查询的时间复杂度是 $log_2(N)$，与二叉树性能相当



### 2. keys 命令问题

**要求**

* 理解低效命令对单线程的 Redis 影响

**问题描述**

* redis有一亿个 key，使用 keys 命令是否会影响线上服务？

**解答**

* keys 命令时间复杂度是 $O(n)$，n 即总的 key 数量，n 如果很大，性能非常低
* redis 执行命令是单线程执行，一个命令执行太慢会阻塞其它命令，阻塞时间长甚至会让 redis 发生故障切换

**改进方案**

* 可以使用 scan 命令替换 keys 命令，语法 `scan 起始游标 match 匹配规则 count 提示数目`，返回值代表下次的起点
  1. 虽然 scan 命令的时间复杂度仍是 $O(n)$，但它是通过游标分步执行，不会导致长时间阻塞
  2. 可以用 count 参数**提示**返回 key 的个数
  3. 弱状态，客户端仅需维护游标
  4. scan 能保证在 rehash 也正常工作
  5. 缺点是可能会重复遍历 key（缩容时）、应用应自己处理重复 key



### 3. 过期 key 的删除策略

**要求**

* 了解 Redis 如何记录 key 的过期时间
* 掌握 Redis 对过期 key 的删除策略

**记录 key 过期时间**

* 每个库中都包含了 expires 过期字典
  * hashtable结构，键为指针，指向真正 key，值为 long 类型的时间戳，毫秒精度
* 当设置某个 key 有过期时间时，就会向过期字典中添加此 key 的指针和时间戳

**过期 key 删除策略**

* 惰性删除

  * 在执行读写数据库的命令时，执行命令前会检查 key 是否过期，如果已过期，则删除 key

* 定期删除

  * redis 有一个定时任务处理器 serverCron，负责周期性任务处理，默认 100 ms 执行一次（hz 参数控制）包括：① 处理过期 key、② hash 表 rehash、③ 更新统计结果、④ 持久化、⑤ 清理过期客户端

  * 对于处理过期 key 会：依次遍历库，在规定时间内运行如下操作

    ① 从每个库的 expires 过期字典中随机选择 20 个 key 检查，如果过期则删除

    ② 如果删除达到 5 个，重复 ① 步骤，没有达到，遍历至下一个库

    ③ 规定时间没有做完，等待下一轮 serverCron 运行



### 4. Redis 持久化

**要求**

* 掌握 AOF 持久化和 AOF 重写
* 掌握 RDB 持久化
* 了解混合持久化



**AOF 持久化**

* AOF - 将每条**写命令追加至 aof 文件**，当重启时会执行 aof 文件中每条命令来重建内存数据
* AOF 日志是**写后日志**，即先执行命令，再记录日志
  * Redis 为了性能，向 aof 记录日志时没有对命令进行语法检查，如果要先记录日志，那么日志里就会记录语法错误的命令
* 记录 AOF 日志时，有**三种同步策略**
  * Always 同步写，日志写入磁盘再返回，可以做到基本不丢数据，性能不高
    * 为什么说基本不丢呢，因为 aof 是在 serverCron 事件循环中执行 aof 写入的，并且这次写入的是上一次循环暂存在 aof 缓冲中的数据，因此最多还是可能丢失一个循环的数据
  * Everysec 每秒写，日志写入 AOF 文件的内存缓冲区，每隔一秒将内存缓冲区数据刷入磁盘，最多丢一秒的数据
  * No 操作系统写，日志写入AOF 文件的内存缓冲区，由操作系统决定何时将数据刷入磁盘

**AOF 重写**

* AOF 文件太大引起的问题
  1. 文件大小受操作系统限制
  2. 文件太大，写入效率变低
  3. 文件太大，**恢复时非常慢**
* 重写就是对同一个 key 的多次操作进行瘦身
  1. 例如一个 key 我改了 100 遍，aof 里记录了100 条修改日志，但实际上只有最后一次有效
  2. 重写无需操作现有 aof 日志，只需要根据当前内存数据的状态，生成相应的命令，记入一个新的日志文件即可
  3. 重写过程是由另一个后台子进程完成的，不会阻塞主进程
* **AOF 重写过程**
  1. 创建子进程时会根据主进程生成内存快照，只需要对子进程的内存进行遍历，把每个 key 对应的命令写入新的日志文件（即重写日志）
  2. 此时如果有新的命令执行，修改的是主进程内存，不会影响子进程内存，并且新命令会记录到 `重写缓冲区`  
  3. 等子进程所有的 key 处理完毕，再将 `重写缓冲区` 记录的增量指令写入重写日志 
  4. 在此期间旧的 AOF 日志仍然在工作，待到重写完毕，用重写日志替换掉旧的 AOF 日志

**RDB 持久化**

* RDB - 是把整个内存数据以二进制方式写入磁盘
  * 对应数据文件为 `dump.rdb`
  * 好处是恢复速度快
* 相关命令有两个
  * save - 在主进程执行，会阻塞其它命令
  * bgsave - 创建子进程执行，避免阻塞，是默认方式
    * 子进程不会阻塞主进程，但创建子进程的期间，仍会阻塞，内存越大，阻塞时间越长
    * bgsave 也是利用了快照机制，执行 RDB 持久化期间如果有新数据写入，新的数据修改发生在主进程，子进程向 RDB 文件中写入还是旧的数据，这样新的修改不会影响到 RDB 操作
    * 但这些**新数据不会补充**至 RDB 文件
* 缺点： 可以通过调整 redis.conf 中的 save 参数来控制 rdb 的执行周期，但这个周期不好把握
  * 频繁执行的话，会影响性能
  * 偶尔执行的话，如果宕机又容易丢失较多数据

**混合持久化**

* 从 4.0 开始，Redis 支持混合持久化，即使用 RDB 作为全量备份，两次 RDB 之间使用 AOF 作为增量备份
  * 配置项 aof-use-rdb-preamble 用来控制是否启用混合持久化，默认值 no
  * 持久化时将数据都存入 AOF 日志，日志前半部分为二进制的 RDB 格式，后半部分是 AOF 命令日志
  * 下一次 RDB 时，会覆盖之前的日志文件
* 优缺点
  * 结合了 RDB 与 AOF 的优点，恢复速度快，增量用 AOF 表示，数据更完整（取决于同步策略）、也无需 AOF 重写
  * 与旧版本的 redis 文件格式不兼容



### 5. 缓存问题

**要求**

* 掌握缓存击穿
* 掌握缓存雪崩
* 掌握缓存穿透
* 掌握旁路缓存与缓存一致性

**缓存击穿**

* 缓存击穿是指：某一热点 key 在缓存和数据库中都存在，它过期时，这时由于并发用户特别多，同时读缓存没读到，又同时去数据库去读，压垮数据库

* 解决方法

  1. 热点数据不过期
  2. 对【查询缓存没有，查询数据库，结果放入缓存】这三步进行加锁，这时只有一个客户端能获得锁，其它客户端会被阻塞，等锁释放开，缓存已有了数据，其它客户端就不必访问数据库了。但会影响吞吐量（有损方案）

  ![image-20210902105842482](./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210902105842482.png)

**缓存雪崩**

* 情况1：由于大量 key 设置了相同的过期时间（数据在缓存和数据库都存在），一旦到达过期时间点，这些 key 集体失效，造成访问这些 key 的请求全部进入数据库。

* 解决方法：

  1. 错开过期时间：在过期时间上加上随机值（比如 1~5 分钟）

  2. 服务降级：暂停非核心数据查询缓存，返回预定义信息（错误页面，空值等）

* 情况2：Redis 实例宕机，大量请求进入数据库

* 解决方法：

  1. 事前预防：搭建高可用集群

  2. 多级缓存：缺点是实现复杂度高

  3. 熔断：通过监控一旦雪崩出现，暂停缓存访问待实例恢复，返回预定义信息（有损方案）

  4. 限流：通过监控一旦发现数据库访问量超过阈值，限制访问数据库的请求数（有损方案）

**缓存穿透**

* 缓存穿透是指：如果一个 key 在缓存和数据库都不存在，那么访问这个 key 每次都会进入数据库

  * 很可能被恶意请求利用
  * 缓存雪崩与缓存击穿都是数据库中有，但缓存暂时缺失
  * 缓存雪崩与缓存击穿都能自然恢复，但缓存穿透则不能

* 解决方法

  1. 如果数据库没有，也将此不存在的 key 关联 null 值放入缓存，缺点是这样的 key 没有任何业务作用，白占空间

  2. 布隆过滤器

  ![image-20210902110302668](./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210902110302668.png)

  ① 过滤器可以用来判定 key 不存在，发现这些不存在的 key，把它们过滤掉就好

  ② 需要将所有的 key 都预先加载至布隆过滤器

  ③ 布隆过滤器不能删除，因此查询删除的数据一定会发生穿透

**旁路缓存**

* 旁路缓存（Cache Aside），是一种常见的使用缓存的策略
* 查询规则
  * 先读缓存
  * 如果命中，直接返回
  * 如果缺失，查 DB 并将结果放入缓存，再返回
* 增、删、改规则
  * 新增数据，直接存 DB
  * 修改、删除数据，先更新DB，再删缓存

> ***为什么要先操作库，再操作缓存？***
>
> * 假设操作库和缓存均能成功，如果先操作缓存，会大几率出现数据库与缓存不一致的情况

**一致性分析 - 先清缓存，再更新库**

<img src="./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210902111258034.png" alt="image-20210902111258034" style="zoom:80%;" />

**一致性分析 - 先更新库，再清缓存**

<img src="./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210902111337037.png" alt="image-20210902111337037" style="zoom:80%;" />

* 会有短暂不一致，但最终会一致

<img src="./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210902111414597.png" alt="image-20210902111414597" style="zoom:80%;" />

* 假设查询线程 A 查询数据时恰好缓存数据由于时间到期失效，或是第一次查询，会如上图所示出现不一致
* 但这种几率出现机会很小

**用锁解决一致性**

<img src="./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210902111615554.png" alt="image-20210902111615554" style="zoom:80%;" />

* 缺点：影响吞吐量、分布式锁设计较为复杂



### 6. 缓存原子性

**要求**

* 掌握 Redis 事务的局限性
* 理解用乐观锁保证原子性
* 理解用 lua 脚本保证原子性

**Redis 事务局限性**

* 单条命令是原子性，这是由 redis 单线程保障的
* 多条命令能否用 `multi + exec` 来保证其原子性呢？

Redis 中 `multi + exec` 并不支持回滚，例如有初始数据如下

```cmd
set a 1000
set b 1000
set c a
```

执行

```cmd
multi
decr a
incr b
incr c
exec
```

执行 `incr c` 时，由于字符串不支持自增导致此条命令失败，但之前的两条命令并不会回滚

更为重要的是，`multi + exec` 中的**读操作没有意义**，因为读的结果并不能赋值给临时变量，用于后续的写操作，既然 `multi + exec` 中读没有意义，就**无法保证读 + 写的原子性**，例如有初始数据如下

```cmd
set a 1000
set b 1000
```

假设 a 和 b 代表的是两个账户余额，现在获取旧值，执行转账 500 的操作：

```cmd
get a /* 存入客户端临时变量 */
get b /* 存入客户端临时变量 */
/* 客户端计算出 a 和 b 更新后的值 */
multi
set a 500
set b 1500
exec
```

但如果在 get 与 multi 之间其它客户端修改了 a 或 b，会造成丢失更新



**乐观锁保证原子性**

watch 命令，用来盯住 key（一到多个），如果这些 key 在事务期间：

* 没有被别的客户端修改，则 exec 才会成功

* 被别的客户端改了，则 exec 返回 nil

还是上一个例子

```cmd
get a /* 存入客户端临时变量 */
get b /* 存入客户端临时变量 */
/* 客户端计算出 a 和 b 更新后的值 */
watch a b /* 盯住 a 和 b */
multi
set a 500
set b 1500
exec
```

此时，如果其他客户端修改了 a 和 b 的值，那么 exec 就会返回 nil，并不会执行两条 set 命令，此时客户端可以进行重试



**lua 脚本保证原子性**

Redis 支持 lua 脚本，能保证 lua 脚本执行的原子性，可以取代 `multi + exec`

例如要解决上面的问题，可以执行如下命令

```cmd
eval "local a = tonumber(redis.call('GET',KEYS[1]));local b = tonumber(redis.call('GET',KEYS[2]));local c = tonumber(ARGV[1]); if(a >= c) then redis.call('SET', KEYS[1], a-c); redis.call('SET', KEYS[2], b+c); return 1;else return 0; end" 2 a b 500
```

* eval 用来执行 lua 脚本
* 2 表示后面用空格分隔的参数中，前两个是 key，剩下的是普通参数
* 脚本中可以用 keys[n] 来引用第 n 个 key，用 argv[n] 来引用第 n 个普通参数
* 其中双引号内部的即为 lua 脚本，格式化如下

```lua
local a = tonumber(redis.call('GET',KEYS[1]));
local b = tonumber(redis.call('GET',KEYS[2]));
local c = tonumber(ARGV[1]); 
if(a >= c) then 
    redis.call('SET', KEYS[1], a-c); 
    redis.call('SET', KEYS[2], b+c); 
    return 1;
else 
    return 0; 
end
```



### 7. LRU Cache 实现

**要求**

* 掌握基于链表的 LRU Cache 实现
* 了解 Redis 在 LRU Cache 实现上的变化

**LRU Cache 淘汰规则**

Least Recently Used，将最近最少使用的 key 从缓存中淘汰掉。以链表法为例，最近访问的 key 移动到链表头，不常访问的自然靠近链表尾，如果超过容量、个数限制，移除尾部的

* 例如有原始数据如下，容量规定为 3

  <img src="./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210902141534470.png" alt="image-20210902141534470" style="zoom:50%;" />

* 时间上，新的留下，老的淘汰，比如 put d，那么最老的 a 被淘汰

  <img src="./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210902141720278.png" alt="image-20210902141720278" style="zoom:50%;" />

* 如果访问了某个 key，则它就变成最新的，例如 get b，则 b 被移动到链表头

  <img src="./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210902141912068.png" alt="image-20210902141912068" style="zoom:50%;" />

**LRU Cache 链表实现**

* 如何断开节点链接

<img src="./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210902141247148.png" alt="image-20210902141247148" style="zoom:80%;" />

* 如何链入头节点

<img src="./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210902141320849.png" alt="image-20210902141320849" style="zoom:80%;" />

参考代码一

```java
package day06;

import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

public class LruCache1 {
    static class Node {
        Node prev;
        Node next;
        String key;
        Object value;

        public Node(String key, Object value) {
            this.key = key;
            this.value = value;
        }

        // (prev <- node -> next)
        public String toString() {
            StringBuilder sb = new StringBuilder(128);
            sb.append("(");
            sb.append(this.prev == null ? null : this.prev.key);
            sb.append("<-");
            sb.append(this.key);
            sb.append("->");
            sb.append(this.next == null ? null : this.next.key);
            sb.append(")");
            return sb.toString();
        }
    }

    public void unlink(Node node) {
        node.prev.next = node.next;
        node.next.prev = node.prev;
    }

    public void toHead(Node node) {
        node.prev = this.head;
        node.next = this.head.next;
        this.head.next.prev = node;
        this.head.next = node;
    }

    int limit;
    Node head;
    Node tail;
    Map<String, Node> map;
    public LruCache1(int limit) {
        this.limit = Math.max(limit, 2);
        this.head = new Node("Head", null);
        this.tail = new Node("Tail", null);
        head.next = tail;
        tail.prev = head;
        this.map = new HashMap<>();
    }

    public void remove(String key) {
        Node old = this.map.remove(key);
        unlink(old);
    }

    public Object get(String key) {
        Node node = this.map.get(key);
        if (node == null) {
            return null;
        }
        unlink(node);
        toHead(node);
        return node.value;
    }

    public void put(String key, Object value) {
        Node node = this.map.get(key);
        if (node == null) {
            node = new Node(key, value);
            this.map.put(key, node);
        } else {
            node.value = value;
            unlink(node);
        }
        toHead(node);
        if(map.size() > limit) {
            Node last = this.tail.prev;
            this.map.remove(last.key);
            unlink(last);
        }
    }

    @Override
    public String toString() {
        StringBuilder sb = new StringBuilder();
        sb.append(this.head);
        Node node = this.head;
        while ((node = node.next) != null) {
            sb.append(node);
        }
        return sb.toString();
    }

    public static void main(String[] args) {
        LruCache1 cache = new LruCache1(5);
        System.out.println(cache);
        cache.put("1", 1);
        System.out.println(cache);
        cache.put("2", 1);
        System.out.println(cache);
        cache.put("3", 1);
        System.out.println(cache);
        cache.put("4", 1);
        System.out.println(cache);
        cache.put("5", 1);
        System.out.println(cache);
        cache.put("6", 1);
        System.out.println(cache);
        cache.get("2");
        System.out.println(cache);
        cache.put("7", 1);
        System.out.println(cache);
    }

}
```



参考代码二

```java
package day06;

import java.util.LinkedHashMap;
import java.util.Map;

public class LruCache2 extends LinkedHashMap<String, Object> {

    private int limit;

    public LruCache2(int limit) {
        // 1 2 3 4 false
        // 1 3 4 2 true
        super(limit * 4 /3, 0.75f, true);
        this.limit = limit;
    }

    @Override
    protected boolean removeEldestEntry(Map.Entry<String, Object> eldest) {
        if (this.size() > this.limit) {
            return true;
        }
        return false;
    }

    public static void main(String[] args) {
        LruCache2 cache = new LruCache2(5);
        System.out.println(cache);
        cache.put("1", 1);
        System.out.println(cache);
        cache.put("2", 1);
        System.out.println(cache);
        cache.put("3", 1);
        System.out.println(cache);
        cache.put("4", 1);
        System.out.println(cache);
        cache.put("5", 1);
        System.out.println(cache);
        cache.put("6", 1);
        System.out.println(cache);
        cache.get("2");
        System.out.println(cache);
        cache.put("7", 1);
        System.out.println(cache);
    }
}
```



**Redis LRU Cache 实现**

Redis 采用了随机取样法，较之链表法占用内存更少，每次只抽 5 个 key，每个 key 记录了它们的**最近访问时间**，在这 5 个里挑出最老的移除

* 例如有原始数据如下，容量规定为 160，put 新 key a

  ![image-20210902142353705](./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210902142353705.png)

* 每个 key 记录了放入 LRU 时的时间，随机挑到的 5 个 key（16,78,90,133,156），会挑时间最老的移除（16）

* 再 put b 时，会使用上轮剩下的 4 个（78,90,133,156），外加一个随机的 key（125），这里面挑最老的（78）

  ![image-20210902142644518](./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210902142644518.png)

* 如果 get 了某个 key，它的访问时间会被更新（下图中 90）这样就避免了它下一轮被移除

  ![image-20210902143122234](./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210902143122234.png)

+++

## 分布式篇

### 1. CAP 定理

**要求**

* 理解 CAP 定理
* 知道常见的一致性级别

**CAP 定理**

* Consistency 一致性：访问分布式系统中任意节点，总能返回一致的结果
  * Every read receives the most recent write or an error
* Availability 可用性：分布式系统总能向客户端返回响应
  * Every request receives a (non-error) response, without the guarantee that it contains the most recent write
* Partition tolerance 分区容忍：当分布式系统节点间通信发生了消息丢失或消息延迟，仍然允许系统继续运行
  * The system continues to operate despite an arbitrary number of messages being dropped (or delayed) by the network between nodes

CAP 定理：最多三选二，无法兼得，通常在 CP 或者 AP 之间做出选择

**不一致的产生**

1. client 向 Node1 写入新值 v1

<img src="./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210902144323586.png" alt="image-20210902144323586" style="zoom: 67%;" />

2. 写入成功 Node1 更新成 v1

<img src="./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210902144332846.png" alt="image-20210902144332846" style="zoom:67%;" />

3. Node1 在没有将变更同步到 Node2 时，就向客户端返回了应答

<img src="./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210902144346469.png" alt="image-20210902144346469" style="zoom:67%;" />

4. client 发起向 Node2 的读操作

<img src="./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210902144357711.png" alt="image-20210902144357711" style="zoom:67%;" />

5. 返回了旧值 v0（不一致）的结果

<img src="./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210902144405372.png" alt="image-20210902144405372" style="zoom:67%;" />

**保证一致性**

1. client 向 Node1 写入新值 v1

<img src="./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210902144821346.png" alt="image-20210902144821346" style="zoom:67%;" />

2. 写入成功 Node1 更新成 v1，此时不能立刻向 client 返回应答，而是需要将 v1 同步到 Node2

<img src="./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210902144917135.png" alt="image-20210902144917135" style="zoom:67%;" />

3. 同步 v1 成功

<img src="./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210902145113734.png" alt="image-20210902145113734" style="zoom:67%;" />

4. 此时才能向 client 返回应答

<img src="./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210902145138926.png" alt="image-20210902145138926" style="zoom:67%;" />

5. 如果此时 client 再去访问 Node2，不会出现不一致的情况

**保 CP 失 A**

1. 当发生了网络分区，Node1 与 Node2 已经失去了联系，这时仍想对外提供服务（保 P）

<img src="./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210902145433075.png" alt="image-20210902145433075" style="zoom:67%;" />

2. client 向 Node1 写入新值 v1

<img src="./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210902145513466.png" alt="image-20210902145513466" style="zoom:67%;" />

3. 写入 Node1 成功，但无法同步至 Node2

<img src="./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210902145616905.png" alt="image-20210902145616905" style="zoom:67%;" />

4. 这时为了保证一致性，Node1 不能向 client 返回应答，造成操作挂起无法完成（失去可用性）

**保 AP 失 C**

1. 当发生了网络分区，Node1 与 Node2 已经失去了联系，这时仍想对外提供服务（保 P）

<img src="./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210902145433075.png" alt="image-20210902145433075" style="zoom:67%;" />

2. client 向 Node1 写入新值 v1

<img src="./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210902145513466.png" alt="image-20210902145513466" style="zoom:67%;" />

3. 写入 Node1 成功，但无法同步至 Node2

<img src="./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210902145616905.png" alt="image-20210902145616905" style="zoom:67%;" />

4. 为了保证可用性，向 client 返回了应答（但牺牲了一致性）

<img src="./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210902150437928.png" alt="image-20210902150437928" style="zoom:67%;" />

**一致性级别**

CP 和 AP 之间需要做权衡，其实根据需求不同，也可以将一致性划分成几个级别，在这些级别里做一个权衡。

* 强一致性：系统写入什么，读出来的也会是什么，但实现起来往往对性能影响较大，例如之前 CP 的例子
  * 例如：火车站售票，有就是有，没有就是没有，不能出现不一致的情况
  * 典型算法：Paxos、Raft、ZAB
* 弱一致性：系统写入成功后，不承诺立刻可以读到写入的值，也不承诺具体多久后数据能达到一致，还可以细分为：
  * 会话一致性，同一个客户端会话中可以保证一致，其它会话不能保证
  * 用户一致性，同一个用户中可以保证一致，其它用户不能保证
  * 例如：网上购物，在商品详情页看到库存量还有好多，下单的瞬间才被提示“库存量不足”，实际上商品详情页展示的库存并不是最新的数据，只是在某个流程上才会做准确的检查
* 最终一致性：是弱一致性的特例，保证在一定时间内，能够达到一个一致的状态
  * 例如：转账，转账完成后，会有一个提示，您的转账会在 24 小时内到账，一般用户也能接受，但最终必须是一致的
  * 典型协议：Gossip



### 2. Paxos 算法

**要求**

* 理解 Paxos 产生背景
* 掌握 Basic Paxos 算法

**问题提出**

1. 集群中有 N 个节点，如果一个节点写入后要求同步到剩余 N-1 个节点后再向客户端返回 ok，虽然看起来最保险，但其中任意一个节点同步失败，势必造成整个集群不可用，能否在此基础上稍微提高可用性呢？

<img src="./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210902150851559.png" alt="image-20210902150851559" style="zoom:67%;" />

2. 答案是 **（写）多数派**，集群节点设置为奇数，同步超过集群中 N/2 个节点成功，则向客户端返回 ok，但存在顺序性问题，如 3 描述
3. 多数派写操作成功后的读一致性暂不考虑，思考下图中的两项操作，都满足了多数派通过，但 S3 这台服务器并没有与 S1，S2 达成一致，要**达到多数派内部一致性**

![image-20210902151020777](./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210902151020777.png)



**Paxos**

Paxos 是一种共识算法，目的是解决之前提到的写多数派时的顺序性问题

Paxos 角色划分：集群中的每个节点都可以充当

1. **Proposer**：负责生成提案
   * 注意：Paxos 算法允许有多个 Proposer 同时提案，但可能会引起活锁问题

2. **Acceptor**：负责批准提案
   * Acceptor 如果只有一个的话，存在单点问题，因此应当有多个

3. Learner：负责获取提案，Acceptor 批准提案后，会将提案发送给所有 Learner

执行一个修改操作，不是一上来就能执行，分成两个阶段：

1. 准备阶段：Proposer负责接收 client 请求并产生提案，必须由多数派 Acceptor 批准通过提案
2. 接受阶段：提案通过后，再将要执行的修改操作广播给 Acceptor，这次仍然多数派通过，此修改才能生效，可以返回响应给客户端

![image-20210902151606775](./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210902151606775.png)

算法要点：

* 整个算法分成两个阶段：预备阶段，前两个箭头，接受阶段，后两个箭头。
  * 预备阶段的目的是：第一拦截掉旧的提案，第二找到最新的 acceptValue
* 对于 Proposer
  * 预备阶段只发送`提案号`，接受阶段发送`提案号 + 值`
  * `提案号` n 唯一且全局递增，大的`提案号`有更高优先级
  * 如果见到最新`已接受值`，就会替换掉 Proposer 自己原来的值，保证一致性
* 对于 Acceptor 会持久化以下信息
  * minN（最小提案号），会在预备阶段和接受阶段被更新为更大提案号，会用来决定 Proposer 是否能选中提案
  * acceptN（已接受提案号）和 acceptValue（已接受值），会在接受阶段被更新，如果 minN > n 则不会更新



**例1**

![image-20210902152006734](./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210902152006734.png)

1. P 广播提案号 1

2. 有 3 个 A 接到提案，此时满足 n > minN，将 minN 更新为 1

3. 3个 A 成功返回，P 收到的应答过半，但没有遇到更大的 acceptNo 和 acceptValue，因此使用自己的 value X

4. P 广播提案号和值 1:X

5. 3 个 A 接到提案号和值，更新状态，返回 minN 值 1 给 P

6. P 收到过半应答，并检查发现没有出现 minN > 1，便选中提案值 X



**例2**

![image-20210902152121752](./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210902152121752.png)

1. S1 广播提案号 1，想把值更新为 X

2. S5 广播提案号 2，想把值更新为 Y

3. S1、S2、S3 已经经历了 Accept 阶段并选中值 X

4. **关键点**，S3 也接到了 S5 的prepare 提案，这时是否会有不一致的情况呢？

5. 此时 S3 状态已将 acceptN 和 acceptValue 分别更新为 1:X；再返回 S5 的 ack 时就会将 1:X 返回给 S5

6. S5 用返回的 X 替换掉了自己原有的值 Y，并执行后续流程，后续都会同步为 X



**例3**

![image-20210902152345674](./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210902152345674.png)

1. S1 广播提案号 1，想把值更新为 X

2. S5 广播提案号 2，想把值更新为 Y

3. S1、S2、S3 已经经历了 Accept 阶段，**与例2 不同的是，值 X 还未选中**

4. **关键点**，S3 也接到了 S5 的prepare 提案，这时是否会有不一致的情况呢？

5. 此时 S3 状态将 acceptN 和 acceptValue 分别更新为 1:X；再返回 S5 的 ack 时就会将 1:X 返回给 S5

6. S5 用返回的 X 替换掉了自己原有的值 Y，并执行后续流程，后续都会同步为 X



**例4**

![image-20210902152544031](./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210902152544031.png)

1. S1 广播提案号 1，想把值更新为 X

2. S5 广播提案号 2，想把值更新为 Y

3. **关键点**，S3 还未经历 Accept 阶段时，就拿到了 S5 的 prepare 提案，这时是否会有不一致的情况呢？

4. S3 在接到 S1 的 accept 请求时，n>=minN 条件不成立，因此没有更新 acceptN 和 acceptValue，并且返回的 minN 是 2

5. 对 S1 来说，S3 返回的 minN 大于 n，选中失败；想更新 X 需要发起新一轮提案

6. 对 S5 来说，accept 阶段发送的是它自己的 2:Y，后续会把值同步为 Y



**例5**

回顾最早提到的顺序性问题，看 Paxos 能否解决它

![image-20210902152742816](./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210902152742816.png)

下图演示了 Paxos 是如何解决顺序性问题的，分析步骤参考例3

![image-20210902152753028](./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210902152753028.png)



**Paxos 缺点**

1. 效率较低，两轮操作只能选中一个值

2. 难于理解

3. 活锁问题

![image-20210902153136877](./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210902153136877.png)

* Paxos 是允许多个 Proposer 的，因此如果按上图所示运行，则后一个提案总会让前面提案选中失败，显然死循环



> ***参考资料***
>
> * https://www.youtube.com/watch?v=JEpsBg0AO6o&t=41s Raft 作者讲解 Paxos



### 3. Raft 算法

**要求**

* 理解 Raft 算法

**Raft 算法**

另一种共识算法，目的是比 Paxos 更易理解

整个 Raft 算法分解为三部分：

1. Leader 选举

   ① 只有一个 Server 能作为 Leader

   ② 一旦此 Server 崩溃，选举新 Leader

2. 执行操作，以日志复制为例（Log replication）

   ① 由 Leader 执行自己的日志记录

   ② 将日志复制到其它 Server，会覆盖掉不一致的部分

   ③ 多数派记录日志成功，Leader 才能执行命令，向客户端返回结果

3. 确保安全

   ① 保证日志记录的一致性

   ② 拥有最新日志的 Server 才能成为 Leader



**Leader 选举**

1. **Leader** 会不断向**选民**发送 AppendEntries 请求，证明自己活着

2. **选民**收到 **Leader** AppendEntries 请求后会重置自己的 timeout 时间

<img src="./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210902154049390.png" alt="image-20210902154049390" style="zoom:80%;" />

3. **选民**收不到 AppendEntries 请求超时后，转换角色为**候选者**，并将**任期**加1，发送 RequestVote 请求，推选自己

<img src="./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210902154114306.png" alt="image-20210902154114306" style="zoom:80%;" />

4. **选民**收到第一个 RequestVote，会向该**候选者**投一票，这样即使有多个**候选者**，必定会选出一个 **Leader**，选票过半即当选，如果落选会变回**选民**

<img src="./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210902154133262.png" alt="image-20210902154133262" style="zoom:80%;" />

5. 每一**任期**最多有一个 **Leader**，但也可能没有（选票都不过半的情况，需要再进行一轮投票，timeout 在 T~2T 间随机）

6. **任期**由各个 server 自己维护即可，无需全局维护，在超时后加1，在接收到任意消息时更新为更新的**任期**，遇到更旧的**任期**，视为错误



**执行操作（以日志复制为例）**

1. **客户端**发送命令至 **Leader**

2. **Leader** 将命令写入日志（S1虚框），并向所有**选民**发送 AppendEntries 请求

<img src="./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210902154733468.png" alt="image-20210902154733468" style="zoom:80%;" />

3. 多数派通过后，执行命令（即提交，S1虚框变实），此时就可以向**客户端**返回结果

<img src="./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210902154843978.png" alt="image-20210902154843978" style="zoom:80%;" />

4. 在后续的 AppendEntries 请求中通知**选民**，**选民**执行命令（即提交，S2,S3,S4,S5虚框变实）

<img src="./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210902155110401.png" alt="image-20210902155110401" style="zoom:80%;" />

5. 如果**选民**挂了，则 **Leader** 会不断尝试，待到**选民**重启，会将其缺失的日志陆续补齐



**确保安全**

Leader 日志的完整性

1. Leader 被认为拥有最完整的日志

2. 一旦 Leader 完成了某条命令提交，那么未来的 Leader 也必须存有该条命令提交信息

3. 投票时，会将候选者最新的 `<Term，Index>` 随 RequestVote 请求发送，如果候选者的日志还没选民的新，则投否决票

<img src="./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210902155344700.png" alt="image-20210902155344700" style="zoom:80%;" />

* 图中 S2 如果超时，发起选举请求，其它服务器只会对它投否决票，因为它的 Index 比其它人都旧
* 图中 S5 如果超时，发起选举请求，其它服务器也不会选它，因为他的 Term 太旧

选民日志的一致性

1. 以 Leader 为准，对选民的日志进行补充或覆盖

2. AppendEntries 请求发送时会携带最新的 `<Term,Index,Command>` 以及上一个的 `<Term,Index>`

3. 如果选民发现上一个的 `<Term,Index>` 能够对应上则成功，否则失败，继续携带更早的信息进行比对

<img src="./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210902155724436.png" alt="image-20210902155724436" style="zoom:80%;" />

* 图中 Leader 发送了 `<3,4,Command>` 和 `<2,3>` 给 follower，follower 发现 `<2,3>` 能够与当前最新日志对应，这时直接执行 `<3,4,Command>` 即可

<img src="./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210902155837273.png" alt="image-20210902155837273" style="zoom:80%;" />

* 图中 Leader 发送了 `<3,4,Command>` 和 `<2,3>` 给 follower，follower 发现 `<2,3>` 不能与当前最新日志对应，会央求 Leader 发送更早日志

<img src="./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210902160222512.png" alt="image-20210902160222512" style="zoom:150%;" />

* Leader 这次发送了 `<3,4,Command>` ， `<2,3,Command>` ，`<1,2>` 给 follower，follower 发现 `<1,2>` 能够与当前最新日志对应，这时补全 `<3,4,Command>` ， `<2,3,Command>`  即可



> ***参考资料***
>
> * https://www.youtube.com/watch?v=vYp4LYbnnW8 Raft 作者讲解 Raft
> * https://raft.github.io/ Raft 资源
> * https://raft.github.io/raftscope/index.html Raft 可视化



### 4. Gossip 协议

**要求**

* 掌握 Gossip 协议

**Gossip 协议**

与 Paxos 和 Raft 目标是强一致性不同，Gossip 达到的是最终一致性

* A gossip protocol is a procedure or process of computer peer-to-peer communication that is based on the way epidemics spread.

它可以快速地将信息散播给集群中每个成员，散播速度为 $𝑙𝑜𝑔_𝑓 (𝑁)$ ，其中 f 术语称为 fanout，代表每次随机传播的成员数，而 N 代表总共成员数。例如：

* $𝑙𝑜𝑔_4 (40)≈2.66$ ，也就是大约三轮传播，就可以让集群达到一致
* 实际传播次数可能会高于此结果，因为随机时会随到一些重复的成员



**Gossip 协议工作流程**

1. 例如，图中红色节点有其它节点不知道的信息，它的传播方式如下

<img src="./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210902161814969.png" alt="image-20210902161814969" style="zoom: 100%;" />

2. 在红色节点能连通的节点中随机挑选 fanout 个（粗线所示）

<img src="./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210902161917080.png" alt="image-20210902161917080" style="zoom: 50%;" />

<img src="./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210902162015054.png" alt="image-20210902162015054" style="zoom:50%;" />

3. 把信息传播给它们（感染）

<img src="./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210902162053851.png" alt="image-20210902162053851" style="zoom:50%;" />

4. 在这些已被【感染】的节点中，重复 2. 3. 两步，直至全部感染，即达到最终一致性

<img src="./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210902162400932.png" alt="image-20210902162400932" style="zoom:50%;" />



**Gossip 协议优点**

* 扩展性高，传播次数不会受集群成员增长而增长过快
  * 例如： $𝑙𝑜𝑔_4 (80)≈3.16$ ，集群实际成员翻了一倍，但传播次数几乎不变
* 容错性好，即使某些节点间发生了故障无法通信，也不会影响最终的一致性
  * 例如：A 与 B 之间发生故障无法通信，但只要 A 与其他能连通 B 的节点通信，那么信息就一定会散播到 B 
* Robust（鲁棒性），即皮实，集群中的节点是对等的，即便一些节点挂了，一些节点新添加进来，也不会影响其它节点的信息传播



> ***参考资料***
>
> * [https://flopezluis.github.io/gossip-simulator/#](https://flopezluis.github.io/gossip-simulator/) Gossip 可视化



### 5. 分布式通用设计

**要求**

* 掌握检测节点活着
* 掌握实现高可用
* 掌握全局 ID 生成
* 掌握负载均衡策略
* 掌握数据分片策略
* 掌握分布式事务

> ***提示***
>
> * 这里介绍以思想为主，因为实现和涉及的框架实在是太多了



**如何检测节点活着**

答案：通过心跳

* 向节点周期性发送心跳请求，如果能收到心跳回应，表示该节点还活着
* 但如果收不到心跳回应，却不能证明该节点死了，可能由于网络抖动、回应延时等原因没能及时收到回应。有如下解决思路：
  1. 如 Redis 哨兵模式中，如果 sentinel 向 master 发送 PING 而没有收到 PONG，只能判定主观下线，必须采纳其它 sentinel 的意见，达到多数派后才能判定客观下线，进入主备切换流程
  2. 将周期心跳检测升级为累计心跳检测机制，即记录统计该节点的历史响应时间，如果超过警戒，则发起有限次的重试作为进一步判定



**如何实现高可用**

建议阅读《大型网站技术架构 – 核心原理与案例分析》一书，李智慧著，优点是条理清晰，不像另一些东拼西凑的文章
节录、概要如下：

* 应用层高可用

  * 关键是做到：**无状态**，即所有节点地位平等，去 session 化。利用**负载均衡**将请求发送到任意一台节点进行处理，如果有某个节点宕机，把该节点从服务列表中移除，不会影响业务运行

* 服务层高可用

  * 同样要做到：无状态，此外还应当考虑：

    ① 核心服务和非核心服务隔离部署，分级管理，方便非核心服务降级

    ② 对于即时性没有要求的服务可以考虑采用异步调用优化

    ③ 合理设置超时时间，在超时后应当有相应的处理策略，如：重试、转移、降级等

* 数据层高可用

  * 需要有**数据备份**机制与**故障转移**机制

  * 缓存服务是否需要高可用，两种观点：

    ① 缓存服务不可用会让数据库失去保护，因此需要保证缓存服务高可用

    ② 缓存服务不是数据存储服务，缓存宕机应当通过其他手段解决，如扩大缓存规模，一个缓存服务器的宕机只会影响局部



**全局 ID 生成**

1. 数据库 id 表
   * Oracle 数据库，直接使用序列作为 id
   * MySQL 数据库，采用自增主键作为 id，如果想避免单点故障，用多台 MySQL 使用不同的起始值和步长来设置 auto_increment
   * 缺点：数据库并发不高，属于集中式的解决方案
2. Redis 
   * 使用 incr 生成 id，由于 redis 的单线程特性，能保证它不会重复
   * 缺点：仍然属于集中式的解决方案，有网络消耗
3. UUID
   * UUID 有多种实现，典型的 UUID 实现会包含时间信息、MAC 地址信息、随机数
   * 优点：属于本地解决方案，无网络消耗
   * 缺点：MAC 地址提供了唯一性的保证，但也带来安全风险，最糟的是它是字符串形式，占用空间大，查询性能低，无法保证趋势递增
4. Snowflake
   * 通常的实现是 41 位时间信息、精确到毫秒，10 位的机器标识、12 位 的序列号，还有 1 位没有使用，共 8 个字节
   * 理解思想后，可以根据自己实际情况对原有算法做调整
   * 优点：本地解决方案，无网络消耗。长整型避免了字符串的缺点，并能保证趋势递增



**负载均衡策略**

负载均衡：即使用多台服务器共同分担计算任务，把网络请求和计算按某种算法均摊到各个服务器上

* 可以使用硬件实现（如 F5），也可以使用软件实现（如 Nginx、Dubbo、 Ribbon 等诸多软件均有自己的负载均衡实现）
* 常见负载均衡算法有：
  * 轮询，轮流来（Nginx、Ribbon）
  * 加权轮询，在轮询的基础上考虑权重，权重高的，分到请求的机会更多（Nginx 、Dubbo）
  * 最少连接，指谁的活跃连接数少，就把请求分发给谁，因为活跃多意味着响应慢（Nginx、Dubbo）
  * 最少响应时间，指谁的响应快，且活跃连接数少，就把请求发给谁（Nginx、Ribbon）
  * 随机，随便发给谁（Nginx、Dubbo、Ribbon）
  * hash，例如根据 ip 的hash 值分配请求，ip 相同的请求总会由同一台服务器处理（Nginx）
  * 一致性 hash，比 hash 好处在于添加、移除节点对请求分发影响较小（Dubbo）

> ***参考文档***
>
> * https://nginx.org/en/docs/http/load_balancing.html
> * https://dubbo.apache.org/zh/docsv2.7/user/references/xml/dubbo-provider/



**数据分片策略**

所谓分片就是指数据量较大时，对数据进行水平切分，让数据分布在多个节点上。

1. Hash
   * 按照 key 的 hash 值将数据映射到不同的节点上
   * 优点：实现简洁、数据分布均匀
   * 缺点1：如果直接 hash 与节点数取模，节点变动时就会造成数据大规模迁移，可以使用一致性 hash 改进
   * 缺点2：查询某一类热点数据时，由于它们是用 hash 分散到了不同节点上，造成查询效率不高
2. Range
   * 可以将 key 按照进行 range 划分，让某一范围的数据都存放在同一节点上
   * 优点1：按 range 查询，性能更高
   * 优点2：如果配合动态 range 分片，可以将较小的分片合并、将热点数据分散，有很多有用的功能
3. 静态调度与动态调度
   * 静态意味着数据分片后分布固定，即使移动也需要人工介入
   * 动态意味着通过管理器基于调度算法在各节点之间自由移动数据



**分布式事务 - 方案1：2PC 两阶段提交**

![image-20210902163714878](./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210902163714878.png)

* 在准备阶段所有参与者都返回 yes，则提交阶段通知参与者提交事务
* 在准备阶段有一个参与者返回 no，或是返回响应超时，则提交阶段通知所有参与者回滚事务

存在问题

* 阻塞型协议：所有参与者在等待接到下一步操作前，都处于阻塞，占用的资源也一直被锁定
* 过于保守：任一个节点失败都将导致事务回滚
* 数据不一致：在阶段二，如果只有部分参与者收到了提交请求，则会造成数据不一致
* 协调者单点问题：如果协调者故障在阶段二出现问题，会导致所有参与者（不会超时）始终处于阻塞状态，资源也被锁定得不到释放



**分布式事务 - 方案2：TCC 事务补偿**

![image-20210902163905501](./../../../%25E5%25AD%25A6%25E4%25B9%25A0%25E8%25A7%2586%25E9%25A2%2591/%25E6%259C%2580%25E6%2596%25B0%25E5%25BC%25BA%25E5%258C%2596/%25E9%259D%25A2%25E8%25AF%2595/1.Java%25E9%259D%25A2%25E8%25AF%2595%25E4%25B8%2593%25E9%25A2%2598%25E8%25AF%25BE/Java%25E9%259D%25A2%25E8%25AF%2595%25E4%25B8%2593%25E9%25A2%2598-%25E8%25B5%2584%25E6%2596%2599/day07-%25E5%2588%2586%25E5%25B8%2583%25E5%25BC%258F/%25E8%25AE%25B2%25E4%25B9%2589/img/image-20210902163905501.png)

* Try：对数据校验、资源预留
* Confirm：执行业务确认
* Cancel：实现与 try 相反的操作

要点

* 本质上还是两阶段提交，不过无需借助数据库驱动，在应用层完成，业务侵入比较深
* 需要每个节点上配置 TCC 框架，记录操作日志和状态，以便在宕机时恢复
* TCC 操作必须要考虑幂等



**分布式事务 - 方案3：基于可靠性消息的最终一致性方案**

要点

* 2PC 和 TCC 都属于同步方案，实际开发中更多采用的是异步方案

  * 例如：**下单**后的支付、扣减库存、增加积分等操作对实时性要求并不高。此时将**下单成功的消息**写入消息中间件，利用消息中间件实现最终一致性

* 问题转换成保证**本地事务与消息投递的原子性**

  * 例如：RocketMQ 的解决方案如下

    ① 发送消息到 broker ，只是此时消息称为**半消息**，无法消费

    ② 执行本地事务，如果成功，则半消息转换为正式消息，允许被消费；如果失败，删除 broker 上的半消息

    ③ 对于 broker 这端，如果迟迟不能收到半消息的 commit 或 rollback 信息，则会**回查**本地事务是否完成，根据状态确定如何处理



### 6. 一致性 Hash（补充）

前面讲负载均衡和数据分片时，都提到了一致性 Hash，它是为了解决在服务器增、删时普通 hash 算法造成数据大量迁移问题的



**普通 hash 算法**

* 假设有 3 台服务器，10 个 key 在服务器上的分布如下图所示

<img src="./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210902164609023.png" alt="image-20210902164609023" style="zoom:80%;" />

* 添加一台服务器后，数据分布变成下图，可以看到除了一个 key（上下颜色相同的）以外，其它 key 都得迁移

<img src="./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210902164843883.png" alt="image-20210902164843883" style="zoom:80%;" />

**一致性 hash 算法**

* 假设有 3 台服务器，10 个 key 在服务器上的分布如下图所示

<img src="./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210902165058027.png" alt="image-20210902165058027" style="zoom:80%;" />

* 添加一台服务器后，数据分布变成下图，发现仅有 3 个key 需要迁移（上下颜色不同的）

<img src="./Java%E9%9D%A2%E8%AF%95-%E9%BB%91%E9%A9%AC.assets/image-20210902165158024.png" alt="image-20210902165158024" style="zoom:80%;" />

