---
title: 到底啥是hash
date: 2018-04-15 11:56:34
tags:
---
作为一个程序员，每天都会和`Hash`打交道，比如`HashMap<K,V>`，比如`hashCode()`等等，那么啥是hash，这篇memo就整的明明白白。

> 散列的[wikipedia](https://zh.wikipedia.org/wiki/%E6%95%A3%E5%88%97%E5%87%BD%E6%95%B8)

hash的目的实际上是将原数据空间的数据映射到一个较小的数据空间中，并使得**碰撞**的概率尽量低。如果想要创造一个在原数据空间上完美无碰撞的hash函数，那么这个函数的目标取值空间应该不小于原空间。

由于高级语言中的对象都能够序列化为一个字符串（或者说一个很大的数字），所以大部分的hash研究都是针对平凡字符串的，当然，也有很多领域专用的hash函数，比如一致性hash算法，这些hash函数都是针对一个特定的数据空间和数据分布。

以下是业界比较常用的几种字符串hash:

|Hash函数|数据1|数据2|数据3|数据4|数据1得分|数据2得分|数据3得分|数据4得分|平均分|
|----|----|----|----|----|----|----|----|----|----|
|BKDRHash|2|0|4774|481|96.55|100|90.95|82.05|92.64|
|APHash|2|3|4754|493|96.55|88.46|100|51.28|86.28|
|DJBHash|2|2|4975|474|96.55|92.31|0|100|83.43|
|JSHash|1|4|4761|506|100|84.62|96.83|17.95|81.94|
|RSHash|1|0|4861|505|100|100|51.58|20.51|75.96|
|SDBMHash|3|2|4849|504|93.1|92.31|57.01|23.08|72.41|
|PJWHash|30|26|4878|513|0|0|43.89|0|21.95|
|ELFHash|30|26|4878|513|0|0|43.89|0|21.95|

以上的几种算法中，以K&R提出的BKDRHash函数为例，它大概长这样:
```C
/* BKDR Hash Function */
unsigned int BKDR_hash(char *str) {
    unsigned int seed = 131; // 31 131 1313 13131 131313 etc..
    unsigned int hash = 0;
    while (*str) {
        hash = hash * seed + (*str++);
    }
    return (hash & 0x7FFFFFFF);
}
```
上面这个函数等价于:
<img src="http://chart.googleapis.com/chart?cht=tx&chl=hash = \left ( \sum a\left [ n \right ]s^{n} \right ) mod 2^{31}" style="border:none;">

公式中的`a`是要计算的`str`, 公式中的`s`就是`seed`。而`seed`的取值也十分巧妙，具体的解释可以查看[这个问答](https://www.zhihu.com/question/20507188)。

我们从公式中可以看出，`usigned int` 的值空间是小于str的值空间的，如果我们有能力遍历整个字符串的值空间，那么必定能够找到两个字符串的hash值是相同的。但从概率上讲，能够找到两个字符串的hash值相同的概率非常低，这也就保证了hash值的抗碰撞性。在一定程度上，我们可以认为，如果两个字符串的hash值相同，那么这两个字符串的值也相同。

# MD5

MD5是一种特殊的散列函数，也是将一段信息运算变为另一个固定长度的值，他的伪代码是这样的:
```
//Note: All variables are unsigned 32 bits and wrap modulo 2^32 when calculating
var int[64] r, k

//r specifies the per-round shift amounts
r[ 0..15]：= {7, 12, 17, 22,  7, 12, 17, 22,  7, 12, 17, 22,  7, 12, 17, 22} 
r[16..31]：= {5,  9, 14, 20,  5,  9, 14, 20,  5,  9, 14, 20,  5,  9, 14, 20}
r[32..47]：= {4, 11, 16, 23,  4, 11, 16, 23,  4, 11, 16, 23,  4, 11, 16, 23}
r[48..63]：= {6, 10, 15, 21,  6, 10, 15, 21,  6, 10, 15, 21,  6, 10, 15, 21}

//Use binary integer part of the sines of integers as constants:
for i from 0 to 63
    k[i] := floor(abs(sin(i + 1)) × 2^32)

//Initialize variables:
var int h0 := 0x67452301
var int h1 := 0xEFCDAB89
var int h2 := 0x98BADCFE
var int h3 := 0x10325476

//Pre-processing:
append "1" bit to message
append "0" bits until message length in bits ≡ 448 (mod 512)
append bit length of message as 64-bit little-endian integer to message

//Process the message in successive 512-bit chunks:
for each 512-bit chunk of message
    break chunk into sixteen 32-bit little-endian words w[i], 0 ≤ i ≤ 15

    //Initialize hash value for this chunk:
    var int a := h0
    var int b := h1
    var int c := h2
    var int d := h3

    //Main loop:
    for i from 0 to 63
        if 0 ≤ i ≤ 15 then
            f := (b and c) or ((not b) and d)
            g := i
        else if 16 ≤ i ≤ 31
            f := (d and b) or ((not d) and c)
            g := (5×i + 1) mod 16
        else if 32 ≤ i ≤ 47
            f := b xor c xor d
            g := (3×i + 5) mod 16
        else if 48 ≤ i ≤ 63
            f := c xor (b or (not d))
            g := (7×i) mod 16
 
        temp := d
        d := c
        c := b
        b := leftrotate((a + f + k[i] + w[g]),r[i]) + b
        a := temp
    Next i
    //Add this chunk's hash to result so far:
    h0 := h0 + a
    h1 := h1 + b 
    h2 := h2 + c
    h3 := h3 + d
End ForEach
var int digest := h0 append h1 append h2 append h3 //(expressed as little-endian)
```
如上代码等同于以下的公式:
![公式](https://wikimedia.org/api/rest_v1/media/math/render/svg/29bfebad4e7bdd4a2fc1210694eb5664262faecc)
![公式](https://wikimedia.org/api/rest_v1/media/math/render/svg/7068038702afd55190f991518f3a9188565f32d0)
![公式](https://wikimedia.org/api/rest_v1/media/math/render/svg/c121ed0510b6ad3ffde9b89cec96ff7552ae9236)
![公式](https://wikimedia.org/api/rest_v1/media/math/render/svg/2f119366de7d323f5e02b8d12a741968fa9d0f99)

即使字符串有很小的变化，经过MD5后，字符串的MD5也会有很大的变化。