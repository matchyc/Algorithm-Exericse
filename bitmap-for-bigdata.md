# Bigmap solution for big data question

Normally an integer is represented as 4 bytes in computer's memory.

> 英文写起来磕磕绊绊 可恶啊 还是中文吧

怎么说呢，就是一般情况下用一个 bit 代替一个整数。

换句话说，我们使用一个 int 数组 `int array[2]`, 有两个元素，共 8 个字节、32bit。 如果我们使用一个 bit 代替一个数字的话，我们就能够使得`array`这个数组表示 0-31 的数字。（注意从 0 开始）。

试想，如果有 40 亿个数，我们要找到没有出现的数字，（范围 0~10 亿-1），如果用整数存储，需要 $40 * 10^{9} * 4 bytes \approx 14.9GB$，但是我们如果用上述方法，就只需要 $\frac{40 * 10^{9}}{8*2^{10}*2^{10}} \approx 476 MB$.

效果显而易见，就是**压缩了存储**。

如果我们是一位表示一个数，那么我们可以用这样的方式完成 bit 位的设置：
```c
int array[10] = {};
const int SHIFT = 3;
const int MASK = 7;
void set(int* arr, int x) {
    // arr[x/8] |= (1 << (x % 8))  ‘|=’ 位运算
    // arr[x << SHIFT] |= (1 << (x & MASK)) 模 8 等于 & 7
}
```




Reference
>https://www.cnblogs.com/moonandstar08/p/5236539.html