# C语言break和continue用法详解（跳出循环）

使用while或[for循环](http://c.biancheng.net/view/172.html)时，如果想提前结束循环（在不满足结束条件的情况下结束循环），可以使用break或continue关键字。

## break关键字

在《[C语言switch case语句](http://c.biancheng.net/view/1808.html)》一节中，我们讲到了break，用它来跳出 switch 语句。

当 break 关键字用于 while、[for 循环](http://c.biancheng.net/view/172.html)时，会终止循环而执行整个循环语句后面的代码。break 关键字通常和 if 语句一起使用，即满足条件时便跳出循环。

使用 [while 循环](http://c.biancheng.net/view/180.html)计算1加到100的值：

```c
#include <stdio.h>
int main(){
    int i=1, sum=0;
    while(1){  //循环条件为死循环
        sum+=i;
        i++;
        if(i>100) break;
   }
    printf("%d\n", sum);
    return 0;
}
```

运行结果：
5050

while 循环条件为 1，是一个死循环。当执行到第100次循环的时候，计算完`i++;`后 i 的值为 101，此时 if 语句的条件 i> 100 成立，执行`break;`语句，结束循环。

在多层循环中，一个 break 语句只向外跳一层。例如，输出一个4*4的整数矩阵：

```c
#include <stdio.h>
int main(){
    int i=1, j;
    while(1){  // 外层循环
        j=1;
        while(1){  // 内层循环
            printf("%-4d", i*j);
            j++;
            if(j>4) break;  //跳出内层循环
        }
        printf("\n");
        i++;
        if(i>4) break;  // 跳出外层循环
    }
    return 0;
}
```

运行结果：

```
1   2   3   4
2   4   6   8
3   6   9   12
4   8   12  16
```

当 j>4 成立时，执行`break;`，跳出内层循环；外层循环依然执行，直到 i>4 成立，跳出外层循环。内层循环共执行了4次，外层循环共执行了1次。

## continue语句

continue 语句的作用是跳过循环体中剩余的语句而强制进入下一次循环。continue语句只用在 while、for 循环中，常与 if 条件语句一起使用，判断条件是否成立。

来看一个例子：

```c
#include <stdio.h>
int main(){
    char c = 0;
    while(c!='\n'){  //回车键结束循环
        c=getchar();
        if(c=='4' || c=='5'){  //按下的是数字键4或5
            continue;  //跳过当次循环，进入下次循环
        }
        putchar(c);
    }
    return 0;
}
```

运行结果：
0123456789↙
01236789

程序遇到while时，变量c的值为`'\0'`，循环条件`c!='\n'`成立，开始第一次循环。getchar() 使程序暂停执行，等待用户输入，直到用户按下回车键才开始读取字符。

本例我们输入的是 0123456789，当读取到4或5时，if 的条件`c=='4'||c=='5'`成立，就执行 continue 语句，结束当前循环，直接进入下一次循环，也就是说`putchar(c);`不会被执行到。而读取到其他数字时，if 的条件不成立，continue 语句不会被执行到，`putchar(c);`就会输出读取到的字符。

break与continue的对比：break 用来结束所有循环，循环语句不再有执行的机会；continue 用来结束本次循环，直接跳到下一次循环，如果循环条件成立，还会继续循环。