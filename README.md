# 整理的细节

1.二分查找，`mid = left + (right - left)`，然后每次`left = mid + 1`，`right = mid - 1`.注意不要等于`mid`.

2.c++中int最大值是`INT_MAX`，要引入头文件`#include<climit>`.

3.模板函数必须在头文件内定义，普通函数不能在头文件内定义，否则多个cpp引用该头文件会报错。

4.三目运算符完整语法：`condition ? value_if_true : value_if_false`

5.c++algorithm头文件中的sort排序底层是**IntroSort**（快排 + 堆排 + 插入排混合），效率非常高。

语法是：

```c++
std::sort(begin, end);                        // 升序排序
std::sort(begin, end, cmp);                   // 自定义排序规则
```

​	begin和end必须是迭代器。降序在cmp中填入`greater<int>()`.

6.

| 语法形式                      | 类型            | 是否修改原容器     | 是否避免拷贝开销         |
| ----------------------------- | --------------- | ------------------ | ------------------------ |
| `for (auto it : nums)`        | 值拷贝          | ❌ 无法修改原始数据 | ❌ 有拷贝开销             |
| `for (auto& it : nums)`       | 引用（非const） | ✅ 可修改原始数据   | ✅ 无拷贝                 |
| `for (const auto& it : nums)` | 引用（只读）    | ❌ 不能修改         | ✅ 无拷贝，推荐遍历大对象 |

​	`for (auto it = nums.begin(); it != nums.end(); ++it)`相当于`for (auto& it : nums)`。

7.求幂的 `pow` 函数，定义在头文件 `<cmath>` 中。

8.容器的最后一个元素取出来用back()，但也有的不支持。

9.`unordered_map<char, int> onemap;`可以这样定义。`unordered_map` 的一个非常常用也非常强大的特性：**下标访问操作符 `[]` 会自动进行插入并初始化默认值**。默认值是0.

10.set集合有.contains(x)方法可以判断x在不在集合中，.erase(x)去除集合中的元素x。

11.定长数组初始化：`int num[26]{}`，`{}` 将所有元素**初始化为 0**（默认值）。

12.取字典内部元素的值用一个迭代器it，然后it->first是key，it->second是value。

13.substr中的第二个值是长度！！！不是右区间！！s.substr(left, right);是非常错误的用法，应该是s.substr(left, len);

14.左值引用不能去绑定一个右值：int& r = 10;这是不对的，但这样就可以：

```c++
int a = 10;
int& r = a; // ✅ 可以
```

15.字典处理中如果某个key的值降为0了，可以用map.erase(x)删去

16.int的范围是：`-2147483648 <= int <= 2147483647`

```c++
int x = -2147483648;
int y = -x;  // ❌ 未定义行为

//安全做法一：
long long y = -1LL * x;
//安全做法二：
long long y = abs((long long)x);
```

17.set插入数据用insert()

18.数组的容量设置用`v.reserve(100); // 修改 capacity 为至少 100，size 仍为 0`。

重新为数组赋值用assign：

```c++
v.assign(100, -1);  // 变成长度为 100，全部是 -1
```

19.向上取整公式：

<img src="images\LeetcodeLearningNotes\image-20250803191411847.png" alt="image-20250803191411847" style="zoom: 40%;" />

20.二维数组创建：

```c++
vector<vector<int>> cnt(26);  // ✅ 合法，每个元素是一个空 //vector<int>
vector<vector<int>> cnt(26, vector<int>()); // 显式写法
```

21.在 **Windows 的 64 位系统**上：`long` 仍然是 32 位（和 `int` 一样）,64位需要long long

| 类型        | 位数 | 最大值（约等于）          | 约等于10 的几次方 |
| ----------- | ---- | ------------------------- | ----------------- |
| `short`     | 16   | 32,767                    | 3 x 10^4          |
| `int`       | 32   | 2,147,483,647             | 2.1 x 10^9        |
| `long long` | 64   | 9,223,372,036,854,775,807 | 9.2 x 10^18       |

22.string去除前导0的方法：

```c++
s.erase(0, s.find_first_not_of('0'));
```

23.lambda表达式：

```c++
type name = [capture](parameters) -> return_type {
    function_body;
}
-----------------示例---------------------
int x = 5, y = 10;
// 捕获 x 的值，不能改
auto val_capture = [x]() {
    cout << x << endl;
};
// 捕获 x 的引用，可以改
auto ref_capture = [&x]() {
    x += 10;
    cout << x << endl;
};
```

捕获的作用：让 lambda 使用**所在作用域外的变量**。

因为 lambda 是局部匿名函数，它默认不能访问外部变量——除非你 **显式捕获**。

 捕获语法：`[capture]`

| 捕获方式  | 例子                             | 含义               |
| --------- | -------------------------------- | ------------------ |
| `[=]`     | 捕获所有外部变量的**副本（值）** | 拷贝外部变量       |
| `[&]`     | 捕获所有外部变量的**引用**       | 修改外部变量       |
| `[x]`     | 只捕获 `x` 的**副本**            | 只读外部变量       |
| `[&x]`    | 只捕获 `x` 的**引用**            | 可修改外部变量     |
| `[=, &y]` | 捕获所有变量值，`y` 例外是引用   | 混合方式           |
| `[this]`  | 捕获当前类的 `this` 指针         | 用于类的成员函数中 |

24.c++11以上支持：

```c++
#include <algorithm>
#include <initializer_list>

int a = 5, b = 9, c = 3;
int result = std::max({a, b, c});  // ✅ C++11 起支持的写法
```

​	用大括号括起来，求多个元素的max

25.标准库中，`pair` 的比较规则是：

> **先比较第一个元素（first）**，如果相等，再比较第二个元素（second）。

26.开平方根用<cmath>中的sqrt函数，可以对结果用int强制类型转换

27.判断某个字符串存不存在于一堆字符串中，可以把这堆字符串放在unordered_set哈希集合里，然后用count()方法判断。

28.如何判断一个数是否作为key在一个unordered_map中?

​	用count可以返回bool值，用find返回迭代器

29.创建堆：

<img src="images\LeetcodeLearningNotes\image-20250822235911302.png" alt="image-20250822235911302" style="zoom:50%;" />

多个元素可以用pair：

<img src="images\LeetcodeLearningNotes\image-20250822235938794.png" alt="image-20250822235938794" style="zoom:50%;" />

greater会自动比较字典序，第一个相等会自动比第二个，哪个小哪个排前面。

30.无向图判断环的一个直观性质：如果点a四周有两个及以上的点a被遍历过,则必有环。

31.哈希记录距离的小细节：将二维坐标 (*x*,*y*) 转化为对应的一维下标 *i**d**x*=*x*∗*n*+*y* 作为 key 进行存储。

32.如何创建一个由nums的下标构成的数组，里面的元素是按nums从大到小排列的元素的对应下标：

```c++
int ids[n];
iota(ids, ids + n, 0);
sort(ids, ids + n, [&](int i, int j) { return nums[i] > nums[j]; });
```



# 大整数乘法问题

问题描述：给定一个大整数 `a`（通常以字符串形式输入，长度可达 1000+ 位）和一个整数 `n (0 ≤ n ≤ 1000)`，输出 `a^n` 的精确结果。

先要了解以下知识：

1.**a位数和b位数相乘，所得结果最大不超过(a+b)位**

2.**a 的第 i 位 × b 的第 j 位，乘积的值落在结果的第 i + j 位（从 0 开始计）**

推导：

```c++
a = 23  = 3×10^0 + 2×10^1     → a[0]=3, a[1]=2
b = 45  = 5×10^0 + 4×10^1     → b[0]=5, b[1]=4
a x b得到：
(2×10^1 + 3×10^0) × (4×10^1 + 5×10^0)
= 2×4×10^2  + 2×5×10^1  + 3×4×10^1 + 3×5×10^0
= 8×10^2   + 10×10^1 + 12×10^1 + 15×10^0
= (8×10^2) + (22×10^1) + (15×10^0)
10的幂次代表的就是所在的位数
```

思路：采用以上方法进行相乘，然后通过<u>快速幂</u>方法求幂值。

经验：

<img src="images\LeetcodeLearningNotes\image-20250726170426104.png" alt="image-20250726170426104" style="zoom:50%;" />

在写大数乘时，犯了个错误，这里单独看 `a[i] * b[j]` 是否超过 10 就提前进位了，其实是不对的。正确做法是：**先全部乘完加到 `res[i + j]` 上，再统一进位处理**。

## 快速幂

假设你要计算：
$$
a^n
$$
​	**普通做法**是用循环，把 a 连乘 n 次，时间复杂度是 O(n)，当 n 很大时，效率非常低。

快速幂的核心思想：

​	利用指数的二进制展开，把幂运算拆成更少的乘法：
$$
a^n =
\begin{cases}
1 & n=0 \\
a \times a^{n-1} & n \text{ 是奇数} \\
(a^{n/2})^2 & n \text{ 是偶数}
\end{cases}
$$
​	通过递归或迭代，把指数对半折半减少，时间复杂度变成 O(log n) 。

现在的问题是如何把这个算法转化为代码：

首先要理解**指数的二进制结构**：

```
13 的二进制是： 1101
=> 2^13 = 2^8 × 2^4 x 2^1
```

​	2的10次幂，就是2的8次幂乘上2的2次幂，这个8和2正好就对应了10的二进制里面**<u>为1的位</u>**。

​	在快速幂中，指数被看作二进制。

​	算法执行中，bash持有的值 = <u>当前对应的 <img src="images\LeetcodeLearningNotes\image-20250726174603660.png" alt="image-20250726174603660" style="zoom: 50%;" /></u>，其中 i 是当前处理的二进制位编号（从低位开始数）。

| 二进制位数 i | 当前 base 的值             |
| ------------ | -------------------------- |
| 0            | $a^{2^0} = a$              |
| 1            | $a^{2^1} = a^2$            |
| 2            | $a^{2^2} = a^4$            |
| 3            | $a^{2^3} = a^8$            |
| ...          | 每轮 base 都变成上轮的平方 |

​	具体代码中，只要n的当前位是 1，就把对应位次的 `base` 乘进 `result`。

