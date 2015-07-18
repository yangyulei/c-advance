#预处理

下面介绍一些 C/C++ 中几个不常见却有用的预编译和宏定义。

>  # error  

语法格式如下：  
\#error token-sequence
其主要的作用是在编译的时候输出编译错误信息token-sequence，从方便程序员检查程序中出现的错误。 
 
```
例:  
#include "stdio.h"  
int main(int argc, char* argv[])  
{  
#define CONST_NAME1 "CONST_NAME1"  
    printf("%s\n",CONST_NAME1);  
#undef CONST_NAME1  
#ifndef CONST_NAME1  
    #error No defined Constant Symbol CONST_NAME1  
#endif  
	{  
	#define CONST_NAME2 "CONST_NAME2"  
	    printf("%s\n",CONST_NAME2);  
	}  
	printf("%s\n",CONST_NAME2);  
	return 0;  
}  
```
在编译的时候输出如编译信息  
fatal error C1189: #error : No definedConstant Symbol CONST_NAME1

>  #pragma  

其语法格式如下:   
\# pragma token-sequence   
此指令的作用是触发所定义的动作。如果token-sequence存在，则触发相应的动作，否则忽略。此指令一般为编译系统所使用。例如在Visual C++.Net 中利用# pragma once 防止同一代码被包含多次。  
 
>  #line

此命令主要是为强制编译器按指定的行号，开始对源程序的代码重新编号，在调试的时候，可以按此规定输出错误代码的准确位置。  

* 形式1  **# line constant “filename”**   
其作用是使得其后的源代码从指定的行号constant重新开始编号，并将当前文件的名命名为filename。

```
例:   
#include "stdio.h"  
void Test();  
#line 10 "Hello.c"  
int main(int argc, char* argv[])  
{  
    #define CONST_NAME1 "CONST_NAME1"  
        printf("%s\n",CONST_NAME1);  
    #undef CONST_NAME1  
    printf("%s\n",CONST_NAME1);  
   {  
       #define CONST_NAME2 "CONST_NAME2"  
       printf("%s\n",CONST_NAME2);  
   }  
   printf("%s\n",CONST_NAME2);  
   return 0;  
}  

void Test()  
{  
    printf("%s\n",CONST_NAME2);  
}  
```

提示如下的编译信息：  
Hello.c(15) : error C2065: 'CONST_NAME1' :undeclared identifier  
表示当前文件的名称被认为是Hello.c， #line 10 "Hello.c"所在的行被认为是第10行，因此提示第15行出错。  

* 形式2  **# line constant**  
其作用在于编译的时候，准确输出出错代码所在的位置（行号），而在源程序中并不出现行号，从而方便程序员准确定位。
 
> 运算符#和##   

在ANSI C中为预编译指令定义了两个运算符——#和##。  

 **`# 的作用是实现文本替换（字符串化）`**
 
```
例:
#define HI(x) printf("Hi,"#x"\n");
void main()
{
    HI(John);
}
```

程序的运行结果:  
Hi,John  
在预编译处理的时候, #x的作用是将x替换为所代表的字符序列。(即把x宏变量字符串化)在本程序中x为John，所以构建新串“Hi,John”。  
 
**`##的作用是串连接`**   

```
例:
#define CONNECT(x,y) x##y
void main()
{
    int a1,a2,a3;
    CONNECT(a,1)=0;
    CONNECT(a,2)=12;
    a3=4;
    printf("a1=%d\ta2=%d\ta3=%d",a1,a2,a3);
}
```

程序的运行结果为:  
a1=0 a2=12 a3=4  
在编译之前， CONNECT(a,1)被翻译为a1， CONNECT(a,2)被翻译为a2。  

#预定义的宏
标准C的预处理器定义了一些宏，这些宏的名称都是以`两个下划线字符开始`和结束的。程序员不能取消这些预定义宏的定义或对它们进行重新定义。   
几个常用的预定义宏：

```
__LINE__    当前源程序行的行号，用十进制整数常量表示
__FILE__    当前源文件的名称，用字符串常量表示
__DATA__    编译时的日期，用“Mmm dd yyyy”形式的字符串常量表示
__TIME__    编译时的时间，用“hh:mm:ss”形式的字符串常量表示。
```
(注意，前后是各两个下划线)

























