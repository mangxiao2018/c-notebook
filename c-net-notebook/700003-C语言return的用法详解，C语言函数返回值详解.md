# C语言return的用法详解，C语言函数返回值详解

函数的返回值是指函数被调用之后，执行函数体中的代码所得到的结果，这个结果通过 **return** 语句返回。

return 语句的一般形式为：

```
return 表达式;
```

或者：

```
return (表达式);
```

有没有`( )`都是正确的，为了简明，一般也不写`( )`。例如：

```
return max;
return a+b;
return (100+200);
```

#### 对C语言返回值的说明：

\1) 没有返回值的函数为空类型，用`void`表示。例如：

```c
void func(){
    printf("http://c.biancheng.net\n");
}
```

一旦函数的返回值类型被定义为 void，就不能再接收它的值了。例如，下面的语句是错误的：

```
int a = func();
```

为了使程序有良好的可读性并减少出错， 凡不要求返回值的函数都应定义为 void 类型。

\2) return 语句可以有多个，可以出现在函数体的任意位置，但是每次调用函数只能有一个 return 语句被执行，所以只有一个返回值（少数的编程语言支持多个返回值，例如Go语言）。例如：

```c
//返回两个整数中较大的一个
int max(int a, int b){
    if(a > b){
        return a;
    }else{
        return b;
    }
}
```

如果`a>b`成立，就执行`return a`，`return b`不会执行；如果不成立，就执行`return b`，`return a`不会执行。

\3) 函数一旦遇到 return 语句就立即返回，后面的所有语句都不会被执行到了。从这个角度看，return 语句还有强制结束函数执行的作用。例如：

```c
//返回两个整数中较大的一个
int max(int a, int b){
    return (a>b) ? a : b;
    printf("Function is performed\n");
}
```

第 4 行代码就是多余的，永远没有执行的机会。

下面我们定义了一个判断素数的函数，这个例子更加实用：

```c
#include <stdio.h>
int prime(int n){
    int is_prime = 1, i;
    //n一旦小于0就不符合条件，就没必要执行后面的代码了，所以提前结束函数
    if(n < 0){ return -1; }
    for(i=2; i<n; i++){
        if(n % i == 0){
            is_prime = 0;
            break;
        }
    }
    return is_prime;
}
int main(){
    int num, is_prime;
    scanf("%d", &num);
    is_prime = prime(num);
    if(is_prime < 0){
        printf("%d is a illegal number.\n", num);
    }else if(is_prime > 0){
        printf("%d is a prime number.\n", num);
    }else{
        printf("%d is not a prime number.\n", num);
    }
    return 0;
}
```

prime() 是一个用来求素数的函数。素数是自然数，它的值大于等于零，一旦传递给 prime() 的值小于零就没有意义了，就无法判断是否是素数了，所以一旦检测到参数 n 的值小于 0，就使用 return 语句提前结束函数。

return 语句是提前结束函数的唯一办法。return 后面可以跟一份数据，表示将这份数据返回到函数外面；return 后面也可以不跟任何数据，表示什么也不返回，仅仅用来结束函数。

更改上面的代码，使得 return 后面不跟任何数据：

```c
#include <stdio.h>
void prime(int n){
    int is_prime = 1, i;
    if(n < 0){
        printf("%d is a illegal number.\n", n);
        return;  //return后面不带任何数据
    }
    for(i=2; i<n; i++){
        if(n % i == 0){
            is_prime = 0;
            break;
        }
    }
    if(is_prime > 0){
        printf("%d is a prime number.\n", n);
    }else{
        printf("%d is not a prime number.\n", n);
    }
}
int main(){
    int num;
    scanf("%d", &num);
    prime(num);
    return 0;
}
```

prime() 的返回值是 void，return 后面不能带任何数据，直接写分号即可。