* 做题历程
这道题花费大概五六个小时:sob::sob:，最大的问题仍是找不到合适的专门学习这个方面的视频（基本都是c++，c的又讲得不行）。高精度乘法相对较简单跟减法加法类似，但高精度除法是真的难，我是在网上的视频看了他们的思路，然后根据这个思路敲代码，在这过程中虽然有几个函数可以用前几部分打出来的，但要用到的高精度减法函数前面打的不适用（前面的是输入字符串直接输出结果），所以为了适用我的思路又在这函数基础上修改了很多才能直接使用，这一步完成后完成大数除法运算算法又花费了很多时间，这一系列完成后就是函数的封装了。函数封装后我本以为基本就完成了但运行结果却一直有问题，找了很久很久也问了AI都没找到，最后才发现原来是前面part3的代码有一点点错误，然后又去改去前面的，改了后运行结果终于对了。（实在太难了）（写的代码有亿点长但全是自己想的逻辑）


```c
#include <stdio.h>
#include <string.h>
#define maxSize 500

// 大数除法函数
void division(char *str1,char *str2);
// 辅助大数除法的减法函数
void divisionJian(char *str1,char *str2);
// 比较两个大数大小的函数
int compare(char *num1, char *num2);
// 复制字符串并在末尾添加指定数量的'0'的函数
void copy(char *str,char *tempStr,int i);
// 大数乘法函数
void multiply(char *str1,char *str2);
// 大数减法函数
void dashuJian(char *str1,char *str2);
// 大数求和函数
void dashuSum(char *str1,char *str2);

int main(){
    char string[maxSize];
    printf("请输入运算式：");
    fgets(string, maxSize, stdin);
    //scanf("%s", string);
    //定义存储两个操作数和操作符的字符数组和变量
    char str1[maxSize]; 
    char str2[maxSize]; 
    //fuhao记录操作符
    char fuhao;
    // temp用于标记第二个操作数是否为负数
    int temp;
    int length = strlen(string);
    //初始化两个操作数的长度
    int len1 = 0; 
    int len2 = 0;
    int i = 0;

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
            temp = 1;//代表为负数
            }else{
                i--;//若没有负号将前面运算的i++减回来
            }
            continue;//不进行下面存数操作直接下一个循环
        }
       
        str2[len2++] = string[i];        
    }
    str2[len2-1] = '\0';

   // 根据操作符进行相应运算并输出结果
    if (fuhao == '+') {
        int bijiao = compare(str1, str2);
        if (temp == 1) {
            if (bijiao == 1) {
                // 如果第二个操作数为负数且小于第一个操作数，进行减法运算
                dashuJian(str1, str2);
            } else {
                // 如果第二个操作数为负数但不小于第一个操作数，进行减法并输出负结果
                printf("-");
                dashuJian(str2, str1);
            }
        } else {
            // 两个正数相加，调用大数求和函数
            dashuSum(str1, str2);
        }
        //减法
    } else if (fuhao == '-') {
        if (temp == 1) {
            // 第一个操作数减去负数相当于两正数相加
            dashuSum(str1, str2);
        } else {
            // 两个正数相减
            int bijiao = compare(str1, str2);
            if (bijiao == 1 || bijiao == 0) {
                dashuJian(str1, str2);
            } else {
                printf("-");//减数大的话输出负数
                dashuJian(str2, str1);
            }
        }
        //乘法
    } else if (fuhao == '*') {
        if(temp == 1){
            printf("-");//有负数答案输出负数
            multiply(str1, str2);
        }else{
            multiply(str1, str2);
        }
        //除法
    } else if (fuhao == '/') {
        if(temp == 1){
            printf("-");//有负数答案输出负数
            division(str1, str2);
        }else{
            division(str1, str2);
        }
    }
    return 0;
}

// 辅助大数除法的减法函数
void divisionJian(char *str1,char *str2){
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
    int lenmax = (len1>len2?len1:len2);

    //判断两数是否想等
    for(int i=0; i<lenmax; i++){
        if(a[i] != b[i]){
            break;
        }
        if(a[i] == b[i] && i == lenmax-1){
            str1[0] = '0';
            str1[1] = '\0';
            return;
        }
    }
    //减法操作
    for(int i = 0;i<lenmax;i++){
        if(a[i] < b[i]){
            a[i+1]--;
            a[i] += 10;
        }
        c[i] = a[i] - b[i];
    }
    //使str1变为减了后的结果方便除法运算
    int index = lenmax - 1;
     while(c[index] == 0){
        index--;
    }
    int k = 0;
    for(int j=index; j>=0; j--){
        str1[k++] = c[j] + '0';
    }
    str1[k] = '\0';
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
// 复制字符串并在末尾添加指定数量的'0'的函数
void copy(char *str,char *tempStr,int i){
    int len = strlen(str);
    strcpy(tempStr,str);
    for(int j = 0; j<i; j++){
        tempStr[len] = '0';
        //tempStr[len+1] = '\0';
        len++;
    }
    tempStr[len] = '\0';
}
// 大数除法函数
void division(char *str1,char *str2){
    //如果除数和被除数有一个为0
    if(str1[0] == '0' ){
        printf("输入错误");
        return;
    }
    if(str2[0] == '0' ){
        printf("0");
        return;
    }
    //存入结果的数组
    int result[maxSize] = {0};
    // 获取第一个大数的长度
    int len1 = strlen(str1);
    // 获取第二个大数的长度
    int len2 = strlen(str2);
    //i用于控制结果数组位数
    int i = 0;
    // 计算两个大数长度之差
    int differ = len1 - len2;
    char tempStr[maxSize];
    // 调用 copy 函数，将 str2 复制到tempStr 
    // 并在末尾添加 differ 个'0'
    copy(str2,tempStr,differ);

    // 调用 compare 函数比较 str1 和修改后的 tempStr 的大小
    int cpare = compare(str1,tempStr);
    // 再次调用 compare 函数比较 str1 和原始的 str2 的大小
    int capre2 = compare(str1,str2);
    // 当 str1 大于等于str2时进入循环
    while(capre2 == 1 || capre2 == 0){
        // 每次循环都重新复制str2到tempStr并添加新的'0'
        copy(str2,tempStr,differ);
        cpare = compare(str1,tempStr);
        // 当 str1 大于等于修改后的tempStr时进入内层循环
        while(cpare == 1 || cpare == 0){
            // 调用大数相减函数，从str1中减去tempStr
            divisionJian(str1,tempStr);
            cpare = compare(str1,tempStr);
            // 记录相减的次数
            result[i]++;        
        }
        //进位到下一位
        i++;
        differ--;
        // 更新 str1 和 str2 的比较结果
        capre2 = compare(str1,str2);
    }
    // 输出商
    for(int j=0;j<i;j++){
        printf("%d",result[j]);
    }
}
// 大数乘法函数
void multiply(char *str1,char *str2){
    int a[maxSize] = {0};
    int b[maxSize] = {0};
    int c[maxSize] = {0};
    // 获取数字的长度
    int len1 = strlen(str1);
    int len2 = strlen(str2);
    //当有数字为零结果输出为零
    if(str1[0] == '0' || str2[0] == '0'){
        printf("0");
        return;
    }

// 将数字的字符转换为整数并逆序分别存储在数组ab中
    for(int i=0; i<len1; i++){
        a[len1-i] = str1[i] - '0';
    }
    for(int i=0; i<len2; i++){
        b[len2-i] = str2[i] - '0';
    }
    int lenMax = len1 + len2;
    //进行乘法运算并将结果存在数组c中
    for(int i = 1;i<=len1; i++){
        for(int j = 1;j<=len2; j++){
            c[i+j-1] += a[i]*b[j];
            c[i+j] += c[i+j-1]/10;
            c[i+j-1] %= 10;
        }
    }
    // 找到结果中最高位非零的位置
    int index = lenMax - 1;
     while(c[index] == 0){
        index--;
    }

    // 处理特殊情况，其中一个数字为零
    if(index == 0 && c[0] == 0)
        printf("0\n");
    else{
        // 输出结果
        for(int j = index; j > 0; j--){
            printf("%d", c[j]);
        }
        printf("\n");
    }

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