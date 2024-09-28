* 做题历程
刚开始接触大数运算感觉比较难理解，但这个part所要求的还是比较简单能很快的有思路做出来
```c

#include <stdio.h>
#include <string.h>
#define maxSize 500

int main()
{
    char str1[maxSize];// 定义字符数组 str1，用于存储第一个输入的数字字符串
    char str2[maxSize];// 定义字符数组 str2，用于存储第二个输入的数字字符串

    printf("\n请输入第一个数字：");
    scanf("%s", str1);// 从用户输入读取第一个数字字符串存入 str1
    printf("\n请输入第二个数字：");
    scanf("%s", str2);// 从用户输入读取第二个数字字符串存入 str2

    int len1 = strlen(str1);// 获取 str1 的长度
    int len2 = strlen(str2);// 获取 str2 的长度

    printf("\n第一个数字为：");
    for(int i =0; i<len1; i++){
        printf("%c", str1[i]);
    }// 遍历输出 str1 的每个字符，即第一个数字
    printf("\n");

    printf("\n第二个数字为：");
    for(int i =0; i<len2; i++){
        printf("%c", str2[i]);
    }// 遍历输出 str2 的每个字符，即第二个数字
    printf("\n");
    
    return 0;
}