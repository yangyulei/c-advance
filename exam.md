#写在前面的小测试

本系列主要讲的是 C 语言的一些深入的知识，故在开始之前大家可以先完成这个小测试，如果你对下面的题目感觉毫无压力，那么你对于 C 语言基础的掌握已经比较牢固了。

>- 1. 下面代码的输出是什么？

```
#include <stdio.h>  
int main(void)  
{  
       int a[3][2] = { (0,1), (2,3), (4,5) } ;  
       int *p ;  
       p = a[0] ;  
       printf(“%d”, p[0]) ;  
}
```
---

>- 2. int a[10]; 问下面哪些不可以表示 a[1] 的地址？  
A. a+sizeof(int)  
B. &a[0]+1  
C. (int\*)&a+1  
D. (int\*)((char*)&a+sizeof(int))

---

>- 3. 下面的C程序是合法的吗？如果是，那么输出是什么？

```
#include <stdio.h>  
int main()   
{  
    int a=3, b = 5;  
   
    printf(&a["Ya!Hello! %s\n"], &b["junk/super"]);  
       
    printf(&a["WHAT%c%c%c %c%c  %c !\n"], 1["this"],  
           2["beauty"],0["tool"],0["is"],3["sensitive"],   
           4["CCCCCC"]);  
           
    return 0;   
} 
```
---
>- 4. 32 位机上根据下面的代码，问哪些说法是正确的？(多选题类型)  
signed char a = 0xe0;  
unsigned int b = a;  
unsigned char c = a;  
A. a>0 && c>0 为真  
B. a == c 为真  
C. b 的十六进制表示是：0xffffffe0  
D. 上面都不对

---
>- 5. 下面程序的输出结果（32位小端机）

```
#include <stdio.h>  
int main()  
{  
    long long a = 1, b = 2, c = 3;  
    printf("%d %d %d\n", a, b, c);  
    return 0;  
}  
```
---
>- 6. 请问下面的程序的输出值是什么？

```
#include <stdio.h>  
#include <stdlib.h>  
   
#define SIZEOF(arr) (sizeof(arr)/sizeof(arr[0]))  
#define PrintInt(expr) printf("%s:%d\n",#expr,(expr))  
   
int main()  
{  
    int pot[] = {  
                    0001,  
                    0010,  
                    0100,  
                    1000  
                };  
    int i;  
    for(i=0;i<SIZEOF(pot);i++)  
        PrintInt(pot[i]);  
           
    return 0;  
}
```
---
>- 7. 请问下面的程序的输出值是什么？

```
#include <stdio.h>  
int main(void)  
{      
       char  a[1000] ;  
       int i ;  
       for(i=0; i<1000; i++)  
       {      
           a[i]= -1-i ;  
       }  
       printf(“%d”,strlen(a)) ;  
   
       return 0 ;  
}  
```
---
>- 8. 下面这段代码会挂么？会挂在哪一行？

```
#include <stdio.h>  
struct str{  
    int len;  
    char s[0];  
};  
  
struct foo {  
    struct str *a;  
};  
  
int main(int argc, char** argv) {  
    struct foo f = {0};  
    if (f.a->s) {  
        printf( f.a->s);  
    }  
    return 0;  
}  
```
---
>- 9. 在X86系统下，输出的值为多少？

```
#include <stdio.h>  
int main(void)  
{  
   int a[5] = {1,2,3,4,5} ;  
   int *ptr1 = (int *)(&a+1) ;  
   int *ptr2 = (int *)((int)a+1) ;  
   
   printf(“%x,%x”,ptr1[-1], *ptr2) ;  
   return 0 ;  
}  
```
---
>- 10. 请问下面的程序的输出值是什么？

```
#include <stdio.h>  
  
int main()  
{  
    int a[5][5] ;     
    int (*p)[4] ;     
    p = a ;   
    printf("%d", &p[4][2]-&a[4][2]) ;  
      
    return 0 ;  
} 
```
---

>- 11. 以下代码在 windows 系统下成员 i 的偏移量是多少？在 Linux 系统下 i 的偏移量是多少？

```
struct S  
{     
    char c ;  
    int i[2];   
    double v ;  
} ;
```
---
>- 12. 下面代码中有BUG，请找出

```
int tadd_ok(int x, int y) //判断加法溢出  
{  
       int sum = x+y ;  
       return (sum-x == y) && (sum-y == x) ;  
}  
int tsub_ok(int x, int y) //判断减法溢出  
{  
       return tadd_ok(x, -y) ;  
}  
```

---
>- 13. 下面变量的核心是什么，并用多个typedef改写下面的声明式  
int  \*(\*(\*(*abc) ( ) ) [6]) ( ) ;  

---
>- 14. (\*(void(*)( ))0 )( ) 这是什么？

---
>- 15. 编写一些代码，确定一个变量是有符号数还是无符号数

[以上问题的参考答案点击](http://blog.csdn.net/yang_yulei/article/details/8086934)









