1.
C语言：
```
#include <stdio.h>
#include <stdlib.h>

int intcomp(int *x, int *y)
{       
    return *x - *y;
}

int a[1000000];

int main()
{   
    int i, n=0;
    while (scanf("%d", &a[n]) != EOF)
        n++;
    qsort(a, n, sizeof(int), intcomp);
    for (i = 0; i < n; i++)
        printf("%d\n", a[i]);
    return 0;
}
```

C++：
```
#include <iostream>
#include <set>
using namespace std;

int main()
{       
    set<int> S;
    int i;
    set<int>::iterator j;
    while (cin >> i)
        S.insert(i);
    for (j = S.begin(); j != S.end(); ++j)
        cout << *j << "\n";
    return 0;
}
```

2.
```
#include <stdio.h>

#define BITSTEPWORD 32     //表示一个整型含有32个位
#define SHIFT 5        //单次位移量
#define MASK 0x1F        //掩码
#define NUM 10000000    //表示1000万个整数
int array[1 + NUM / BITSTEPWORD];//使用整型数组模拟定义1000万个位的数组

/*功能：设置位数组中的从0开始的第i位为1
 *参数：需要设置为1的位
 */
void set(int i)
{
    array[i >> SHIFT] |= (1 << (i & MASK));
}

/*
 *功能：设置位数组中的从0开始的第i位为0
 *参数：需要设置为0的位
 */
void clr(int i)
{
    array[i >> SHIFT] &= ~(1 << (i & MASK));
}
 /*
  *功能：取出从0开始的第i位的值，用于检测
  */
void test(int i)
{
    return array[i >> SHIFT] & (i << (i & MASK));
}

int main(void)
{
    int i;
    clear();
    for( i = 0; i < 100; i++ )
        set( i*2 );
    for( i = 0; i < 200; i++ )
        printf("%d",test(i) ? 1:0 );
    puts("");
    return 0;
}
```

3.
下面使用上一题中的代码：
```
int main(void)
{
  int i;

  for( i = 0; i < N; i++ )
    clr( i );

  while( scanf("%d", &i) != EOF )
    set( i );

  for( i = 0; i < N; i++ )
    if( test(i) )
      printf("%d \n", i);

  return 0;
}

```

4.
```
int Bigrand()// 产生很大随机整数
{
    return RAND_MAX*rand()+rand();
}

int Randl_r(int l,int r)//产生很l~r之间的随机数
{
    return l+Bigrand()%(r-l+1);
}

void make_data(int num)
{
    int *temp = new int[MAXN_NUM];
    for(int i = 0; i <MAXN_NUM;i++)//MAXN_NUM指随机数最大可能是多少
    {
        temp[i] = i + 1;
    }
    for(int i = 0; i < DATA_NUM; i++)//DATA_NUM指随机数有多少个
    {
                int t = Randl_r(i,MAXN_NUM) % MAXN_NUM;
                swap(temp[i], temp[t]);
    }
}　
```

5.我们可以采用多趟算法，如2趟，首先采用500万/8字节的空间（100字节约为1M），来排序0到4999999的数，第二趟再排序5000000到9999999的数！K趟算法空间需求n/k，时间开销kn.典型的时间换空间。

