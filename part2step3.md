* 做题历程
该代码花费了将近两个多小时:sob::sob:，遇到了无数无数问题，比如将上个part如何封装成函数，大数想减的种种情况（由于没找到好的学习资源很多都是自己想的）还有判断正负和运算的各个情况······但总之遇到了许多困难的同时也有一些收获由于此代码很多都是自己想的思路所以会稍显复杂（）:sob::sob:
```c

#include <stdio.h>
#include <string.h>
#define maxSize 500

// 大数求和函数
void dashuSum(char *str1,char *str2);
// 判断字符串是否为负数的函数
int judgeStr(char *str);
// 大数相减函数
void dashuJian(char *str1,char *str2);
// 比较两个大数大小的函数  
int compare(char *num1, char *num2);

int main()
{
    char str1[maxSize];
    char str2[maxSize];
    // 输入两个数字
    printf("\n请输入第一个数字：");
    scanf("%s", str1);
    printf("\n请输入第二个数字：");
    scanf("%s", str2);
    // 判断第一个数字是否为负数，并去掉负号
    int temp1 = judgeStr(str1);
    // 判断第二个数字是否为负数，并去掉负号
    int temp2 = judgeStr(str2);
    // 比较两个数字的大小
    int temp3 = compare(str1,str2);
    // 如果有一个数字为负数
    if(temp1 ==1 || temp2 ==1){
        // 如果两个数字都为负数
        if(temp1 ==1 && temp2 ==1){
            printf("-");
            dashuSum(str1,str2);
        }
        else{
            // 一个数字为负数的情况下根据不同情况进行大数相减操作
            if(temp1 == 1 && temp3 == 1){
                printf("-");
                dashuJian(str1,str2);
            }
            if(temp1 == 1 && temp3 == -1){
                dashuJian(str1,str2);
            }
            if(temp2 == 1 && temp3 == 1){
                dashuJian(str1,str2);
            }
            if(temp2 == 1 && temp3 == -1){
                printf("-");
                dashuJian(str1,str2);
            }
            // 如果两个数字相等
            if(temp3 == 0){
                printf("0");
            }            
        }
    }else{
        // 如果两个都为正数
        dashuSum(str1,str2);
    }
         
    return 0;
}
// 判断字符串是否为负数
int judgeStr(char *str){
    if(str[0] == '-'){
        int len = strlen(str);
        // 将负数符号后的数字移动到字符串开头
        for (int i = 0; i < len - 1; i++) {
            str[i] = str[i + 1];
        }
        str[len - 1] = '\0';
        return 1;
}
    return 0;
}
// 大数求和函数
void dashuSum(char *str1,char *str2){
    int a[maxSize] = {0};
    int b[maxSize] = {0};
    int c[maxSize] = {0};
    // 获取数字的长度
    int len1 = strlen(str1);
    int len2 = strlen(str2);  
    // 将第一个数字的字符转换为整数并逆序存储在数组 a 中 
    for(int i=0; i<len1; i++){
        a[len1-1-i] = str1[i] - '0';
    }
    // 将第二个数字的字符转换为整数并逆序存储在数组 b 中
    for(int i=0; i<len2; i++){
        b[len2-1-i] = str2[i] - '0';
    }
    // 记录最长数字位数
    int lenmax = (len1>len2?len1:len2);
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

}
// 大数相减函数
void dashuJian(char *str1,char *str2){
    //原理同大树相加
    int a[maxSize] = {0};
    int b[maxSize] = {0};
    int c[maxSize] = {0};
    int len1 = strlen(str1);
    int len2 = strlen(str2);

    for(int i=0; i<len1; i++){
        a[len1-1-i] = str1[i] - '0';
    }
    for(int i=0; i<len2; i++){
        b[len2-1-i] = str2[i] - '0';
    }
    int temp = compare(str1,str2);
    int lenmax = (len1>len2?len1:len2);
    //判断两数是否想等
    for(int i=0; i<lenmax; i++){
        if(a[i] != b[i]){
            break;
        }
        if(a[i] == b[i] && i == lenmax-1){
            printf("0");
            return ;
        }
    }
    //根据两数大小作出不同运算
    if(temp == 1){
        for(int i = 0;i<lenmax;i++){
            if(a[i] < b[i]){
                a[i+1]--;
                a[i] += 10;
            }
            c[i] = a[i] - b[i];
        }
    }
    if(temp == -1){
        for(int i = 0;i<lenmax;i++){
            if(b[i] < a[i]){
                b[i+1]--;
                b[i] += 10;
            }
            c[i] = b[i] - a[i];
        }
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

}
// 比较两个大数大小的函数
int compare(char *num1, char *num2) {
    int len1 = strlen(num1);
    int len2 = strlen(num2);
    // 如果第一个数字长度大于第二个数字长度，返回 1
    if (len1 > len2) return 1;
    // 如果第一个数字长度小于第二个数字长度，返回 -1
    else if (len1 < len2) return -1;
    else {
        for (int i = 0; i < len1; i++) {
            if (num1[i] > num2[i]) return 1;
            else if (num1[i] < num2[i]) return -1;
        }
    // 如果第一个数字长度小于第二个数字长度，返回 -1
        return 0;
    }
}