```c++
base = a;
while (n > 0) {
    if (n % 2 == 1) // 结果如果为1说明最低位是1
        result *= base; // base 是当前这一位代表的次幂
    base *= base;       // base 升级为下一个幂（平方）
    n /= 2;             // 二进制数除以二相当于右移，自动处理下一位
}
```

​	以刚才的2的13次幂举例，因为第一位是1，所以对应的bash是2的2^1次方，乘入result中，下一位不是1，就只默默地变更bash，接着再下一位是1，乘入result中……以此类推。

> 为什么bash要从a开始？
>
> 因为bash就是个生成器，用来生成a，a的平方，a的四次方……对应的2的倍数的次方。

​	以下是递归的版本，跟公式完全一样的思路：

```c++
int power(int a, int n) {
    if (n == 0) return 1;
    if (n % 2 == 0) {
        int half = power(a, n / 2);
        return half * half;
    } else {
        return a * power(a, n - 1);
    }
}
```

# 质因数

**定义**

​	质因数是一个整数的质数因子，即能整除这个数的质数。

**质因数分解**

​	每个整数都可以分解成若干质因数的乘积。

**与最大公约数的关系**

​	如果两个数的最大公约数>1	`=`	它们至少有一个公共质因数 

**如何求**

暴力试除法，10的6到7次方以下量级的数据适用，思路：

​	挑选除数，从2开始，一直遍历到根号n。如果n能整除i，这个i就是质因数（因为i是从2开始遍历的，不可能存在i不是质数也能被n整除的情况），把n更新成n/i.如果需要所有的质因数，每次除以i都放进ans数组中，如果不想要重复的，每次n能整除i就写一个while，让n一直除以i，直到除不尽为止。最后待到i遍历到根号n时，如果n这时还大于1，就把n加进ans，因为剩下的n肯定是个质数。最终的ans数组就是n所有的质因数。

# 质数

求1到n之间的所有质数，最快方法是埃氏筛，从2开始，把所有2的倍数标记为合数，从3开始，把所有3的倍数标记为合数....一直到根号n。

值得注意的是，标记合数时，循环中j是从`i*i`开始标记，为什么不是`2*i`？这是在保证**<u>每个合数只被最小的质数标记一次</u>**。

<img src="images\LeetcodeLearningNotes\image-20250827172949160.png" alt="image-20250827172949160" style="zoom:40%;" />

# 递归是如何转为迭代的？

假设计算a的11次幂，

```
n = 11, result = a, bash = a^2
n = 5 , result = a^3, bash = a^4
n = 2 , result = a^3, bash = a^8
```

​	可以发现，当n为奇数时，原式就被拆成了a乘上a的偶次幂，这个例子是:
$$
a^{11} = a * a^{10}
$$
​	但这时如果a的偶次幂除以2，又是个奇数，就会变成这样：
$$
a^{11} = a * a^{5}*a^{5}
$$
​	a的5次幂，由于是奇数，会多拆出两个a：
$$
a^{11} = a * (a*a^{2}*a^{2})* (a*a^{2}*a^{2})
$$
​	可以发现这时<u>由于奇数而拆出来的a已经达到了3个，这正好对应了result是a的3次方</u>。而且由于每次偶数拆出来的都是一对，单独的a经过一对的相乘变成a平方，等到再接着如果继续拆，就会变成a的4次方。所以每次偶次幂拆成一对相乘时，需要有个东西记录下来，这样等到再拆奇次幂时，才能准确得到拆出来的a的个数。

​	于是bash诞生了。bash不仅可以计算拆出来的偶次幂里面a的幂数，还可以记录<u>拆奇次幂时多余的a的数量</u>。这些多余的a汇集到result，而bash全是2的倍数次幂，自然而然地成为了偶次幂的计算素材。

# 二叉树

![图片](images\LeetcodeLearningNotes\640.webp)

# 滑动窗口与双指针

## 一、定长滑动窗口

基础：

