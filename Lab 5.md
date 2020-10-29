# Lab 5

> 本次lab目标：
>
> 1. 进一步规范编程习惯
> 2. 回顾作业存在的问题
> 3. 使用编程解决实际问题



## 获取及提交lab

**获取**：通过 `https://github.com/fdu-20ss-programming-A/lab5`或者`超星平台`获取。

**提交**：超星平台上已经发布了LAB5作业，同学需要将每题的**代码**、**运行结果**、个人对题目的思考体会（可选）以**图片**或者**文档**（ word 或者 pdf ）的形式在超星LAB5对应的作业区域提交即可。

**提交物**：代码、运行结果，每题可整理为一份 word 或者 pdf 文档，也可以将两者截在一张图片上提交。

**截止时间**：2020年11月3日 23:59:59



## 1. lab4回顾

> LAB4复习了控制流的相关知识

- ### 代码要正确排版

  正确排版的代码能显著地提高可读性，具体见于 [C语言代码规范（一）缩进与换行](https://blog.csdn.net/Dr_Haven/article/details/81868857)

  每一份未正确排版的代码视离谱程度扣2~5分

  ```c
  #include <string.h>
  #include <stdio.h>
  int main(){
  int upper=0;
  char input[256];
  scanf("%s",input);
  for(int n=0;n<=strlen(input);n++){
      if(input[n]>='A'&&input[n]<='Z') upper+=1;
  }
  ...
  }
  ```

  ```c
  int main()
  { int n,x,j,k,i;
   	printf("Please input the line number:");
   	scanf("%d",&n);
    for(j=1;j<=n;j++)
    {
        for(x=n-j;x>=0;x--)
        {printf(" ");}
        for(k=j;k>=1;k--)
        {printf("%d",k);}
        if(j>=2)
            for(i=2;i<=j;i++)
        {printf("%d",i);}
    printf("\n");
    	  }
   
   	 return 0;
  }
  ```

  ```c
  if(e>f)			printf("xxxxxxxxxx");
  else if(e<f)   printf("xxxxxxxxxx");
  else          printf("xxxxxxxxxx");
  ```

  

- **学会简单地测试自己的程序**

  多试几组输入，并体现在截图中

- **注意程序的健壮性**

  健壮性是指软件对于规范要求以外的输入情况的处理能力。
  
  所谓健壮的系统是指对于规范要求以外的输入能够判断出这个输入不符合规范要求，并能有合理的处理方式。
  
  以lab4打印金字塔为例，输入的数字有可能不在1~10这个范围内，甚至可能输入的不是数字，这种事情本来在编写程序时是假定不会发生的，但实际使用过程中，一切输入都是有可能的，因此需要程序员添加额外的代码让程序在执行时响应这些事件。
  
  

### 第一题

题目

> 编写程序，提示用户输入一个在1到10间的整数，然后显示一个金字塔形状的图案。

示例

> 请输入行数：5
>
> ​            1
>
> ​         2 1 2
>
> ​      3 2 1 2 3
>
>       4 3 2 1 2 3 4
>
> 5 4 3 2 1 2 3 4 5

出现的问题：

```c
1. 定义了一个数组，在循环中依次打印出数组的元素
int main(){
    int n;
    char s[10][3] = {"1", "2", "3", "4", "5", "6", "7", "8", "9", "10"};
    /***
    char s[10][3]
    "1" \0 \0
    "2" \0 \0
    "3" \0 \0
    ...
    ***/
    /***
    int s[10][3]
    1 2 3
    4 5 6
    ....
    ***/
    scanf("%d", &n);
    for (int i = 0; i < n; i++){
        if (n == 10 && i != 9){
            printf(" ");
        }
        for (int j = 0; j < n - i - 1; j++){
            printf(" ");
        }
        for (int j = i + 1; j > 0; j--){
            printf("%s ", s[j - 1]);/*相当于s[j-1][0]*/
        }
        for (int j = 2; j < i + 2; j++){
            printf("%s ", s[j - 1]);
        }
        putchar('\n');
    }
}
```

参考答案：

```C
#include <stdio.h> 
int main(){ 
    int number; 
    printf("Please input a number:"); 
    scanf("%d",&number); 
    /* 循环打印所有行 */ 
    for(int i = 0; i < number; i++){ 
        /* 循环打印每一行前面的空格 */ 
        for(int j = 0;j < number-i-1; j++){ 
            printf(" "); 
        }
        /* 循环打印每一行的数字 */ 
        for(int j = i; j >= -i; j--){ 
            printf("%d ",j>=0?j+1:-j+1); 
        }
        printf("\n"); 
    }
    return 0; 
}
```



### 第二题

题目

> 计算1+1/2+1/3+1/4+...+1/n的值，n取50000。尝试从左向右计算以及从右向左计算，打印两种
>
> 遍历方法的输出，并尝试比较哪种遍历方法更加精确。（浮点数需要使用flfloat数据类型表示）

提示

> 处理很大的数字以及很小的数字的时候，会产生一个抵消错误。例如，1000 000 000.0 + 0.000 000 0001等于1000 000 000.0。

出现的问题：

```c
1. 未作比较或说明哪种方法更精确
```

参考答案：

```C
#include <stdio.h> 
int main(){ 
    int n = 50000; 
    float sum = 0; 
    for(int i = 1; i <= n; i++){ 
        sum += 1.0f/i; 
    }
    printf("From left to right: %.10f\n",sum); 
    sum = 0;
    for(int i = n; i >= 1; i--){ 
        sum += 1.0f/i; 
    }
    printf("From right to left: %.10f\n",sum); 
    /* double精度更高，得到的结果更加精确 */
    double dsum = 0; 
    for(int i = n; i >= 1; i--){ 
        dsum += 1.0f/i; 
    }
    printf("Sum of double: %.10lf\n",dsum); 
    return 0; 
}
/*将两种遍历结果与使用double作为储存变量的遍历结果比较，可以得知，从右往左的结果更加精确，原因是从右往左可以缓解大浮点数加小浮点数的情况。 */
```



### 第三题

题目

> 编写一个程序，提示用户输入一个字符串，统计字符串中大写字符的数目。

示例

> 请输入字符串：Welcome!
>
> 大写字符的数量为2

参考答案：

```C
#include <stdio.h> 
#include <string.h> 
#define MAXLINE 256 
int main(){ 
    char input[MAXLINE]; 
    printf("Please input a string: "); 
    /* 这里不需要取地址，因为input本身代表字符串地址 */ 
    scanf("%s",input); 
    int len = strlen(input); 
    int count = 0; 
    for(int i = 0; i < len; i++){ 
        /* 判断大写字符 */ 
        if(input[i] >= 'A' && input[i] <= 'Z'){ 
            count++; 
        } 
    }
    printf("Upper case character number: %d\n",count); 
    return 0; 
}
```



### 第四题

题目

> 如果一个正整数等于除它本身之外其他所有因数之和，就称之为完全数。编写程序输出10000以内的所有完全数。

示例

> 6 = 1+2+3，所以6是完全数
>
> 9 != 1+3+9，所以9不是完全数
>
> 28 = 14+7+4+2+1，所以28是完全数

出现的问题：

```c
1. 不读题----最后输出了10000以内完全数的和
```

参考答案：

```C
#include <stdio.h> 
#include <math.h> 
int main(){ 
    int num = 10000; 
    for(int i = 2; i <= num; i++){ 
        /* 判断因数分解的因子只需要循环遍历到sqrt(i) */
        int last = (int)sqrt(i); 
        int sum = 1; 
        for(int j = 2; j <= last; j++){ 
            if((i%j)==0){ 
                sum += j + i / j; 
            } 
        }
        if(sum == i){ 
            printf("%d\n",i); 
        } 
    }
    return 0; 
}
/*输出结果： 
6
28
496 
8128 
*/
```



## 2. 上一次作业回顾

### 第一题

题目

> 利用人工方式比较分数大小的最常见的方法是：对分数进行通分后比较分子的大小。请编程模拟手工比较两个分数的大小。首先输入两个分数分子分母的值，例如"11/13,17/19"，比较分数大小后输出相应的提示信息。例如，第一个分数11/13小于第二个分数17/19，则输出"11/13<17/19"。

出现的问题：

```c
1. 没有通分，直接算浮点值进行比较
2. 大多数人没有处理分母为0以及分母为负数的情况（未扣分）
```

参考答案：

```c
#include <stdio.h> 
int main(){ 
    int a,b,c,d; 
    printf("Input a/b, c/d:"); 
    scanf("%d/%d, %d/%d",&a,&b,&c,&d); 
    if(a*d>c*b) 
        printf("%d/%d>%d/%d\n",a,b,c,d); 
    else if(a*d==c*d) 
        printf("%d/%d=%d/%d\n",a,b,c,d); 
    else
        printf("%d/%d<%d/%d\n",a,b,c,d); 
    return 0; 
}
```

### 第二题

题目

> 设capital是最初的存款总额（即本金），rate是整存整取的存款年利率，n 是储蓄的年份，
>
> deposit是第n年年底账号里的存款总额。已知如下两种本利之和的计算方式：
>
> 按复利方式计息的本利之和计算公式为：
>
>  $deposit = capital * (1+rate) ^n$
>
> 按普通计息方式计算本利之和的公式为：
>
> $deposit=capital*(1+rate*n)$
>
> 已知银行整存整取不同期限存款的年息利率分别为：
>
> 存期1年，利率为 0.0225
>
> 存期2年，利率为 0.0243
>
> 存期3年，利率为 0.0270
>
> 存期5年，利率为 0.0288
>
> 存期8年，利率为 0.0300
>
> 若输入其他年份，则输出"Error year!"
>
> 编程从键盘输入存钱的本金和存款期限，然后再输入按何种方式计息，最后再计算并输出到期时能从银行得到的本利之和，要求结果保留到小数点后4位。

出现的问题：

```c
1. switch语句的滥用或不用
2. 代码冗余
3. 字符输入只考虑了YNyn
4. 不读题----没有分情况（y/n）计算本利之和
```

问题举例：

```c
1.
switch (n){
    case 1:rate=0.0225;
        if(a=='y' ||a=='Y') d=c*pow(1+rate,n);
        	else d=c*(1+pow(rate,n));
        		break;
    case 2:......
}

2.
if(ch=='Y'||ch=='y')
{
    switch(n)
    {
          .....
    }
}
else if(ch=='N'||ch=='n')
{
    switch(n)
    {
          .....  
    }
}

3.
if (year==1)
{
    ...
}
else if(year==2)
{
    ...
}......
```

参考答案：

```c
#include <stdio.h> 
#include <math.h> 
int main(){ 
    int year,capital; 
    float rate,deposit; 
    int choice; 
    char input; 
    printf("Input capital, year:"); 
    scanf("%d,%d",&capital,&year); 
    printf("Compound interest (Y/N)?"); 
    scanf(" %c",&input);/* 注意空格 */ 
    
    switch(year){ 
        case 1: 
            rate = 0.0225; 
            break; 
        case 2: 
            rate = 0.0243; 
            break; 
        case 3: 
            rate = 0.0270; 
            break; 
        case 5: 
            rate = 0.0288; 
            break; 
        case 8: 
            rate = 0.0300;
            break; 
        default: 
            printf("Error year!\n"); 
            return 0; 
    }
    if(input == 'Y' || input == 'y'){ 
        deposit = capital * pow((1+rate),year); 
    }
    else if(input == 'N' || input == 'n'){ 
        deposit = capital * (1 + rate*year); 
    }
    else{
        printf("Wrong choice\n"); 
        return 0; 
    }
    printf("rate = %.4f, deposit = %.4f\n",rate,deposit); 
    return 0; 
}
```



## 3. LAB5上机作业

> 上机作业建议每题单独写在XXX.c文件中。每题提交时只需要提交运行代码截图以及运行结果截图。可以直接截在**一张图片**中，也可以截多张图片**合并为一个文档**。运行结果我们建议同学测试多组测试数据，检查代码逻辑的正确性。

### Question 1

题目

>编写一个程序，显示从101年到2100年的所有闰年
>
>每行10个，用空格分隔

提示

> 闰年分为普通闰年和世纪闰年
>
> 公历年份是4的倍数的，且不是100的倍数，为普通闰年（如2004年、2020年就是闰年）
>
> 公历年份是整百数的，必须是400的倍数才是世纪闰年（如1900年不是世纪闰年，2000年是世纪闰年）

输出示例

> 从101到2100的所有闰年为：
>
> 104 108 112 116 120 124 128 132 136 140
> 144 148 152 156 160 164 168 172 176 180
> 184 188 192 196 204 208 212 216 220 224
> 228 232 236 240 244 248 252 256 260 264
> 268 272 276 280 284 288 292 296 304 308
>
> ........

### Question 2

题目

> 编写一个程序，提示用户输入一个十进制整数并显示其对应的二进制值

示例

> 请输入一个十进制整数：100
>
> 十进制数“100”的二进制值为：1100100 

### Question 3

题目

> 使用如下式子来近似e：
>
> e = 1 + $\frac{1}{1!}$ + $\frac{1}{2!}$ + $\frac{1}{3!}$ + $\frac{1}{4!}$ + ... + $\frac{1}{i!}$  
>
> 编写一个程序，显示i = 10， 20，30，...，100时的e值

输出示例

> i = 10时，e值为：1.7182818011463847
>
> i = 20时，e值为：1.7182818284590455
>
> ....
>
> i = 100时，e值为：1.7182818284590455

### Question 4

题目

> 编写一个程序，提示用户输入一个年份和该年第一天是星期几这两个信息，然后输出该年中每个月的第一天是星期几
>
> 输入时依次用1~7代表星期一到星期日

示例

> 请输入年份与星期几：2013，2
>
> 2013年1月1日是星期二
>
> ....
>
> 2013年12月1日是星期日
