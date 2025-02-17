+++
author = "aobara"
title = "C语言笔记2"
description = ""
date = "2024-11-13"
categories = [
    "编程",
    "笔记",
]
+++
## 记忆化存储
### c-style
注意memo数组在 `memo[49]={-1}`后其他元素为**0**.  
如果想使所有元素为**-1**则要写函数遍历。
```c
#include <stdio.h>
long long allIN(int num,long long *memo){
    if(memo[num-1]!=0)return memo[num-1];
    memo[num-1]= num*allIN(num-1,memo);
    return memo[num-1];
}
int main(){
    long long memo[999]={-1};
    memo[0]=1;
    int n;
    scanf("%d",&n);

    long long tmp=0;
    for(int i=n;i>0;i--){
        tmp+=allIN(i,memo);
    }


    printf("%lld",tmp);
}
```
### cpp-style
```cpp
#include <iostream>
#include <vector>

// 计算阶乘的递归函数，使用记忆化存储
long long factorial(int num, std::vector<long long> &memo) {
    if (num == 0 || num == 1) return 1; 
    if (memo[num] != -1) return memo[num]; 
    memo[num] = num * factorial(num - 1, memo); 
    return memo[num];
}

int main() {
    int n;
    std::cout << "请输入一个正整数 (1 <= n <= 50): ";
    std::cin >> n;

    if (n < 1 || n > 50) {
        std::cout << "输入错误，请输入 1 到 50 之间的整数。\n";
        return 1;
    }

    // 初始化记忆化存储的 vector
    std::vector<long long> memo(n + 1, -1);
    memo[0] = 1; // 0! = 1

    long long sum = 0;
    for (int i = 1; i <= n; i++) {
        sum += factorial(i, memo);
    }

    std::cout << "结果: " << sum << std::endl;

    return 0;
}
```

## 高精度加法
例题：
[[NOIP1998 普及组] 阶乘之和](https://www.luogu.com.cn/problem/P1009)
```c
#include <stdio.h>

int main(){
    int a[101]={0},b[101]={0};
    int num;

    a[0]=1;b[0]=1;
    scanf("%d",&num);
    
    for(int i=2;i<=num;i++){
        for(int j=0;j<100;j++){
            a[j]*=i;                //每位乘n
        }
        for(int j=0;j<100;j++){     //进位
            if(a[j]>9){
                a[j+1]+=a[j]/10;
                a[j]%=10;
            }
        }
        for(int j=0;j<100;j++){
            b[j]+=a[j];
            if(b[j]>9){
                b[j+1]+=b[j]/10;
                b[j]%=10;
            }
        }
    }
    int i;
    for (i=100;i>=0&&b[i]==0;i--);
    for(int j=i;j>=0;j--) printf("%d",b[j]);
    return 0;
}
```