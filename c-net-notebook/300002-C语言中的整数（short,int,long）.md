# C语言中的整数（short,int,long）

整数是编程中常用的一种数据，C语言通常使用`int`来定义整数（int 是 integer 的简写），这在《[大话C语言变量和数据类型](http://c.biancheng.net/view/1756.html)》中已经进行了详细讲解。

在现代操作系统中，int **一般**占用 4 个字节（Byte）的内存，共计 32 位（Bit）。如果不考虑正负数，当所有的位都为 1 时它的值最大，为 232-1 = 4,294,967,295 ≈ 43亿，这是一个很大的数，实际开发中很少用到，而诸如 1、99、12098 等较小的数使用频率反而较高。

使用 4 个字节保存较小的整数绰绰有余，会空闲出两三个字节来，这些字节就白白浪费掉了，不能再被其他数据使用。现在个人电脑的内存都比较大了，配置低的也有 2G，浪费一些内存不会带来明显的损失；而在C语言被发明的早期，或者在单片机和嵌入式系统中，内存都是非常稀缺的资源，所有的程序都在尽力节省内存。

反过来说，43 亿虽然已经很大，但要表示全球人口数量还是不够，必须要让整数占用更多的内存，才能表示更大的值，比如占用 6 个字节或者 8 个字节。

让整数占用更少的内存可以在 int 前边加 **short**，让整数占用更多的内存可以在 int 前边加 **long**，例如：

short int a = 10;
short int b, c = 99;
long int m = 102023;
long int n, p = 562131;

这样 a、b、c 只占用 2 个字节的内存，而 m、n、p **可能**会占用 8 个字节的内存。

也可以将 int 省略，只写 short 和 long，如下所示：

short a = 10;
short b, c = 99;
long m = 102023;
long n, p = 562131;

这样的写法更加简洁，实际开发中常用。

int 是基本的整数类型，short 和 long 是在 int 的基础上进行的扩展，short 可以节省内存，long 可以容纳更大的值。

short、int、long 是C语言中常见的整数类型，其中 int 称为整型，short 称为短整型，long 称为长整型。

## 整型的长度

细心的读者可能会发现，上面我们在描述 short、int、long 类型的长度时，只对 short 使用肯定的说法，而对 int、long 使用了“一般”或者“可能”等不确定的说法。这种描述的言外之意是，只有 short 的长度是确定的，是两个字节，而 int 和 long 的长度无法确定，在不同的环境下有不同的表现。

> 一种数据类型占用的字节数，称为该数据类型的长度。例如，short 占用 2 个字节的内存，那么它的长度就是 2。

实际情况也确实如此，C语言并没有严格规定 short、int、long 的长度，只做了宽泛的限制：

- short 至少占用 2 个字节。
- int 建议为一个机器字长。32 位环境下机器字长为 4 字节，64 位环境下机器字长为 8 字节。
- short 的长度不能大于 int，long 的长度不能小于 int。


总结起来，它们的长度（所占字节数）关系为：

2 ≤ short ≤ int ≤ long

这就意味着，short 并不一定真的”短“，long 也并不一定真的”长“，它们有可能和 int 占用相同的字节数。

在 16 位环境下，short 的长度为 2 个字节，int 也为 2 个字节，long 为 4 个字节。16 位环境多用于单片机和低级嵌入式系统，在PC和服务器上已经见不到了。

对于 32 位的 Windows、Linux 和 Mac OS，short 的长度为 2 个字节，int 为 4 个字节，long 也为 4 个字节。PC和服务器上的 32 位系统占有率也在慢慢下降，嵌入式系统使用 32 位越来越多。

在 64 位环境下，不同的操作系统会有不同的结果，如下所示：

| 操作系统                                                | short | int  | long |
| ------------------------------------------------------- | ----- | ---- | ---- |
| Win64（64位 Windows）                                   | 2     | 4    | 4    |
| 类Unix系统（包括 Unix、Linux、Mac OS、BSD、Solaris 等） | 2     | 4    | 8    |


目前我们使用较多的PC系统为 Win XP、Win 7、Win 8、Win 10、Mac OS、Linux，在这些系统中，short 和 int 的长度都是固定的，分别为 2 和 4，大家可以放心使用，只有 long 的长度在 Win64 和类 Unix 系统下会有所不同，使用时要注意移植性。

## sizeof 操作符

获取某个数据类型的长度可以使用 sizeof 操作符，如下所示：

```c
#include <stdio.h>
int main()
{
    short a = 10;
    int b = 100;
   
    int short_length = sizeof a;
    int int_length = sizeof(b);
    int long_length = sizeof(long);
    int char_length = sizeof(char);
   
    printf("short=%d, int=%d, long=%d, char=%d\n", short_length, int_length, long_length, char_length);
   
    return 0;
}
```

在 32 位环境以及 Win64 环境下的运行结果为：

short=2, int=4, long=4, char=1

在 64 位 Linux 和 Mac OS 下的运行结果为：

short=2, int=4, long=8, char=1


sizeof 用来获取某个数据类型或变量所占用的字节数，如果后面跟的是变量名称，那么可以省略`( )`，如果跟的是数据类型，就必须带上`( )`。

需要注意的是，sizeof 是C语言中的操作符，不是函数，所以可以不带`( )`，后面会详细讲解。

## 不同整型的输出

使用不同的格式控制符可以输出不同类型的整数，它们分别是：

- `%hd`用来输出 short int 类型，hd 是 short decimal 的简写；
- `%d`用来输出 int 类型，d 是 decimal 的简写；
- `%ld`用来输出 long int 类型，ld 是 long decimal 的简写。


下面的例子演示了不同整型的输出：

```c
#include <stdio.h>
int main()
{
    short a = 10;
    int b = 100;
    long c = 9437;
    printf("a=%hd, b=%d, c=%ld\n", a, b, c);
    return 0;
}
```

运行结果：
a=10, b=100, c=9437

在编写代码的过程中，我建议将格式控制符和数据类型严格对应起来，养成良好的编程习惯。当然，如果你不严格对应，一般也不会导致错误，例如，很多初学者都使用`%d`输出所有的整数类型，请看下面的例子：

```c
#include <stdio.h>
int main()
{
    short a = 10;
    int b = 100;
    long c = 9437;
   
    printf("a=%d, b=%d, c=%d\n", a, b, c);
    return 0;
}
```

运行结果仍然是：
a=10, b=100, c=9437

当使用`%d`输出 short，或者使用`%ld`输出 short、int 时，不管值有多大，都不会发生错误，因为格式控制符足够容纳这些值。

当使用`%hd`输出 int、long，或者使用`%d`输出 long 时，如果要输出的值比较小（就像上面的情况），一般也不会发生错误，如果要输出的值比较大，就很有可能发生错误，例如：

```c
#include <stdio.h>
int main()
{
    int m = 306587;
    long n = 28166459852;
    printf("m=%hd, n=%hd\n", m, n);
    printf("n=%d\n", n);
    return 0;
}
```

在 64 位 Linux 和 Mac OS 下（long 的长度为 8）的运行结果为：
m=-21093, n=4556
n=-1898311220

输出结果完全是错误的，这是因为`%hd`容纳不下 m 和 n 的值，`%d`也容纳不下 n 的值。

读者需要注意，当格式控制符和数据类型不匹配时，编译器会给出警告，提示程序员可能会存在风险。

> 编译器的警告是分等级的，不同程度的风险被划分成了不同的警告等级，而使用`%d`输出 short 和 long 类型的风险较低，如果你的编译器设置只对较高风险的操作发出警告，那么此处你就看不到警告信息。

