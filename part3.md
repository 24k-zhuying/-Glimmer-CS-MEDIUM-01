* 做题历程
该代码大概用时两个多小时:sob::sob:，最大的问题就是网上没找到专门学习这个的视频，于是去问了问AI但AI给的代码有很多我理解不了的函数和思路，所以几经波折后还是决定完全靠自己想，所以花费了很多时间去思考题目，其中遇到了许多难题，就比如代码基本都写好后尝试手动输入事例但结果怎么都不对，想了很久都没想明白为什么把事例写到代码里和手动输入为什么结果不一样，最后问了AI才知道原来事例中的空格会终止输入所以才将scanf换成了fgets :sob::sob:
```c

#include <stdio.h>
#include <string.h>
#define maxSize 500

int main() {
    char string[maxSize];
    printf("请输入运算式：");
    fgets(string, maxSize, stdin);
    //scanf("%s", string);
    // 定义存储两个操作数和操作符的字符数组和变量
    char str1[maxSize]; 
    char str2[maxSize]; 
    char fuhao;
    int i = 0;
    // temp1 和 temp2 用于标记两个操作数是否为负数
    int temp1, temp2;
    int length = strlen(string);
    //初始化两个操作数的长度
    int len1 = 0; 
    int len2 = 0;
    // 如果第一个字符是'-'，表示第一个操作数是负数，
    //设置 temp1 为 1，并跳过负号
    if(string[i] == '-'){
        temp1 = 1;//代表为负数
        i++;
    }
    // 遍历字符串，提取第一个操作数
    for(i; i<length; i++){
        if(string[i] == ' '){
            continue;//不进行下面存数操作直接下一个循环
        }
        // 遇到操作符，跳出循环
        if(string[i] == '-'|| string[i] == '+' || string[i] == '*' || string[i] == '/'){
            fuhao = string[i++];
            break;
        }       
        str1[len1++] = string[i];        
    }
    str1[len1] = '\0';

    // 继续遍历字符串，提取第二个操作数
    for(i; i<length; i++){
        if(string[i] == ' '||string[i] == ')'){
            continue;
        }
        // 如果遇到'('，判断括号内是否为负数
        if(string[i] == '('){
            if(string[++i] == '-'){
            temp2 = 1;//代表为负数
            }else{
                i--;//若没有负号将前面运算的i++减回来
            }
            continue;//不进行下面存数操作直接下一个循环
        }
       
        str2[len2++] = string[i];        
    }
    str2[len2-1] = '\0';

    // 根据 temp1 的值输出第一个操作数及其正负情况
    if(temp1 == 1){
        printf("操作数1：-%s\n", str1);
    }
    else{
        printf("操作数1：%s\n", str1);
    }
    // 根据 temp2 的值输出第二个操作数及其正负情况
    if(temp2 == 1){
        printf("操作数2：-%s\n", str2);
    }
    else{
        printf("操作数2：%s\n", str2);
    }
    // 输出操作符
    printf("操作符: %c\n", fuhao);

    return 0;
}