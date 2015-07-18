##非局部跳转
在 C 中，goto 语句是不能跨越函数的，而执行这类跳转功能的是 **`setjmp 和 longjmp 宏`**。这两个宏对于处理发生在深层嵌套函数调用中的出错情况是非常有用的。   
此即为：**非局部跳转**。非局部指的是，这不是由普通 C 语言 goto 语句在一个函数内实施的跳转，而是在栈上跳过若干调用帧，返回到当前函数调用路径的某个函数中。
 
```
#include <setjmp.h>
int  setjmp (jmp_buf env) ;  /*设置调转点*/
void longjmp (jmp_buf env,  int val) ;  /*跳转*/
```
setjmp 参数 env 的类型是一个特殊类型 jmp_buf。这一数据类型是某种形式的数组，其中存放 在调用 longjmp 时能用来恢复栈状态的所有信息。因为需在另一个函数中引用 env 变量，所以应该将 env 变量定义为全局变量。   
longjmp 参数 val，它将成为从 setjmp 处返回的值。

```
#include <stdio.h>  
#include <setjmp.h>  
  
static jmp_buf buf;  
  
void second(void)   
{  
    printf("second\n");  
    longjmp(buf,1);              
    // 跳回setjmp的调用处使得setjmp返回值为1  
}  
  
void first(void)   
{  
    second();  
    printf("first\n");            
    // 不可能执行到此行  
}  
  
int main()   
{     
    if (!setjmp(buf))   
    {  
        // 进入此行前，setjmp返回0  
        first();  
    }   
    else   
    {     
        // 当longjmp跳转回，setjmp返回1，因此进入此行  
        printf("main\n");  
    }  
      
    return 0;  
}
```
直接调用 setjmp 时，返回值为 0，这一般用于初始化（设置跳转点时）。以后再调用 longjmp 宏时用 env 变量进行跳转。程序会自动跳转到 setjmp 宏的返回语句处，此时 setjmp 的返回值为非 0，由 longjmp 的第二个参数指定。   
**一般地，宏 setjmp 和 longjmp 是成对使用的，这样程序流程可以从一个深层嵌套的函数中返回**。



##变长参数列表

\<stdarg.h> 头文件定义了一些宏，当函数参数未知时去获取函数的参数
变量：typedef  va\_list
 
宏：  
va\_start()  
va\_arg()  
va\_end()  
 
va\_list 类型通过 stdarg 宏定义来访问一个函数的参数表，参数列表的末尾会用省略号省略   
**( va\_list 用来保存 va\_start , va\_end 所需信息的一种类型。为了访问变长参数列表中的参数，必须声明 va\_list 类型的一个对象 )**
 
我们通过`初始化( va_start )类型为 va_list 的参数表指针，并通过 va_arg 来获取下一个参数`。


```
//求任意个整数的最大值
#include <stdio.h>  
#include <stdarg.h>  
  
int maxint(int n, ...)         /* 参数数量由非变长参数n直接指定 */  
{  
    va_list ap;  
    int     i, arg, max;  
  
    va_start(ap, n);           /* ap为参数指针，首先将其初始化为最后一个具名参数, 以便va_arg获取下一个省略号内参数 */  
    for (i = 0; i < n; i++) {  
        arg = va_arg(ap, int); /* 类型固定为int, 按照给定类型返回下一个参数 */  
        if (i == 0)              
            max = arg;             
        else {  
            if (arg > max)  
                max = arg;  
        }  
    }  
    va_end(ap);  
    return max;  
}  
  
void main()  
{  
    printf("max = %d\n", maxint(5, 2, 6, 8, 11, 7));     
}  

```

##可变长数组

历史上，C语言只支持在编译时就能确定大小的数组。程序员需要变长数组时，不得不用malloc或calloc这样的函数为这些数组分配存储空间，且涉及到多维数组时，不得不显示地编码，用行优先索引将多维数组映射到一维的数组。  
ISO C99引入了一种能力，**允许数组的维度是表达式，在数组被分配的时候才计算出来**。  

```
#include <stdio.h>  
  
int  main(void)  
{  
    int n, i ;  
  
    scanf("%d", &n) ;   
  
    int array[n] ;   
    for (; i<n; i++)  
    {  
        array[i] = i ;  
    }  
  
    for (i=0; i<n; i++)  
    {  
        printf("%d,", array[i]) ;  
    }  
  
    return 0;  
} 
```