[1456. 定长子串中元音的最大数目](https://leetcode.cn/problems/maximum-number-of-vowels-in-a-substring-of-given-length/)

进阶：

[3439. 重新安排会议得到最多空余时间 I](https://leetcode.cn/problems/reschedule-meetings-for-maximum-free-time-i/)

## 二、不定长滑动窗口

### 1.求最长/最大子数组/越短越合法

基础：

[3. 无重复字符的最长子串](https://leetcode.cn/problems/longest-substring-without-repeating-characters/)：滑动窗口，但与哈希表进行了结合。

[3090. 每个字符最多出现两次的最长子字符串](https://leetcode.cn/problems/maximum-length-substring-with-two-occurrences/)：秒了

[1493. 删掉一个元素以后全为 1 的最长子数组](https://leetcode.cn/problems/longest-subarray-of-1s-after-deleting-one-element/)：拿下

[1208. 尽可能使字符串相等](https://leetcode.cn/problems/get-equal-substrings-within-budget/)：秒了

进阶：

[2779. 数组的最大美丽值](https://leetcode.cn/problems/maximum-beauty-of-an-array-after-applying-operation/)：<img src="images\LeetcodeLearningNotes\image-20250728165309623.png" alt="image-20250728165309623" style="zoom:25%;" />

<img src="images\LeetcodeLearningNotes\image-20250728170046261.png" alt="image-20250728170046261" style="zoom: 40%;" />

<img src="images\LeetcodeLearningNotes\image-20250728170118884.png" alt="image-20250728170118884" style="zoom:40%;" />

神了：![image-20250728170022627](images\LeetcodeLearningNotes\image-20250728170022627.png)

### 2.求最短/最小子数组/越长越合法

[209. 长度最小的子数组](https://leetcode.cn/problems/minimum-size-subarray-sum/)：写过了

[2904. 最短且字典序最小的美丽子字符串](https://leetcode.cn/problems/shortest-and-lexicographically-smallest-beautiful-string/)：秒了

[76. 最小覆盖子串](https://leetcode.cn/problems/minimum-window-substring/)：艰难过了。

​	这题题解很秀，甚至还有优化版本

初始化 ansLeft=−1, ansRight=m，用来记录最短子串的左右端点，其中 m 是 s 的长度。
用一个哈希表（或者数组）cntT 统计 t 中每个字母的出现次数。
初始化 left=0，以及一个空哈希表（或者数组）cntS，用来统计 s 子串中每个字母的出现次数。
遍历 s，设当前枚举的子串右端点为 right，把 s[right] 的出现次数加一。
遍历 cntS 中的每个字母及其出现次数，如果出现次数都大于等于 cntT 中的字母出现次数：
如果 right−left<ansRight−ansLeft，说明我们找到了更短的子串，更新 ansLeft=left, ansRight=right。
把 s[left] 的出现次数减一。
左端点右移，即 left 加一。
重复上述三步，直到 cntS 有字母的出现次数小于 cntT 中该字母的出现次数为止。
最后，如果 ansLeft<0，说明没有找到符合要求的子串，返回空字符串，否则返回下标 ansLeft 到下标 ansRight 之间的子串。

```c++
class Solution {
    bool is_covered(int cnt_s[], int cnt_t[]) {
        for (int i = 'A'; i <= 'Z'; i++) {
            if (cnt_s[i] < cnt_t[i]) {
                return false;
            }
        }
        for (int i = 'a'; i <= 'z'; i++) {
            if (cnt_s[i] < cnt_t[i]) {
                return false;
            }
        }
        return true;
    }

public:
    string minWindow(string s, string t) {
        int cnt_s[128]{}; // s 子串字母的出现次数
        int cnt_t[128]{}; // t 中字母的出现次数
        for (char c : t) {
            cnt_t[c]++;
        }

        int m = s.size();
        int ans_left = -1, ans_right = m;
        int left = 0;
        for (int right = 0; right < m; right++) { // 移动子串右端点
            cnt_s[s[right]]++; // 右端点字母移入子串
            while (is_covered(cnt_s, cnt_t)) { // 涵盖
                if (right - left < ans_right - ans_left) { // 找到更短的子串
                    ans_left = left; // 记录此时的左右端点
                    ans_right = right;
                }
                cnt_s[s[left]]--; // 左端点字母移出子串
                left++;
            }
        }
        return ans_left < 0 ? "" : s.substr(ans_left, ans_right - ans_left + 1);
    }
};
```

[2875. 无限数组的最短子数组](https://leetcode.cn/problems/minimum-size-subarray-in-infinite-array/)：

自己非常艰难地过了：

<img src="images\LeetcodeLearningNotes\image-20250730191211103.png" alt="image-20250730191211103" style="zoom:33%;" />

<img src="images\LeetcodeLearningNotes\image-20250730191225828.png" alt="image-20250730191225828" style="zoom: 50%;" />

​	我自己的算法，这个会导致超时。

​	灵神实在是太强了，遇见重复的，要学会取余，<u>**把中间重复的化解掉，只看两端。**</u>不仅仅是计算简单，更是节约了时间。

### 3.求子数组个数

#### 越长越合法

​	核心：一般要写 `ans += left`。

​	内层循环结束后，[left,right] 这个子数组是<u>**不满足**</u>题目要求的，但在退出循环之前的最后一轮循环，[left−1,right] 是满足题目要求的。由于子数组越长，越能满足题目要求，所以除了 [left−1,right]，还有 [left−2,right],[left−3,right],…,[0,right] 都是满足要求的。也就是说，当右端点固定在 right 时，左端点在 0,1,2,…,left−1 的所有子数组都是满足要求的，这一共有 left 个。

[1358. 包含所有三种字符的子字符串数目](https://leetcode.cn/problems/number-of-substrings-containing-all-three-characters/)：秒了

[2799. 统计完全子数组的数目](https://leetcode.cn/problems/count-complete-subarrays-in-an-array/)：秒了

#### 越短越合法

​	核心：一般要写 `ans += right - left + 1`。

​	内层循环结束后，[left,right] 这个子数组是<u>**满足**</u>题目要求的。由于子数组越短，越能满足题目要求，所以除了 [left,right]，还有 [left+1,right],[left+2,right],…,[right,right] 都是满足要求的。也就是说，当右端点固定在 right 时，左端点在 left,left+1,left+2,…,right 的所有子数组都是满足要求的，这一共有 right−left+1 个。

[713. 乘积小于 K 的子数组](https://leetcode.cn/problems/subarray-product-less-than-k/)：秒了

[2302. 统计得分小于 K 的子数组数目](https://leetcode.cn/problems/count-subarrays-with-score-less-than-k/)：秒了

#### 恰好型滑动窗口

要计算有多少个元素和恰好等于 k 的子数组，可以把问题变成：

计算有多少个元素和 ≥k 的子数组。
计算有多少个元素和 >k，也就是 ≥k+1 的子数组。
答案就是元素和 ≥k 的子数组个数，减去元素和 ≥k+1 的子数组个数。这里把 > 转换成 ≥，从而可以把滑窗逻辑封装成一个函数 f，然后用 f(k) - f(k + 1) 计算，无需编写两份滑窗代码。

总结：「恰好」可以拆分成两个「至少」，也就是两个「越长越合法」的滑窗问题。

注：也可以把问题变成 ≤k 减去 ≤k−1（两个至多）。可根据题目选择合适的变形方式。

[930. 和相同的二元子数组](https://leetcode.cn/problems/binary-subarrays-with-sum/)：没写出来，但看完上述的解析直接悟了。

1248.统计优美子数组：用大于等于和小于等于都写了一遍。

## 三、单序列双指针

### 1.相向双指针

​	两个指针 *left*=0, *right*=*n*−1，从数组的两端开始，向中间移动，这叫**相向双指针**。上面的滑动窗口相当于**同向双指针**。

[1750. 删除字符串两端相同字符后的最短长度](https://leetcode.cn/problems/minimum-length-of-string-after-deleting-similar-ends/)：秒了。

### 2.同向双指针

​	两个指针的移动方向相同（都向右，或者都向左）。

[611. 有效三角形的个数](https://leetcode.cn/problems/valid-triangle-number/)：

<img src="images\LeetcodeLearningNotes\image-20250731185620782.png" alt="image-20250731185620782" style="zoom:40%;" />

把问题转化为：从 *nums* 中选三个数，满足 1≤*a*≤*b*≤*c* 且 *a*+*b*>*c* 的方案数。

**枚举最长边+相向双指针**：

<img src="images\LeetcodeLearningNotes\image-20250731190023564.png" alt="image-20250731190023564" style="zoom:50%;" />

最外层的循环是c在移动，内层有两个相向双指针i和j，初始值是0和right - 1.

内层循环的条件判断是：==左边指针i指向的值nums[i]小于右边指针指向的nums[j]==，直到大于等于才停止内层的循环。

- 如果nums[i] + nums[j] > c。nums[j] 与下标 i'在 [i,j−1] 中的任何 nums[i′] 相加，都是 >c 的，因此直接找到了 j−i 个合法方案，加到答案中，然后将 j 减一。
- 如果nums[i] + nums[j] ≤ c。这时无论怎么移动j，nums[i] + nums[j] 都小于c，所以此时要右移i，i++。
- 重复上述直到i >= j.

总的结构是这样的：

```c++
for (int k = 2; k < nums.size(); k++) {
    while (i < j) {
        if (nums[i] + nums[j] > c) {
        } else {    
        }
    }
}
```

**枚举最短边+同向双指针：**

感觉比上一个方法抽象一点点。

枚举最短边 a，问题变成计算满足 c−b<a 的 (b,c) 个数。

这个条件意味着，当 a 固定不变时，b 和 c 不能隔太远。

这可以用同向双指针解决。

- 枚举 a=nums[i]，其中 i=0,1,2,…,n−3。
- 如果 a=0，则跳过。
- 现在计算，对于 k=i+2,i+3,…,n−1，有多少个符合要求的 j？。
- 枚举 k 的同时，维护指针 j，初始值为 i+1。
- 如果发现 nums[k]−nums[j]≥a，说明 b 和 c 隔太远了，那么把 j 不断加一，直到 c−b<a，也就是 nums[k]−nums[j]<a 为止。
- 此时，对于固定的 a=nums[i] 和固定的 c=nums[k]，nums[j],nums[j+1],…,nums[k−1] 都可以作为 b，这一共有 k−j 个，加入答案。

```c++
for (int i = 0; i < n - 2; i++) {
            int a = nums[i];
            if (a == 0) { // 三角形的边不能是 0
                continue;
            }
            int j = i + 1;
            for (int k = i + 2; k < n; k++) {
                while (nums[k] - nums[j] >= a) {
                    j++;
                }
                // 如果 a=nums[i] 和 c=nums[k] 固定不变
                // 那么 b 可以是 nums[j],nums[j+1],...,nums[k-1]，一共有 k-j 个
                ans += k - j;
            }
        }
```

[1574. 删除最短的子数组使剩余数组有序](https://leetcode.cn/problems/shortest-subarray-to-be-removed-to-make-array-sorted/)：最终114/119

<img src="images\LeetcodeLearningNotes\image-20250731235453665.png" alt="image-20250731235453665" style="zoom:50%;" />

**写法一：枚举左端点，移动右端点**

​	理解起来有一定难度，把问题抽象成这么个情况：

​	相当于要把两个非递减子串拼接到一起，需要计算出两个非递减子串中间元素个数的最小值。先求right就相当于，先求出第二个非递减子串的首元素。右边找好后，在左边的子串中枚举每一个递增数（left指向的数），都和右边最小的值（即right指针指向的数）做比较，如果小于，那么就整体上满足递增，之后就是滑动窗口登场了。如果左边枚举的数大了，就缩右边子串的开头，直到缩到右边子串的开头还是大于左边枚举的值。

​	==coding中有个关键点是ans的初始值设定，按照这种写法初值为right！切记不是arr.size()！==

写法二类似写法一，在此不表。

### 3.背向双指针

[1793. 好子数组的最大分数](https://leetcode.cn/problems/maximum-score-of-a-good-subarray/)：

<img src="images\LeetcodeLearningNotes\image-20250801160304461.png" alt="image-20250801160304461" style="zoom:50%;" />

**方法一：单调栈**

这题可以看作求[84. 柱状图中最大的矩形](https://leetcode.cn/problems/largest-rectangle-in-histogram/)，只是加了个限定：矩形中必须包含k。

思路：枚举nums中的每个元素作为矩形的高度h = nums[p]，现在的问题聚焦于找矩形最大的宽度。求矩形最大的宽度要去找：

- 在 p左侧的小于 h 的最近元素的下标 left，如果<u>**不存在则为 −1**</u>。求出了 left，那么 left+1 就是矩形最左边那根柱子。
- 在 p 右侧的小于 h 的最近元素的下标 right，如果<u>**不存在则为 n**</u>。求出了 right，那么 right−1 就是矩形最右边那根柱子。

> 这些初始值尤其要重视，记住。

**方法二：背向双指针**

​	由于这题核心是对子数组的最小值敏感，所以要让<u>**最小值减小的更慢**</u>！以k为左右指针的起始位置，i是左指针，j是右指针，背向移动。如果当nums[i - 1] < nums[j + 1]，此时左指针再向左移动时，就会使最小值更新了，所以此时应该保持左指针不动，而去移动右指针。如果nums[i - 1] > nums[j + 1]，则相反移动左指针。相等的话移动哪个都行。

​	想清楚以上这些尤为关键，我想到了要让最小值减小的慢，但还是没想到位如何处理左右指针。

### 4.原地修改

[80. 删除有序数组中的重复项 II](https://leetcode.cn/problems/remove-duplicates-from-sorted-array-ii/)：利用栈，原地删除元素，看完解析后秒了。  

[922. 按奇偶排序数组 II](https://leetcode.cn/problems/sort-array-by-parity-ii/)：灵神用双指针，一个指向每一个奇数下标，一个指向每一个偶数下标，每次递增2.

[1089. 复写零](https://leetcode.cn/problems/duplicate-zeros/):

<img src="images\LeetcodeLearningNotes\image-20250802022019112.png" alt="image-20250802022019112" style="zoom:50%;" />

左边一个指针i，从下标0开始遍历arr，右边的指针j从n - 1开始遍历arr。j表示要从原arr中移除的元素（因为0的个数变多，所以会有不少元素“溢出”arr，j记录的就是这些溢出的元素）。但是要注意，如果i指向的元素不为0，那j就无需移动。j最终指向的是新arr的最后一个元素。

```c++
void duplicateZeros(vector<int>& arr) {
        int n = arr.size(), i, j = n-1, k = n-1;
        for(i = 0; i < j; ++i)
            if(arr[i] == 0)
                --j;
        // 此处特判需要读者多加思考
        if(i == j && arr[i] == 0) arr[k--] = arr[j--];
        while(j >= 0)
        {
            if(arr[j] == 0)
                arr[k--] = 0;
            arr[k--] = arr[j--];
        }
    }
```

<img src="images\LeetcodeLearningNotes\image-20250802022416505.png" alt="image-20250802022416505" style="zoom:33%;" />

<img src="images\LeetcodeLearningNotes\image-20250802022432081.png" alt="image-20250802022432081" style="zoom:33%;" />

<img src="images\LeetcodeLearningNotes\image-20250802022501097.png" alt="image-20250802022501097" style="zoom:33%;" />

<img src="images\LeetcodeLearningNotes\image-20250802023033767.png" alt="image-20250802023033767" style="zoom:33%;" />

特判：考虑一个情况：数组的最后一个能“放下”的位置刚好是一个 0，但数组长度只够放下它的一次，**无法容纳重复的 0**。此时如果不做特殊处理，在主循环中会将这个 0 当作需要“双倍复制”，导致下标越界或错位。

```c++
 // 此处特判需要读者多加思考
   if(i == j && arr[i] == 0) arr[k--] = arr[j--];
```

因为最后一个元素是 `0`，那只允许它放入一次（不能放两次），所以就手动放进去一次，即`arr[k--] = arr[j--]`；

至于为什么要写 i == j，如果arr = {1，0，0，1，3}这种，出for循环时i>j.即i指向的那个0导致j向左移动，而此时j又等于了i，就导致i++后大于了j

[75. 颜色分类](https://leetcode.cn/problems/sort-colors/)：

<img src="images\LeetcodeLearningNotes\image-20250802163426550.png" alt="image-20250802163426550" style="zoom: 50%;" />

**方法一：三指针**

​	最左边left指针维护要放入0的位置，mid维护要放入1的位置，right维护要放入2的位置。

​	在主循环中：

- 让mid 先等于right（因为right右边全是2，mid向左遍历，无需管大于1的），然后mid向左移动直到找到一个不为1的数，此时mid指向这个数并停下。
- 这时判断left指向的数是不是2，如果是2，交换nums【left】和nums【right】，并把right减1.再判断right指向的是不是0，如果是，同样交换left和right指向的数，但这样是left加1.
- 最后写两个while循环，一直右移left，直到遇到非0的数，一直左移right，直到遇到非2的数。

​	这是一整个主循环的内容，停止的条件是mid来到了left左边，显然0，1，2都已就绪。

**方法二：统计并重新放入**

​	统计nums中0，1，2的个数，这里统计数组的长度是由nums中元素的种类决定的，长度为3，而不等于nums的长度n。

## 四、双序列双指针

### 1.双指针

[2109. 向字符串添加空格](https://leetcode.cn/problems/adding-spaces-to-a-string/)：秒了

[2540. 最小公共值](https://leetcode.cn/problems/minimum-common-value/)：秒了

[面试题 16.06. 最小差](https://leetcode.cn/problems/smallest-difference-lcci/)：双指针秒了

### 2.判断子序列

[392. 判断子序列](https://leetcode.cn/problems/is-subsequence/)：秒了

[524. 通过删除字母匹配到字典里最长单词](https://leetcode.cn/problems/longest-word-in-dictionary-through-deleting/)：秒了

[1023. 驼峰式匹配](https://leetcode.cn/problems/camelcase-matching/)：秒了

#### 进阶

[1898. 可移除字符的最大数目](https://leetcode.cn/problems/maximum-number-of-removable-characters/)：没做出来但好像要用到二分，以后再写。

## 五、三指针



## 六、分组循环

适用场景：按照题目要求，数组会被分割成若干组，每一组的判断/处理逻辑是相同的。

核心思想：

​	外层循环负责遍历组之前的准备工作（记录开始位置），和遍历组之后的统计工作（更新答案最大值）。
​	内层循环负责遍历组，找出这一组最远在哪结束。
​	这个写法的好处是，各个逻辑块分工明确，也不需要特判最后一组（易错点）。以灵神的经验，这个写法是所有写法中最不容易出 bug 的。

[1446. 连续字符](https://leetcode.cn/problems/consecutive-characters/) ：简单

[2760. 最长奇偶子数组](https://leetcode.cn/problems/longest-even-odd-subarray-with-threshold/)：简单

[1578. 使绳子变成彩色的最短时间](https://leetcode.cn/problems/minimum-time-to-make-rope-colorful/)：秒了

# 二分算法

​	牢记left到right之间是还未确定是否符合要求的元素，至于方式的话，闭区间，开区间，半开半闭都可以。

## 一、二分查找

### 1.基础

[34. 在排序数组中查找元素的第一个和最后一个位置](https://leetcode.cn/problems/find-first-and-last-position-of-element-in-sorted-array/)：秒了

[35. 搜索插入位置](https://leetcode.cn/problems/search-insert-position/) ：秒了

### 2.进阶

[2300. 咒语和药水的成功对数](https://leetcode.cn/problems/successful-pairs-of-spells-and-potions/)：秒了

[911. 在线选举](https://leetcode.cn/problems/online-election/)：过了，没啥意思

## 二、二分答案

​	二分答案的一个难点是 `check` 函数怎么写，这会涉及到**贪心**等技巧，可以练练贪心题单（主要是第一章节）。

### 1.求最小

​	题目求什么，就二分什么。

[1283. 使结果不超过阈值的最小除数](https://leetcode.cn/problems/find-the-smallest-divisor-given-a-threshold/)：

​	这题最关键是发现：当除数越来越大，计算的结果会随着越来越小，有单调性。这时可以用二分，但要思考right边界应该定在哪。

### 2.求最大

[275. H 指数 II](https://leetcode.cn/problems/h-index-ii/)：秒了

### 3.二分间接值

[3143. 正方形中的最多点数](https://leetcode.cn/problems/maximum-points-inside-the-square/):想到了切比雪夫距离，思路没啥问题。

### 4.最小化最大值

### 5.最大化最小值

[3281. 范围内整数的最大得分](https://leetcode.cn/problems/maximize-score-of-numbers-in-ranges/)：贪心地想，第一个数越小，第二个数就越能在区间内，所以第一个数要选 *x*0=*start*[0]。这句话是关键。

如果一个点的值加上了mid，还不够，没关系，直接把下个点的开始位置设成这个点对应的start[i]。因为mid是最小的距离，只要保证点之间距离<u>**大于等于它就行**</u>，不一定非要大于。

### 6.第K小/大

​	例如数组 [1,1,1,2,2]，其中第 1 小、第 2 小和第 3 小的数都是 1，第 4 小和第 5 小的数都是 2。

- 第 k 小等价于：求最小的 x，满足 ≤x 的数至少有 k 个。

- 第 k 大等价于：求最大的 x，满足 ≥x 的数至少有 k 个。

> 注 1：一般规定 k 从 1 开始，而不是像数组下标那样从 0 开始。

> 注 2：部分题目也可以用堆解决。

[668. 乘法表中第k小的数](https://leetcode.cn/problems/kth-smallest-number-in-multiplication-table/)：卡在了求乘法表中小于等于x的数的个数上。

​	这里很精妙：如果i表示行号的话，由于每一行的元素都是[i, 2i, 3i, 4i, ... , ni]，所以一行中小于等于x的数是：

<img src="images\LeetcodeLearningNotes\image-20250804220524679.png" alt="image-20250804220524679" style="zoom:50%;" />

​	i在这里可以看作是基数，由于行号增加，基数也增加，x/i并向下取整其实求的就是列的数，即有多少个 j 满足：i×j≤x （其中 1≤j≤n），把 j 除到右边就得到了上式。

- 在二分的时候，**x 本身不一定在乘法表中**，但最后返回的 `x`，为什么**一定是乘法表中存在的第 k 小的数**？

>  `check(x)` 的意义是：在乘法表中，有多少个数 **小于等于 x**
>
> 如果返回true，说明小于等于mid的数还有点多，mid可以继续向左边找，于是更新right。记住在check中正好等于的 条件，就代表最后要返回的。比如这里就是返回right。

## 三、其他



# 单调栈

## 一、单调栈

### 1.基础

[739. 每日温度](https://leetcode.cn/problems/daily-temperatures/)：秒了

[503. 下一个更大元素 II](https://leetcode.cn/problems/next-greater-element-ii/)：秒了

### 2.进阶



## 二、矩形

[84. 柱状图中最大的矩形](https://leetcode.cn/problems/largest-rectangle-in-histogram/)：不用单调栈用滑窗会导致最终时间复杂度趋近于n^2

[85. 最大矩形](https://leetcode.cn/problems/maximal-rectangle/)：类似84.

[1504. 统计全 1 子矩形](https://leetcode.cn/problems/count-submatrices-with-all-ones/)：过了，这题真的很有意思。

<img src="images\LeetcodeLearningNotes\image-20250805153500383.png" alt="image-20250805153500383" style="zoom:50%;" />

​	用 单调栈 计算小于 heights[j] 的左边最近柱子的位置 left，把子矩形分成两类：

- 左边界 ≤left。假设在右边界为 j=left 时我们算出这有 f 个，现在将其右边界扩展到 j，得到 f 个新的子矩形。
- 左边界 >left。矩形左边界可以是 left+1,left+2,…,j，共 j−left 种；矩形高度可以是 1,2,…,heights[j]，共 heights[j] 种。所以有 (j−left)⋅heights[j] 个子矩形。
- 二者相加，就是右边界在 j 的子矩形个数。加到答案中。

## 三、贡献法

[907. 子数组的最小值之和](https://leetcode.cn/problems/sum-of-subarray-minimums/)：

## 四、最小字典序

[402. 移掉 K 位数字](https://leetcode.cn/problems/remove-k-digits/)：过了

[316. 去除重复字母](https://leetcode.cn/problems/remove-duplicate-letters/)：秒了。

[321. 拼接最大数](https://leetcode.cn/problems/create-maximum-number/)：两个数组是枚举，如果左边数组取1个数，右边就要取k - 1个数。然后单独求 ：取每个数组里面的k1个数能让子序列字典序最大这个是比较简单的，用单调栈即可。之后把两个结果进行合并。

![](images\LeetcodeLearningNotes\dcae5d6f7feb6e8adb14ad15292f052771d6dfdf1e682d6e657f69b6a404479e.jpg)

合并可以用双指针，依次比较两个指针指向的元素，把更大的元素放入数组，并移动这个指针，保持另一个指针不变，直到两个指针都走到尽头。

**==需要注意的是双指针指向的两个数相等的情况：==**

![image-20250806005025632](images\LeetcodeLearningNotes\image-20250806005025632.png)

​	需要比较后面的元素，直到某一边大于另一边。比如这种情况如果选右边的5，就变成了5564，选左边就是5654.

附代码:

```c++
// 比较两个子序列 subsequence1 和 subsequence2 从各自的起始位置 
// index1、index2 开始往后走，谁更大（字典序）。
int compare(vector<int>& subsequence1, int index1, vector<int>& subsequence2, int index2) {
        int x = subsequence1.size(), y = subsequence2.size();
        while (index1 < x && index2 < y) {
            int difference = subsequence1[index1] - subsequence2[index2];
            if (difference != 0) { //某一位不相等
                return difference;
            }
            index1++;
            index2++;
        }
        return (x - index1) - (y - index2);// 如果前面都一样，谁后面长谁赢
    }
```

# 贪心算法

问：贪心和 DP 的区别是什么？

- 答：DP 可以视为带记忆化的暴力搜索，只要不遗漏任何分支，答案一定是对的。贪心可以视为带剪枝的搜索，如果贪心策略不对，就容易贪过头，把正确的分支给剪掉。

问：有没有万能方法，判断一道题是贪心还是 DP？

- 答：很难。如果不知道题目类型，把 DP 想成贪心的大有人在。我的建议是先思考 DP 能不能做，再思考贪心。如果 DP 的时间复杂度足以通过题目，就不用思考贪心策略了。

# 动态规划

​	记忆化搜索是新手村神器（甚至可以用到游戏后期），推荐先看 动态规划入门：从记忆化搜索到递推。

​	但记忆化搜索并不是万能的，某些题目只有写成递推，才能结合数据结构等来优化时间复杂度，多数题目还可以优化空间复杂度。所以尽量在写完记忆化搜索后，把递推的代码也写一下。熟练之后直接写递推也可以。

## 一、入门DP

<img src="images\LeetcodeLearningNotes\image-20250806015123451.png" alt="image-20250806015123451" style="zoom: 33%;" />

​	记忆化搜索 = 递归搜索 + 保存计算结果

​	时间复杂度计算公式：状态个数 x 单个状态所需要的时间

<img src="images\LeetcodeLearningNotes\image-20250806015958319.png" alt="image-20250806015958319" style="zoom:33%;" />

> 递归和循环都是对每个i做计算。

改成递推需要对i=0和i=1做特殊处理，因为会产生负数下标。处理办法是把i改成从2开始（f数组前面插入2个0），也可以把带i的地方都加2.

### 1.爬楼梯

[70. 爬楼梯](https://leetcode.cn/problems/climbing-stairs/)：秒了

[2266. 统计打字方案数](https://leetcode.cn/problems/count-number-of-texts/)：过了

### 2.打家劫舍

[198. 打家劫舍](https://leetcode.cn/problems/house-robber/)：模板题

[213. 打家劫舍 II](https://leetcode.cn/problems/house-robber-ii/)：分类讨论，考虑是否偷 nums[0]：

- 如果偷 nums[0]，那么 nums[1] 和 nums[n−1] 不能偷，问题变成从 nums[2] 到 nums[n−2] 的非环形版本，调用 198 题的代码解决；
- 如果不偷 nums[0]，那么问题变成从 nums[1] 到 nums[n−1] 的非环形版本，同样调用 198 题的代码解决。
- 这两种方案覆盖了所有情况（毕竟 nums[0] 只有偷与不偷，没有第三种选择），所以取两种方案的最大值，即为答案。

[3186. 施咒的最大总伤害](https://leetcode.cn/problems/maximum-total-damage-with-spell-casting/)：552/554

### 3.最大子数组和(最大子段和)

​	有两种做法：

- 定义状态 f[i] 表示以 a[i] 结尾的最大子数组和，不和 i 左边拼起来就是 f[i]=a[i]，和 i 左边拼起来就是 f[i]=f[i−1]+a[i]，取最大值就得到了状态转移方程 f[i]=max(f[i−1],0)+a[i]，答案为 max(f)。这个做法也叫做 Kadane 算法。
- 用 前缀和，转化成 121. 买卖股票的最佳时机。

[2606. 找到最大开销的子字符串](https://leetcode.cn/problems/find-the-substring-with-maximum-cost/)：秒了

[2321. 拼接数组的最大分数](https://leetcode.cn/problems/maximum-score-of-spliced-array/):

<img src="images\LeetcodeLearningNotes\image-20250807011616786.png" alt="image-20250807011616786" style="zoom: 50%;" />

![image-20250807011551022](images\LeetcodeLearningNotes\image-20250807011551022.png)



## 二、网格图DP

​	对于一些二维 DP（例如背包、最长公共子序列），如果把 DP 矩阵画出来，其实状态转移可以视作在网格图上的移动。所以在学习相对更抽象的二维 DP 之前，做一些形象的网格图 DP 会让后续的学习更轻松（比如 0-1 背包的空间优化写法为什么要倒序遍历）	

### 1.基础

[64. 最小路径和](https://leetcode.cn/problems/minimum-path-sum/)：秒了

### 2.进阶

[1594. 矩阵的最大非负积](https://leetcode.cn/problems/maximum-non-negative-product-in-a-matrix/)：秒了

### 三、背包

​	<u>**注意背包必须是【顺序无关】的！**</u>

### 1.0-1背包

​	每个物品只能选一次。

<img src="images\LeetcodeLearningNotes\image-20250807151535488.png" alt="image-20250807151535488" style="zoom: 33%;" />



[494. 目标和](https://leetcode.cn/problems/target-sum/)：典型题

![image-20250807160023989](images\LeetcodeLearningNotes\image-20250807160023989.png)

​	这里是巧妙之处在于，由于nums全是非负的，所以设符合要求的某个方法种，非负数的和是p，nums元素总和是s。那么负数的绝对值一定是s-p。所以：

```
tartget = 正的 - 负的
		= p - (s - p)
		= 2p - s
所以 p = (target + s) / 2
```

​	注意p必须是一个偶数，因为这里除以二必须要整除，如果不是整除就不是恰好等于target了。因此t+s如果是奇数可以直接返回0，没有任何一种方案满足。

之后再进行以下的记忆化搜索：

```python
@cache
def dfs(i,c):
	if i < 0:
		return 1 if c==0 else 0 
	if c< nums[i]:
		return dfs(i-1，c)
	return dfs(i-1,c) + dfs(i-1,c-nums[i])
```

#### 进阶

[3082. 求出所有子序列的能量和](https://leetcode.cn/problems/find-the-sum-of-the-power-of-all-subsequences/)：太难了写不出来

​	给你一个长度为 `n` 的整数数组 `nums` 和一个 **正** 整数 `k` 。一个整数数组的 **能量** 定义为和 **等于** `k` 的子序列的数目。请你返回 `nums` 中所有子序列的 **能量和** 。由于答案可能很大，请你将它对 `109 + 7` **取余** 后返回。

<img src="images\LeetcodeLearningNotes\image-20250807173813477.png" alt="image-20250807173813477" style="zoom:33%;" />

![image-20250807185113448](images\LeetcodeLearningNotes\image-20250807185113448.png)

![image-20250807183010571](images\LeetcodeLearningNotes\image-20250807183010571.png)

**为什么要用二维0-1背包**？

​	因为我们必须要知道在某个包含前i个元素的子序列的   总和为k的子序列的长度，即<u>**子序列的子序列的长度**</u>。

**代码实现**

​	可以把i这个维度优化掉，核心是倒序遍历j和c，

```c++
int sumOfPower(vector<int> &nums, int k) {
    const int MOD = 1'000'000'007;
    int n = nums.size();
    vector<vector<int>> f(k + 1, vector<int>(n + 1));
    f[0][0] = 1;
    for (int i = 0; i < n; i++) {
        for (int j = k; j >= nums[i]; j--) {
            for (int c = i + 1; c > 0; c--) {
                f[j][c] = (f[j][c] + f[j - nums[i]][c - 1]) % MOD;
            }
        }
    }

    int ans = 0;
    int pow2 = 1;
    for (int i = n; i > 0; i--) {
        ans = (ans + (long long) f[k][i] * pow2) % MOD;
        pow2 = pow2 * 2 % MOD;
    }
    return ans;
}
```

**会不会存在`f[j - nums[i]][c - 1]`还没被填好的问题？**

​	并不会存在，因为当前 `i` 轮中，我们 ==<u>**只负责从旧的 `f[j - nums[i]][c - 1]` 推导新的 `f[j][c]`**</u>。==`f[j - nums[i]][c - 1]`是上一轮被写入的值。

**优化掉i这个维度，为什么不可以正序遍历j和c？**

 	注意三维的状态转移方程：<img src="images\LeetcodeLearningNotes\image-20250807184238639.png" alt="image-20250807184238639" style="zoom:40%;" />，这里面我们想要取得i+1时的值，依托的是i时的值。如果正序遍历，即

```c++
for (int j = nums[i]; j <= k; j++) {
    for (int c = 1; c <= i + 1; c++) {
         f[j][c] = (f[j][c] + f[j - nums[i]][c - 1]) % MOD;
    }
}
```

会发现在更新`f[j][c]`时，`f[j - nums[i]][c - 1]` 可能已经是这轮（当前 i）新更新后的结果，不再是上一轮（i - 1）留下的状态。因为虽然j在增大，但j-nums[i]计算出来的值依然是从0到k-nums[i]的，很容易就被覆盖。

​	而如果是倒序，j从很大的地方下来，j -nums[i]的值又一定是在减小的，可以保证不会被覆盖。

​	本质还是这一轮的i需要用上一轮i-1对应的`f[j][c]`的数据，如果不考虑优化空间，正叙和倒序其实都是完全可以的，只是想要优化就必须倒序。

[879. 盈利计划](https://leetcode.cn/problems/profitable-schemes/)：难度2204，过了

[2742. 给墙壁刷油漆](https://leetcode.cn/problems/painting-the-walls/)：这题的难点主要是状态个数的优化，灵神提出了两种思路，但都需要极强的状态个数优化的能力。

​	第一种方法是i表刷墙个数，j和k分别代表有偿刷墙的总时间和无偿刷墙的总时间。但这样三个状态时间复杂度是n的3次方，考虑到最终判断的是j和k的大小关系，直接把他们的差值定义为一个状态变量，这样状态个数大大减少。当然，这么处理边界条件也需要很费脑力的判断，比如

- j >=  i表示`有偿刷墙的总时间-免费刷墙花费的时间` > `所要刷的墙数`，这显然可以令i个墙全部免费刷，都满足要求，这时的开销f(i,j)肯定是0.
- j < i <= 0 表示不符合题目要求，直接返回INT_MAX
- 还有记忆化搜索的缓存数组的定义：`memo(n, vector<int>(n * 2, -1))`，因为j有可能是负的，最小就是-n，即n个墙全是免费，有偿的时间开销是0，上限呢？上限可能你觉得是Σtime[i]，但其实是n。因为当j大于n会怎样？说明`有偿刷墙的总时间-免费刷墙花费的时间` 已经比要刷的墙的最大个数n都大了，这显然算在j > i这种情况中，直接就返回0了。
- 另外由于memo的列是2n，在读取元素时使用j+n来避免数组越界，下标为负。而且很细节的让初始元素全部为-1，-1表示这个位置之前没被计算过。

​	第二种方法是从开销入手。已知有偿花费的时间需要 >= n - 有偿的个数。这涉及3个状态，一个是墙数i，一个是有偿的时间t，一个是有偿的个数c。**如何优化状态个数？**

​	上式中有偿的个数移到左边，`有偿花费的时间+有偿的个数` = Σ(time[i]+1).

​	这一步非常巧妙。

**总结**

​	遇到最后需要==<u>**比较两个状态数值**</u>、<u>**某个状态的值和另一个状态有一一对应关系**</u>==这种情况，都可以考虑能否把两个状态转化为一个。

### 2.完全背包

n个物品变成了n种物品

<img src="images\LeetcodeLearningNotes\image-20250807155438607.png" alt="image-20250807155438607" style="zoom:33%;" />

>  问：关于完全背包，有两种写法，一种是外层循环枚举物品，内层循环枚举体积；另一种是外层循环枚举体积，内层循环枚举物品。如何评价这两种写法的优劣？
>
> 答：两种写法都可以，但更推荐前者。外层循环枚举物品的写法，只会遍历物品数组一次；而内层循环枚举物品的写法，会遍历物品数组多次。从 cache 的角度分析，多次遍历数组会导致额外的 cache miss，带来额外的开销。所以虽然这两种写法的时间空间复杂度是一样的，但外层循环枚举物品的写法常数更小。

这里记忆化搜索方程中原本0-1背包中的dfs(i - 1, c - w[i])变成了dfs(i, c - w[i])

```
dfs(i, c) = max(dfs(i - 1, c), dfs(i, c - w[i]) + v[i])
```

举个例子就能理解：

​	假设背包容量是 `c=10`，第 `i=3` 个物品的重量是 `w[3]=2`，用了它一次，容量变成 `8`，此时你还可以再用一次这个物品，那就变成：

```
dfs(3, 10) → dfs(3, 8) → dfs(3, 6) → ...
```

[279. 完全平方数](https://leetcode.cn/problems/perfect-squares/)：秒了

[1449. 数位成本和为目标值的最大数字](https://leetcode.cn/problems/form-largest-integer-with-digits-that-add-up-to-target/)：过了



### 3.多重背包(选做)



### 4.分组背包



### 5.树形背包(选做)



## 四、经典线性DP

### 1.最长公共子序列(LCS)

![image-20250808213137895](images\LeetcodeLearningNotes\image-20250808213137895.png)

简化：在si和tj都选时，只需要选i-1，j-1即可。在si或tj不选时，无需考虑i-1，j-1

<img src="images\LeetcodeLearningNotes\image-20250808214713032.png" alt="image-20250808214713032" style="zoom:50%;" />

递推：

![image-20250808215115702](images\LeetcodeLearningNotes\image-20250808215115702.png)

​	可以看到fij其实只需要知道它的左边、上面、左上，这三个相邻位置的状态。

<img src="images\LeetcodeLearningNotes\image-20250808215539942.png" alt="image-20250808215539942" style="zoom:50%;" />

​	写一个临时变量存储一下即可解决。

[1143. 最长公共子序列](https://leetcode.cn/problems/longest-common-subsequence/)：模板题

[72. 编辑距离](https://leetcode.cn/problems/edit-distance/)：

​	给定两个字符串 `s` 和 `t`，你要把 `s` 变成 `t`，可以做的操作有：

1. **插入一个字符**
2. **删除一个字符**
3. **替换一个字符**

​	目标是：最少需要多少步操作。

![image-20250808222837026](images\LeetcodeLearningNotes\image-20250808222837026.png)

状态定义：

```
dfs(i, j) = 把 s[0..i] 转换成 t[0..j] 的最小操作数
```

- 当s[i] == t[j]：两个子串的最后一个字母相同，那<u>**这对字符可以保留，不需要额外操作**</u>。问题等价于：`dfs(i, j) = dfs(i-1, j-1)`
- s[i] != t[j]：每种操作都会让步数+1
  - **插入一个字符**：在s的末尾插上`t[j]`，这样s长度变成i+1，但由于这时s和t的末尾都是t[j]，和s[i] == t[j]情况相同，所以只需考虑dfs(i, j-1).但由于是经历了插入操作才使得这样，所以状态是：`dfs(i, j-1) + 1`。
  - **删除一个字符**：在s的末尾删除`s[i]`，这样变成了考虑把 `s[0..i-1]` 转换成 `t[0..j]`消耗的最小步数，因为只要 `s[0..i-1]` 转换成 `t[0..j]`后，再多加一步删掉s[i]，这俩就相同了。以状态是：`dfs(i-1, j) + 1`。
  - **替换一个字符**：把s的末尾换成`t[j]`，此时s和t的末尾都是t[j]，和s[i] == t[j]情况相同，所以只需考虑dfs(i-1, j-1).但由于是经历了插入操作才使得这样，所以状态是：`dfs(i-1, j-1) + 1`。

注意边界条件：

<img src="images\LeetcodeLearningNotes\image-20250808223201451.png" alt="image-20250808223201451" style="zoom:50%;" />

> 这里的i和j都代表下标，表示 s[0...i]和 t[0...j]。注意不是前i个！

### 2.最长递增子序列（LIS）

子集型回溯：得出两种方式

![](images\LeetcodeLearningNotes\image-20250808224935742-1754664576502-3.png)

如果按照思路1，选了3之后，要记下来下标是j，然后移动i，判断i和j指向的数的大小关系；如果按照思路2，选了3之后，只要判断是否比3小，如果比3小那就更改j，指向这个比3小的。

<img src="images\LeetcodeLearningNotes\image-20250808225422545.png" alt="image-20250808225422545" style="zoom:33%;" />

![image-20250808225439066](images\LeetcodeLearningNotes\image-20250808225439066.png)

​	由于选作nums[i]成为末尾最后一个，所以求出来j的最大值还要加上1.

<img src="images\LeetcodeLearningNotes\image-20250808230106332.png" alt="image-20250808230106332" style="zoom: 50%;" />

​	以上是回溯的解法，下面用dp来优化。

递推表达式如下：<img src="images\LeetcodeLearningNotes\image-20250808230243330.png" alt="image-20250808230243330" style="zoom: 33%;" />

<img src="images\LeetcodeLearningNotes\image-20250808230509626.png" alt="image-20250808230509626" style="zoom:50%;" />

**思路三：**

​	由于把一个数组排序去重后，一定是一个严格递增的。因此可以把所给数组排序去重，然后和原先的它本身放在一起求LCS最长公共子序列即是答案。这也是LCS和LIS之间的联系。

<img src="images\LeetcodeLearningNotes\image-20250808230745594.png" alt="image-20250808230745594" style="zoom:40%;" />

## 五、划分型DP

### 1.判定能否划分

[139. 单词拆分](https://leetcode.cn/problems/word-break/)：没思路

> **为什么不是完全背包？**完全背包是同一个物品【连续选择】，然后就再也不选这个物品了。本题可以交替选。

**寻找子问题**：

​	例如 s=leetcode，枚举最后一段的长度：

- 长为 1，即子串 e，如果它在 wordDict 中，那么问题变成：能否把 leetcod 划分成若干段，使得每段都在 wordDict 中？
- 长为 2，即子串 de，如果它在 wordDict 中，那么问题变成：能否把 leetco 划分成若干段，使得每段都在 wordDict 中？
- 长为 3，即子串 ode，如果它在 wordDict 中，那么问题变成：能否把 leetc 划分成若干段，使得每段都在 wordDict 中？
- 长为 4，即子串 code，如果它在 wordDict 中，那么问题变成：能否把 leet 划分成若干段，使得每段都在 wordDict 中？
- ……

**状态定义与状态转移方程**

​	根据上面的讨论，定义状态为 dfs(i)，表示能否把前缀 s[:i]（*表示 s[0] 到 s[i−1] 这段子串*）划分成若干段，使得每段都在 wordDict 中。

枚举 s[:i] 最后一段的长度：

- 长为 1，即子串 s[i−1:i]，如果它在 wordDict 中，那么问题变成：能否把前缀 s[:i−1] 划分成若干段，使得每段都在 wordDict 中，即 dfs(i−1)。
- 长为 2，即子串 s[i−2:i]，如果它在 wordDict 中，那么问题变成：能否把前缀 s[:i−2] 划分成若干段，使得每段都在 wordDict 中，即 dfs(i−2)。
- 长为 3，即子串 s[i−3:i]，如果它在 wordDict 中，那么问题变成：能否把前缀 s[:i−3] 划分成若干段，使得每段都在 wordDict 中，即 dfs(i−3)。
- ……

​	设 wordDict 中字符串的最长长度为 maxLen，枚举的上限不超过 maxLen，因为更长的子串必然不在 wordDict 中。

​	枚举 j=i−1, i−2, i−3, …, max(i−maxLen,0)，<u>**只要其中一个 j 满足**</u> s[j:i] 在 wordDict 中且 dfs(j)=true，那么 dfs(i) 就是 true。

​	因为j是倒序，所以i要正序。当i等于0时，dfs(0)等于0

![image-20250809003149144](images\LeetcodeLearningNotes\image-20250809003149144.png)

[2369. 检查数组是否存在有效划分](https://leetcode.cn/problems/check-if-there-is-a-valid-partition-for-the-array/)：过了

### 2.最优划分

[132. 分割回文串 II](https://leetcode.cn/problems/palindrome-partitioning-ii/)：过了，空间复杂度非常低，遍历i的同时，枚举尾串的长度，即j从i-1一直到0，每次都要比较一下f[i]的最小值，中间不要break，必须遍历完。

### 3.约束划分个数

[1335. 工作计划的最低难度](https://leetcode.cn/problems/minimum-difficulty-of-a-job-schedule/)：过了

## 六、状态机DP

### 1.买卖股票

121. 买卖股票的最佳时机 交易一次
122. 买卖股票的最佳时机 II 交易次数不限
123. 买卖股票的最佳时机 III 交易两次
188. 买卖股票的最佳时机 IV 交易 k 次
3573. 买卖股票的最佳时机 V ~1700 交易 k 次
309. 买卖股票的最佳时机含冷冻期
714. 买卖股票的最佳时机含手续费



​	在某一天卖出股票收益是正，啥也不做是0，买入是负

<img src="images\LeetcodeLearningNotes\image-20250809195207779.png" alt="image-20250809195207779" style="zoom: 50%;" />

请注意这个等价关系：<u>**第i-1天的结束就是第i天的开始**</u>

<img src="images\LeetcodeLearningNotes\image-20250809205904577.png" alt="image-20250809205904577" style="zoom:33%;" />

<img src="images\LeetcodeLearningNotes\image-20250809210339877.png" alt="image-20250809210339877" style="zoom:33%;" />

#### 最多交易k次

再添加一个参数j表示最多交易的笔数，注意是最多，没达到也满足。

在状态转移时，买入和卖出选择其中一个是j-1，另一个是j，因为买卖才算交易一次。

![image-20250809215636723](images\LeetcodeLearningNotes\image-20250809215636723.png)

​	要注意在递推时，j的值最大不是k，而是k+1.原因是：

​	`f[0][j][0]`表示天数为0，交易笔数最多是j，且当前手里没股票的最大利润，显然是0，相当于完全没有在进行交易。`f[0][0][0]`这是合法的，那考虑j = -1呢？此时`f[0][-1][0]`需要等于负无穷，因为虽然天数可以是0，也可以不在交易，但最多j笔的交易条件，边界其实是j=-1，这种情况下没有符合条件的情况，因为交易笔数不可能等于-1。但由于数组下标不能为-1，所以需要给j的上限加1。j=0的情况其实对应的是最多-1笔交易，那么最多k笔就对应j = k + 1.

> 这里i的边界怎么看？为什么i这里0就是边界而不是-1？
>
> 因为原数组的下标从0开始，边界条件是下标为-1，这显然不行，所以我们让i集体加1，i就是从0开始。这可以解释为<u>在0天中进行交易</u>，易于理解，但本质是对应的下标数+1，j的话没有这样类似的解释，只能理解成集体+1，所以稍显抽象。

#### **至少交易k次**

​	值得注意的是当问的是交易笔数至少为k时，递推时i的边界条件依旧是0，但j的边界条件可以是0，因为<u>**在0天中交易笔数至少为0和至少为-1**</u>是一个意思，都是相当于可以无限次交易，所以j无需都加1.`f[0][0][0]` 代表0天中至少交易0次，那就等于没有交易，`f[0][0][0]` = 0。`f[0][j][0]`都是负无穷，因为0天不可能交易次数大于0.

#### 恰好交易k次

​	由于边界条件是交易笔数恰好为-1，所以j也需要全部加1，最终`f[0][1][0]`=0，这对应的是交易笔数恰好为0，其余初始化都是负无穷。

### 2.基础

[3259. 超级饮料的最大强化能量](https://leetcode.cn/problems/maximum-energy-boost-from-two-drinks/)：秒了

### 3.进阶

[1537. 最大得分](https://leetcode.cn/problems/get-the-maximum-score/)：过了。

# 常用数据结构

## 零、枚举

### 0.1 枚举右，维护左

​	这里的右指的是下标偏右边的，并不是大小什么的。

![image-20250820004752964](images\LeetcodeLearningNotes\image-20250820004752964.png)

![image-20250820004820033](images\LeetcodeLearningNotes\image-20250820004820033.png)

### 0.2 枚举中间

![image-20250820004907686](images\LeetcodeLearningNotes\image-20250820004907686.png)

## 一、前缀和

### 1.1 基础

### 1.2 前缀和与哈希表

[560. 和为 K 的子数组](https://leetcode.cn/problems/subarray-sum-equals-k/)：

<img src="images\LeetcodeLearningNotes\image-20250820012303722.png" alt="image-20250820012303722" style="zoom:33%;" />

求出前缀和之后，再对前缀和使用一次枚举右维护左，从而算出答案。

### 1.3 距离和

[2602. 使数组元素全部相等的最少操作次数](https://leetcode.cn/problems/minimum-operations-to-make-all-array-elements-equal/)： 

<img src="images\LeetcodeLearningNotes\image-20250821003005423.png" alt="image-20250821003005423" style="zoom: 33%;" />

### 1.4 前缀异或和

### 1.5 其它一维前缀和

### 1.6 二维前缀和

[304. 二维区域和检索 - 矩阵不可变](https://leetcode.cn/problems/range-sum-query-2d-immutable/)：秒了

## 二、差分

差分与前缀和的关系，类似导数与积分的关系。

数组 *a* 的差分的前缀和就是数组 *a*。

### 2.1 一维差分（扫描线）

![image-20250821163642347](images\LeetcodeLearningNotes\image-20250821163642347.png)

[1094. 拼车](https://leetcode.cn/problems/car-pooling/)：

![image-20250821164253843](images\LeetcodeLearningNotes\image-20250821164253843.png)

![image-20250821164307349](images\LeetcodeLearningNotes\image-20250821164307349.png)

[1589. 所有排列中的最大和](https://leetcode.cn/problems/maximum-sum-obtained-of-any-permutation/)：秒了

### 2.2 二维差分

[2132. 用邮票贴满网格图](https://leetcode.cn/problems/stamping-the-grid/)：难度2364

![image-20250821183222685](images\LeetcodeLearningNotes\image-20250821183222685.png)

先求二维前缀和，如果某个矩阵的和为0，代表能放邮票。本来要给每个格子设置一个cnt，然后++，但用二维差分替代，之后算出来每个小格子上有多少张邮票，如果某个格子上面0张，直接返回flase。

![image-20250821183610534](images\LeetcodeLearningNotes\image-20250821183610534.png)

## 三、栈

### 3.1 基础

[3412. 计算字符串的镜像分数](https://leetcode.cn/problems/find-mirror-score-of-a-string/)：秒了

### 3.5 表达式解析

[1096. 花括号展开 II](https://leetcode.cn/problems/brace-expansion-ii/)：用到编译器的知识，语法分析

## 五、堆（优先队列）

原地堆化的空间复杂度是o（1），时间复杂度是O（n）

在c++中可以直接调用：

```c++
make_heap(gifts.begin(), gifts.end()); // 原地堆化（最大堆）
// gifts是一个vector<int>
```

![image-20250822220543102](images\LeetcodeLearningNotes\image-20250822220543102.png)

**原地堆化的原理：**

​	其实最大堆就是满足根节点最大且局部有序（每个节点都比它的左右孩子大）。下标 ≥ `n/2` 的节点一定没有孩子（它们是叶子），叶子本身就是堆，无需处理。n/2-1是最后一个非叶子节点的下标，只需要处理前面这些即可。倒序遍历，从而保证i的左右子树一定是堆。

​	数组存堆，其实就是数组存完全二叉树，如果父节点下标是i，左孩子下标 = `2*i + 1`，右孩子下标 = `2*i + 2`。如果孩子节点下标是j，它的父节点下标是 `(j - 1) / 2`。

**下沉：**

<img src="images\LeetcodeLearningNotes\image-20250822221956632.png" alt="image-20250822221956632" style="zoom:50%;" />

<img src="images\LeetcodeLearningNotes\image-20250822222037994.png" alt="image-20250822222037994" style="zoom:50%;" />

​	因为如果发生了交换，则原本的根节点被换到下面去了，它是小于子节点的，它来到原先子节点的位置，那有可能不满足比左右子孩子都大的条件，所以还需继续下沉；原先的另一个子孩子由于没有发生交换，又因为是倒序处理，所以它已经是满足条件的，不用动。

### 5.1 基础

[1882. 使用服务器处理任务](https://leetcode.cn/problems/process-tasks-using-servers/)：要用两个堆，注意堆的创建可以用priority_queue

### 5.2 进阶

[1353. 最多可以参加的会议数目](https://leetcode.cn/problems/maximum-number-of-events-that-can-be-attended/)：第一种方法是枚举左，维护右+最小堆

第二种方法是枚举右+并查集

### 5.3 重排元素

[3081. 替换字符串中的问号使分数最小](https://leetcode.cn/problems/replace-question-marks-in-string-to-minimize-its-value/)：写出来了，但第二种方法贪心能把时间复杂度从Onlogn打到On，但没看懂。

## 六、字典树trie

初始化：创建一棵 26 叉树，一开始只有一个根节点 root。26 叉树的每个节点包含一个长为 26 的儿子节点列表 son，以及一个布尔值 end，表示是否为终止节点。

```c++
struct Node {
    Node* son[26]{};
    bool end = false;
};
```



### 6.1 基础

[208. 实现 Trie (前缀树)](https://leetcode.cn/problems/implement-trie-prefix-tree/)：

## 七、并查集

首先明白连通块的定义，连通块是极大连通子图：

- 在这个子图里的任意两个点，都存在一条路径相连。
- 再往外加任何一个新点，就不再满足条件。

​	并查集这种数据结结构中需要两个数组和一个整型变量。比如有n个节点，需要一个数组father表示<u>**i的父节点（代表元）**</u>，和一个sz数组表示**<u>以i为根的集合大小</u>**。一个int变量cc表示当前连通块个数（集合个数）。

​	初始化，把father填充成{0，1，2，3，4，...}，即每个元素一开始的父节点是自己。sz每个元素都是1。cc的值为n，每个元素都是一个连通块。

<img src="images\LeetcodeLearningNotes\image-20250827153606784.png" alt="image-20250827153606784" style="zoom: 33%;" />
![image-20250827153632704](images\LeetcodeLearningNotes\image-20250827153632704.png)

### 7.1 基础

[990. 等式方程的可满足性](https://leetcode.cn/problems/satisfiability-of-equality-equations/)：秒了

### 7.2 进阶

### 7.3 GCD并查集

[2709. 最大公约数遍历](https://leetcode.cn/problems/greatest-common-divisor-traversal/)：2172 分解质因数 + 并查集

[1998. 数组的最大公因数排序](https://leetcode.cn/problems/gcd-sort-of-an-array/)：2429 将近一个小时拿下

### 7.4 数组上的并查集

[1353. 最多可以参加的会议数目](https://leetcode.cn/problems/maximum-number-of-events-that-can-be-attended/)：旧题重写

[2334. 元素值大于变化阈值的子数组](https://leetcode.cn/problems/subarray-with-elements-greater-than-varying-threshold/)：

![image-20250828002709669](images\LeetcodeLearningNotes\image-20250828002709669.png)

​	并查集解法本质是统计“**以某个nums[i]为最小值的最大连续子数组的长度**”，所以要把它左边和右边比它大的元素都包含进来。最后还是看不懂

​	最后用单调栈秒了。

### 7.5 区间并查集

[3244. 新增道路查询后的最短距离 II](https://leetcode.cn/problems/shortest-distance-after-road-addition-queries-ii/)：这里“合并”不能从点的层面思考，而是要从边的层面。比如2，4就相当于把2到3和3到4这两条边合并了。

​	在合并上学到了新的细节，每次找x所在的这个集合的最右边节点（其实就是find(x)的结果），然后把包含x的这个集合和它右边邻近的集合合在一起，以此类推。

# 网格图

## 一、网格图DFS

​	适用于计算连通块个数、大小的题目。

[200. 岛屿数量](https://leetcode.cn/problems/number-of-islands/)：向四个方向dfs，把去到的每个格子置为2表示已经去过了。

[695. 岛屿的最大面积](https://leetcode.cn/problems/max-area-of-island/)：秒了

[1559. 二维网格图中探测环](https://leetcode.cn/problems/detect-cycles-in-2d-grid/)：难度1838：

## 二、网格图BFS

​	适用于需要计算最短距离（最短路）的题目。

**多源BFS：**

​	如果图上有 **多个起点**，想要求 **每个点到最近的起点的距离**，这时要用多源BFS。

[1162. 地图分析](https://leetcode.cn/problems/as-far-from-land-as-possible/)：多源bfs，秒了

[749. 隔离病毒](https://leetcode.cn/problems/contain-virus/)：难度2277

主函数中，

​	while(true)一直循环，其实相当于模拟天数，里面写一个getCnt函数返回病毒在明天会感染的最大的块数，如果返回值是0那么说明第二天没有块被感染，循环终止。

getCnt中，

- 先初始化一个大小和网格一样的bool矩阵，表示格子是否被访问，初始全是false。
- 准备两个哈希集合，一个l1存放各个“岛屿”的坐标，一个l2存放每个“岛屿”第二天要感染的格子的坐标，==所有的坐标都用x*m+y进行二维转一维==。
- 在整个网格中循环，只有遇到1和未访问过的格子时，调用search函数寻找这个岛屿要加的防火墙的数量。
- 每次search完之后，看一下对明天的威胁是不是最大，如果是最大，把防火墙个数对应更新一下。
- 整个矩阵遍历完时，开始当天的感染和围防火墙工作。这一步非常巧妙！！遍历整个l1和l2，每次遍历设置一个bool  block = false，如果某个l2的s2的长度是刚才记录的第二天最大要感染数，那就让block = true。这时不从l2中取，因为这个岛屿要围起来，所以从s1中取数，取完拆成坐标，然后把这个格子置为2表示已经被防护；否则就从s2中取数，把对应格子给设为1，表示被感染。

```c++
for (int i = 0; i < l1.size(); ++i) {
	bool block = false;
	if (l2[i].size() == maxl) block true;
	for (auto& it : block ? l1 : l2) {
		int x = it / m;
		int y = it % m;
		grid[x][y] = block ? 2 : 1;
	}
}
```

- 返回第二天感染的最大威胁数。

search中，

- BFS寻找明天要感染的格子，逐渐寻找“岛屿”的边界，并把访问过的格子都设置为true。
- 尤其要注意==加防火墙的个数**不等于**明天要感染的格子数==！！因为有可能一个格子的好几条边都加防火墙。所以用哈希集合的作用显现了，当遇到某个（nx，ny）是0，把xm+y放入集合自动去重，防火墙的个数无脑+1.

## 三、网格图 0-1 BFS

[1368. 使网格图至少有一条有效路径的最小代价](https://leetcode.cn/problems/minimum-cost-to-make-at-least-one-valid-path-in-a-grid/)：过了，要用双端队列deque

## 四、网格图迪杰斯特拉

### 先搞清楚迪杰斯特拉在求什么？

​	从给定的源点 `start` 出发，求 **源点到图中每个节点的最短距离**。

### 核心思想是什么？

​	每次从未处理的节点中选择“距离源点最近”的节点扩展。

​	已处理的节点的最短路径已经确认，不会再被修改

### 堆顶元素代表什么？

​	堆里放的是：`{候选距离, 节点编号}`，由于是小顶堆，每次弹出的距离和节点，保证了源点到它的路径最短。

### 具体过程

![image-20250824221657567](images\LeetcodeLearningNotes\image-20250824221657567.png)

​	所以g[i]表示以i为起始点，堆中放的是终点和最短路径。

<img src="images\LeetcodeLearningNotes\image-20250824221941985.png" alt="image-20250824221941985" style="zoom:33%;" />

![image-20250824222253111](images\LeetcodeLearningNotes\image-20250824222253111.png)

优先队列中，放的是一堆pair，first代表起点到x的最短路径，second代表x

> 起点start作为函数给的参数传进来，是已知量

从现在开始记住一点，我们的最终目标是取更新dis这个数组！也就是尽量去更新dis[i]的值。

![image-20250824223518466](images\LeetcodeLearningNotes\image-20250824223518466.png)

### **为什么dis_x > dis[x]代表之前出堆过?为什么出堆过就不用再维护dis[x]了？**

​	假设从start到x先有个最短路径是10，dis[x]更新成10，pq.emplace(10,x)会把10丢进堆；后面又来了个到x的最短路径6，那么dis[x]更新成6，(6,x)也被扔进堆。	那么在出堆时，由于是小顶堆，(6,x)一定先出堆。待到(10,x)后出堆时，这时即触发dis_x > dis[x]，因为10>6，这时显然就代表x之前出堆过。

​	至于为什么出堆过就不用再维护，这个问题这样考虑：当源点到x的路径长度超过一众其它点，从堆顶中被pop出来，哪怕源点能经过别的点再到x(比如start-> n -> x)，那也不可能比当前的短了。

​	总之，反直觉的一点是：dis[i]的值是先更新成最小，再把(dis_i, i)扔入堆中，理解这点至关重要。

### 为什么dis[i]的值都已经更新成最小了，还要再把(new_dis_i,i)扔进堆中？

​	因为当dis[i]的值发生变化时，如果<u>**i作为源点和其它节点之间的点，它会影响到别的节点的结果**</u>，就要及时入堆。

​	如果堆中正好还有(dis_i, i)（旧的，比现在的大），那新放入的i就会**<u>“超车”</u>**这个旧的，等到出堆时发现等于dis[i]，被拿来处理跟i有关系的节点。

​	等到之前旧的，相对更大的i出堆时，发现比dis[i]大，于是之间continue。

---

[1631. 最小体力消耗路径](https://leetcode.cn/problems/path-with-minimum-effort/)：拿下。

## 五、综合应用

[1263. 推箱子](https://leetcode.cn/problems/minimum-moves-to-move-a-box-to-their-target-location/)：太难了，BFS中进行DFS，还有一堆细节需要处理好。

# 图论

## 一、基础遍历

### 1.1 DFS基础

[2192. 有向无环图中一个节点的所有祖先](https://leetcode.cn/problems/all-ancestors-of-a-node-in-a-directed-acyclic-graph/)：秒了。

[2092. 找出知晓秘密的所有专家](https://leetcode.cn/problems/find-all-people-with-secret/)：这个建图不难

### 1.2 BFS基础

[815. 公交路线](https://leetcode.cn/problems/bus-routes/)：建图的下标是每个站台，里面放的是路过这个站台的公交车下标，这点很妙。

[2608. 图中的最短环](https://leetcode.cn/problems/shortest-cycle-in-a-graph/)：过了

## 二、拓扑排序

前提条件：图中无环，主要是针对**有向无环图DAG**

最终排序的结果是：

<img src="images\LeetcodeLearningNotes\1738131168-tWFNGZ-006-toposort.png" alt="图论题单 图论算法 图论题目 LeetCode 力扣图论 灵茶山艾府" style="zoom:33%;" />

每条边都是：

​	从排在前面的点，指向排在后面的点。即对于任意有向边 *x*→*y*，*x* 一定在 *y* 之前。

<img src="images\LeetcodeLearningNotes\image-20250825173432627.png" alt="image-20250825173432627" style="zoom:50%;" /><img src="images\LeetcodeLearningNotes\image-20250825173455806.png" alt="image-20250825173455806" style="zoom:50%;" />

### 2.1 拓扑排序

[1591. 奇怪的打印机 II](https://leetcode.cn/problems/strange-printer-ii/)：2291难度，秒了。

[802. 找到最终的安全状态](https://leetcode.cn/problems/find-eventual-safe-states/)：秒了。

### 2.2 在拓扑序上DP

### 2.3 基环树

​	分为两种情况，第一种是基环大于2：

<img src="images\LeetcodeLearningNotes\1641096462-IsWZUX-1.png" style="zoom: 50%;" />

​	这种情况直接就是取环的大小。哪怕链再长也没用，比如2后面还有5，6，7，8，这样8到7到6到5到2到1，看着长度是5，但1喜欢的0不在其中，1是不会来参加会议的。

​	第二种是基环等于2，也就是只有两个节点：

<img src="images\LeetcodeLearningNotes\1641096473-JtGBgY-3.png" alt="3.png" style="zoom:50%;" />

​	假设0和1先坐下，0在左1在右，0在左边还可以接着坐2，4，5，也就是找0反图最长的链，同理1也是这样操作，找到的是6。其实5和6不用有什么关系

​	如果有多个基环长度等于2的基环树，可以把每个基环树所对应的链拼在其余链的末尾。

​	综上，问题演化为找给定的图中的基环树（两种不同情况的）。

![image-20250825234930557](images\LeetcodeLearningNotes\image-20250825234930557.png)

<img src="images\LeetcodeLearningNotes\image-20250826000408422.png" alt="image-20250826000408422" style="zoom:50%;" />
<img src="images\LeetcodeLearningNotes\image-20250826000547134.png" alt="image-20250826000547134" style="zoom:50%;" />
![image-20250826000511433](images\LeetcodeLearningNotes\image-20250826000511433.png)

[2127. 参加会议的最多员工数](https://leetcode.cn/problems/maximum-employees-to-be-invited-to-a-meeting/)：太难了

## 三、最短路

### 3.1 单源最短路：dijkstra

[1976. 到达目的地的方案数](https://leetcode.cn/problems/number-of-ways-to-arrive-at-destination/)：



## 四、最小生成树