```
#include <iostream>
#include <algorithm>
#include <time.h>
#include <bitset>
#include <iostream>
#include <cstring>
#include <cmath>
#include <queue>
#include <stack>
#include <list>
#include <map>
#include <set>
#include <sstream>
#include <string>
#include <bitset>
#include <vector>
#include <cstdio>
#include <algorithm>
#include <fstream>
using namespace std;

#define DATA_NUM 1000000          //生成的随机数的个数
#define MAXN_NUM 1000000         //随机数最大为多少
#define SOURCE     "data.txt"        //保存随机数的文件名称
#define RESULT     "result.txt"    //保存排序结果的文件名称

//功能：生成随机数文件
//参数：num 生成随机数的个数
int Bigrand()// 产生很大随机整数
{
    return RAND_MAX*rand()+rand();
}
int Randl_r(int l,int r)//产生很l~r之间的随机数
{
    return l+Bigrand()%(r-l+1);
}
void make_data(int num)
{
    int *temp = new int[MAXN_NUM];
    if(temp == NULL)
    {
        cout << "new error in make_data()" << endl;
        return;
    }

    for(int i = 0; i <MAXN_NUM;i++)
    {
        temp[i] = i + 1;
    }

    for(int i = 0; i < DATA_NUM; i++)
    {
        int t = Randl_r(i,MAXN_NUM) % MAXN_NUM;
        swap(temp[i], temp[t]);
    }

    //写入文件
    FILE *fp;
    fp = fopen(SOURCE, "w");
    for(int i = 0; i < DATA_NUM; i++)
    {
        fprintf(fp, "%d ", temp[i]);
    }
    fclose(fp);
    cout << "随机数文件生成成功." << endl;
}

void BitMapSort()
{
    bitset<MAXN_NUM+5> bitmap;
    bitmap.reset();
    FILE *fpsrc;
    fpsrc = fopen(SOURCE, "r");
    int data;
    while(fscanf(fpsrc, "%d ", &data) != EOF)
    {
        if(data <= MAXN_NUM / 2)
        {
            bitmap.set(data, 1);
        }
    }
    //将排序好的数写入到结果文件中
    FILE *fpdst;
    fpdst = fopen(RESULT, "w");
    for(int i = 0; i <MAXN_NUM+5; i++)
    {
        if(bitmap[i] == 1)
        {
            fprintf(fpdst, "%d ", i);
        }
    }

    //处理剩下的数据
    int res = fseek(fpsrc, 0, SEEK_SET);
    if(res)
    {
        cout << "fseek() error in BitMapSet()." << endl;
        return;
    }
    bitmap.reset();
    while(fscanf(fpsrc, "%d ", &data) != EOF)
    {
        if(data <= MAXN_NUM&& data > MAXN_NUM/ 2)
        {
            data = data -MAXN_NUM / 2;
            bitmap.set(data, 1);
        }
    }

    for(int i = 0; i <= MAXN_NUM/ 2 + 1; i++)
    {
        if(bitmap[i] == 1)
        {
            fprintf(fpdst, "%d ", i + MAXN_NUM / 2);
        }
    }
    cout << "排序成功." << endl;
    fclose(fpdst);
    fclose(fpdst);
}


int main()
{
    make_data(DATA_NUM);
    clock_t start = clock();
    BitMapSort();
    clock_t end = clock();
    cout << "排序所用时间为:" << (end - start) * 1.0 / CLK_TCK << "s" << endl;
    return 0;
}

```

6.如果每个整数最多出现10次，那么我们可以用4位（2^3<10<2^4）（半个字节）来统计每个整数的出现次数。可以利用问题5中的方法，利用10000000/2个字节的空间遍历一次来完成对整个文件的排序。当保存的数字量变化时，分成更多的份，就可以再更小的空间内完成，如10000000/2k的空间内。

参考链接：![](http://www.cnblogs.com/Zeroinger/p/5502382.html)

9.top变量用来记录已经初始化过的元素个数，from[i]=top，相当于保持a[i]是第几个初始化过的元素，to[top]=i，用来致命第top个初始化的元素在data里的下标是多少。因此每次访问一个data元素时，判断from[i] < top，即data[i]元素是否被初始化过，但当top很大时，from[i]里被内存随便赋予的初始化值可能真的小于top，这时候我们就还需要to[from[i]] == i 的判断了来保证from[i] < top不是因为内存随意赋给from[i]的值本身就小于top而是初始化后小于的。
```
if(from[i] < top && to[from[i]] == i)  
{  
    printf("has used!\n")  
}  
else  
{  
    a[i] = 1;  
    from[i] = top;  
    to[top] = i;  
    top++;  
}
```

10.根据电话号码的最后两位作为客户的哈希索引，进行分类，当顾客打电话下订单的时候，它被放置在一个合适的位置。然后当顾客抵达进行检索商品时，营业员按顺序检索订单，这是经典的解决哈希冲突的解决方法：通过顺序检索。电话号码的最后两位数字是相当随机的，而电话号码的前两位作为索引行不通，因为很多电话号码的前两位是相同的。