注意：  
如果你需要有着变长大小的临时存储，并且其生命周期在变量内部时，可考虑VLA（Variable Length Array，变长数组）。但这有个限制：**每个函数的空间不能超过数百字节**。因为 C99 指出边长数组能自动存储，它们像其他自动变量一样受限于同一作用域。即便标准未明确规定，VLA 的实现都是把内存数据放到栈中。VLA 的最大长度为 SIZE_MAX 字节。考虑到目标平台的栈大小，我们必须更加谨慎小心，以保证程序不会面临栈溢出、下个内存段的数据损坏的尴尬局面。

##switch语句中的case
#### case支持范围取值(gcc扩展特性) 

```
#include <stdio.h>  
  
int  main(void)  
{  
    int i=0;   
    scanf("%d", &i) ;  
  
    switch(i)  
    {  
     case 1 ... 9:  putchar("0123456789"[i]);     
     case 'A' ... 'Z':    //do something  
     }   
  
     return 0;  
}  
```

####case 关键词可以放在if-else或者是循环当中

```
switch (a)  
{  
    case 1:    ;  
      // ...  
      if (b==2)  
      {  
        case 2:;  
        // ...  
      }  
      else case 3:  
      {  
        // ...  
        for (b=0;b<10;b++)  
        {  
          case 5:   ;  
          // ...  
        }  
      }  
      break;  
   
    case 4:  
} 
```

##指定初始化（C99）

在C99之前，你只能按顺序初始化一个结构体。在C99中你可以这样做：

```
struct Foo {
    int x;
    int y;
    int z;
};
Foo foo = {.z = 3, .x = 5};
```

这段代码首先初始化了foo.z,然后初始化了foo.x. foo.y 没有被初始化，所以被置为0。   
这一语法同样可以被用在数组中。以下三行代码是等价的：

```
int a[5] = {[1] = 2, [4] = 5};
int a[]   = {[1] = 2, [4] = 5};
int a[5] = {0, 2, 0, 0, 5};
```

##受限指针（C99）
关键字 restrict 仅对指针有用，修饰指针，表明要修改这个指针所指向的数据区的内容，仅能通过该指针来实现，此关键字的作用是使编译器优化代码，生成更高效的汇编代码。

```
int foo (int* x, int* y)  
{  
    *x = 0;  
    *y = 1;  
    return *x;  
} 
```

很显然函数foo()的返回值是0，除非参数x和y的值相同。可以想象，99％的情况下该函数都会返回0而不是1。然而编译起必须保证生成100%正确的代码，**因此，编译器不能将原有代码替换成下面的更优版本**：

```
int f (int* x, int* y)  
{  
    *x = 0;  
    *y = 1;  
    return 0;  
} 
```

现在我们有了 restrict 这个关键字，就可以利用它来帮助编译器安全的进行代码优化了，由于指针 x 是修改 * x的唯一途径，编译起可以确认 “* y=1; ”这行代码不会修改 * x的内容，因此可以安全的优化。

```
int f (int *restrict x, int *restrict y)  
{  
    *x = 0;  
    *y = 1;  
    return 0;  
}  
```

很多C的库函数中用restrict关键字：  
void * memcpy( void * restrict dest ,const void * restrict src,sizi\_t n)   
这是一个很有用的内存复制函数，由于两个参数都加了 restrict 限定，所以两块区域不能重叠，即 dest 指针所指的区域，不能让别的指针来修改，即 src 的指针不能修改. 相对应的别一个函数 memmove(void *dest,const void * src,size\_t)则可以重叠。


##静态数组索引（C99）
```
void f(int a[static 10]) {
    /* ... */
}
```
你向编译器保证，你传递给 f 的指针指向一个具有至少10个 int 类型元素的数组的首个元素。我猜这也是为了优化；例如，编译器将会假定 a 非空。编译器还会在你尝试要将一个可以被静态确定为 null 的指针传入或是一个数组太小的时候发出警告。

```
void f(int a[const]) {
    /* ... */
}
```
你不能修改指针 a.，这和说明符 int * const a.作用是一样的。然而，当你结合上一段中提到的 static 使用，比如在int a[static const 10] 中，你可以获得一些使用指针风格无法得到的东西。

##多字符常量
```
int x = 'ABCD' ;  
``` 
这会把 x 的值设置为 0×41424344（或者0×44434241，取决于大小端）我们一般的小端机上，低位存在低字节处，DCBA 依次从低字节到高字节排列。  
这只是一种看起来比较炫酷的写法，一般没什么用。












