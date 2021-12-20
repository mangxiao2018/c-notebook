# C语言switch case语句详解

C语言虽然没有限制 [if else](http://c.biancheng.net/c/if_else/) 能够处理的分支数量，但当分支过多时，用 if else 处理会不太方便，而且容易出现 if else 配对出错的情况。例如，输入一个整数，输出该整数对应的星期几的英文表示：

```c
#include <stdio.h>
int main(){
    int a;
    printf("Input integer number:");
    scanf("%d",&a);
    if(a==1){
        printf("Monday\n");
    }else if(a==2){
        printf("Tuesday\n");
    }else if(a==3){
        printf("Wednesday\n");
    }else if(a==4){
        printf("Thursday\n");
    }else if(a==5){
        printf("Friday\n");
    }else if(a==6){
        printf("Saturday\n");
    }else if(a==7){
        printf("Sunday\n");
    }else{
        printf("error\n");
    }
    return 0;
}
```

运行结果：
Input integer number:3↙
Wednesday

对于这种情况，实际开发中一般使用 switch 语句代替，请看下面的代码：

```c
#include <stdio.h>
int main(){
    int a;
    printf("Input integer number:");
    scanf("%d",&a);
    switch(a){
        case 1: printf("Monday\n"); break;
        case 2: printf("Tuesday\n"); break;
        case 3: printf("Wednesday\n"); break;
        case 4: printf("Thursday\n"); break;
        case 5: printf("Friday\n"); break;
        case 6: printf("Saturday\n"); break;
        case 7: printf("Sunday\n"); break;
        default:printf("error\n"); break;
    }
    return 0;
}
```

运行结果：
Input integer number:4↙
Thursday

switch 是另外一种选择结构的语句，用来代替简单的、拥有多个分枝的 [if else 语句](http://c.biancheng.net/c/if_else/)，基本格式如下：

switch(表达式){
  case 整型数值1: 语句 1;
  case 整型数值2: 语句 2;
  ......
  case 整型数值n: 语句 n;
  default: 语句 n+1;
}

它的执行过程是：
\1) 首先计算“表达式”的值，假设为 m。

\2) 从第一个 case 开始，比较“整型数值1”和 m，如果它们相等，就执行冒号后面的所有语句，也就是从“语句1”一直执行到“语句n+1”，而不管后面的 case 是否匹配成功。

\3) 如果“整型数值1”和 m 不相等，就跳过冒号后面的“语句1”，继续比较第二个 case、第三个 case……一旦发现和某个整型数值相等了，就会执行后面所有的语句。假设 m 和“整型数值5”相等，那么就会从“语句5”一直执行到“语句n+1”。

\4) 如果直到最后一个“整型数值n”都没有找到相等的值，那么就执行 default 后的“语句 n+1”。

需要重点强调的是，当和某个整型数值匹配成功后，会执行该分支以及后面所有分支的语句。例如：

```c
#include <stdio.h>
int main(){
    int a;
    printf("Input integer number:");
    scanf("%d",&a);
    switch(a){
        case 1: printf("Monday\n");
        case 2: printf("Tuesday\n");
        case 3: printf("Wednesday\n");
        case 4: printf("Thursday\n");
        case 5: printf("Friday\n");
        case 6: printf("Saturday\n");
        case 7: printf("Sunday\n");
        default:printf("error\n");
    }
    return 0;
}
```

运行结果：
Input integer number:4↙
Thursday
Friday
Saturday
Sunday
error

输入4，发现和第四个分支匹配成功，于是就执行第四个分支以及后面的所有分支。这显然不是我们想要的结果，我们希望只执行第四个分支，而跳过后面的其他分支。为了达到这个目标，必须要在每个分支最后添加`break;`语句。

**break** 是C语言中的一个关键字，专门用于跳出 switch 语句。所谓“跳出”，是指一旦遇到 break，就不再执行 switch 中的任何语句，包括当前分支中的语句和其他分支中的语句；也就是说，整个 switch 执行结束了，接着会执行整个 switch 后面的代码。

使用 break 修改上面的代码：

```c
#include <stdio.h>
int main(){
    int a;
    printf("Input integer number:");
    scanf("%d",&a);
    switch(a){
        case 1: printf("Monday\n"); break;
        case 2: printf("Tuesday\n"); break;
        case 3: printf("Wednesday\n"); break;
        case 4: printf("Thursday\n"); break;
        case 5: printf("Friday\n"); break;
        case 6: printf("Saturday\n"); break;
        case 7: printf("Sunday\n"); break;
        default:printf("error\n"); break;
    }
    return 0;
}
```

运行结果：
Input integer number:4↙
Thursday

由于 default 是最后一个分支，匹配后不会再执行其他分支，所以也可以不添加`break;`语句。

最后需要说明的两点是：
\1) case 后面必须是一个整数，或者是结果为整数的表达式，但不能包含任何变量。请看下面的例子：

```c
case 10: printf("..."); break;  //正确
case 8+9: printf("..."); break;  //正确
case 'A': printf("..."); break;  //正确，字符和整数可以相互转换
case 'A'+19: printf("..."); break;  //正确，字符和整数可以相互转换
case 9.5: printf("..."); break;  //错误，不能为小数
case a: printf("..."); break;    //错误，不能包含变量
case a+10: printf("..."); break;  //错误，不能包含变量
```



\2) default 不是必须的。当没有 default 时，如果所有 case 都匹配失败，那么就什么都不执行。