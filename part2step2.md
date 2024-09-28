* 做题历程
这个part花费将近四十分钟（不包括学习时间），看了几遍视频才弄懂这个加法进位，还遇到了很多问题比如要消去输出结果首位的零等等，所以花费了很多时间去想办法解决:sob:
```c

#include <stdio.h>
#include <string.h>
#define maxSize 500

// 求两个整数中的较大值的函数
int Max(int i, int j){
    if(i>=j){
        return i;
    }else{
        return j;
    }
}

int main()
{
    // 输入数字
    char str1[maxSize];
    char str2[maxSize];
    printf("\n请输入第一个数字：");
    scanf("%s", str1);
    printf("\n请输入第二个数字：");
    scanf("%s", str2);

    int a[maxSize] = {0};
    int b[maxSize] = {0};
    int c[maxSize] = {0};
    // 获取数字的长度
    int len1 = strlen(str1);
    int len2 = strlen(str2);

// 将数字的字符转换为整数并逆序分别存储在数组 a b 中
    for(int i=0; i<len1; i++){
        a[len1-1-i] = str1[i] - '0';
    }
    for(int i=0; i<len2; i++){
        b[len2-1-i] = str2[i] - '0';
    }

    int lenmax = Max(len1,len2);
    // 逐位进行加法运算，并处理进位
    for(int i=0;i<lenmax;i++){
        c[i] += a[i] + b[i];
        c[i+1] = c[i]/10;
        c[i] = c[i]%10;
    }
    // 找到结果中最高位非零的位置
    int index = lenmax - 1;
     while(c[index] == 0){
        index--;
    }
    // 输出结果
    for(int j=index; j>=0; j--){
        printf("%d", c[j]);
    }
    printf("\n");

    return 0;
}