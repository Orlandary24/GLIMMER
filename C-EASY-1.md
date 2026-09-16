# C-EASY-1:C语言入门

## part1_了解C语言配置文件

### 什么是GCC，什么是MinGW？它的作用是什么？

GCC 是 GNU 编译器套件，是真正的编译程序，能够把 C 语言源代码编译链接生成可执行文件。

MinGW 是 Windows 平台上的一套开发工具包，里面封装了 Windows 版本的 GCC，Windows 本身没有自带 gcc，安装 MinGW 后就可以在 Windows 环境调用 gcc 编译 C 程序

### c_cpp_properties.json launch.json tasks.json这三个文件分别有什么作用？

c_cpp_properties.json 控制 VSCode 的智能提示，用来指定编译器路径、头文件搜索目录、C 语言标准，解决头文件飘红、代码自动补全，只负责代码识别，不编译也不调试。tasks.json 是编译任务配置文件，定义调用 gcc 的编译命令、参数以及输出文件名，执行编译操作，将 c 源码编译生成 exe 程序。

launch.json 是调试配置文件，用于 gdb 调试，设置调试器路径、待调试的程序路径，用来断点调试已经编译好的 exe 文件。

### 为什么要在编译器内下载C语言的插件，插件的作用又是什么？

VSCode 本身只是代码编辑器，并不是专门的 C 语言开发软件，所以需要下载 C 语言相关插件。插件可以增加 C 语言语法高亮、代码提示、错误检查等功能，让编辑器能够识别 C 语言代码，方便我们编写、查看代码。

### 补全注释

  {
    // 使⽤ IntelliSense 了解相关属性。
    // 悬停以查看现有属性的描述。
    // 欲了解更多信息，请访问: https://go.microsoft.com/fwlink/?linkid=830387     "version": "0.2.0",
    "configurations": [
        {
            "name": "gcc.exe - ⽣成和调试活动⽂件",  // 该调试任务的名字，启动调试时会在待选列表中显⽰
            "type": "cppdbg",
            "request": "launch",
            "program": "${fileDirname}\\${fileBasenameNoExtension}.exe",            "args": [],
            "stopAtEntry": false,  

//是否在程序入口自动断点。false：不会自动停在开头；true会自动停
            "cwd": "${workspaceFolder}",
            "environment": [],
            "externalConsole": false, 

 //是否使用外置控制台。false=VSCode内置终端；true=单独弹出黑窗口外置控制台
            "MIMode": "gdb",
            "miDebuggerPath": "C:\\mingw64\\bin\\gdb.exe",  

//gdb调试器的完整路径，要和自己MinGW安装位置保持一致
            "setupCommands": [
                {
                    "description": "为 gdb 启⽤整⻬打印",
                    "text": "-enable-pretty-printing",
                    "ignoreFailures": true
                }
            ],
            "preLaunchTask": "C/C++: gcc.exe build active file"  // 调试前的预执⾏任务，这里的值是tasks.json⽂件中对应的编译任务，也就是调试前需要先编译
        }
    ]
}

### 截图

内置终端

![](C:/Users/Orlandary/AppData/Roaming/marktext/images/2026-09-15-11-39-59-image.png)

外置控制台

![](C:/Users/Orlandary/AppData/Roaming/marktext/images/2026-09-15-11-41-56-image.png)

## part2_C语言基础

## 变量类型

变量的类型规定了变量在内存中占用的空间大小、能够存储的数据种类，以及可以对该变量执行哪些合法运算。

变量类型非常重要，编译器会根据类型分配内存，类型选择错误会导致数据溢出、精度丢失、程序运算出错。

存放年龄 18 属于整数，应该选用 int 整型。

如果想存放单词 “apple”，只用一个 char 类型字符变量不能实现，**char 只能存储单个字符**，而 apple 是一串字符组成的字符串，正确存储方式是**使用 char 字符数组**。

## 数组的起始与边界

C 语言数组下标从 0 开始。

数组越界会访问数组以外的内存，可能读取垃圾数据、篡改其他变量，程序异常或崩溃。

C 语言不做下标检查，编译时不提示错误，问题随机性强，不容易定位，因此危险。

## 流程控制 - 循环结构

循环结构依靠**条件判断**控制代码块是否重复执行，当条件成立时代码块执行，条件不成立就结束循环。

for 循环基本结构：for(初始化表达式;条件表达式;迭代表达式){循环体代码}

while 循环基本结构：while(条件表达式){循环体代码}

for 循环三部分作用：
初始化：循环开始前，只执行一次，用来定义、赋初始值；
条件判断：每次循环开始前检查，条件为真才执行循环体，为假直接退出循环；
迭代：每次循环体执行完毕后运行，用来更新变量，趋向让循环结束。

while 与 do…while 关键区别：do…while先执行一次循环再判断条件，必定执行最少一次；while先判断条件再执行循环体。

<img title="" src="file:///C:/Users/Orlandary/AppData/Roaming/marktext/images/2026-09-15-14-53-27-image.png" alt="" width="306"><img title="" src="file:///C:/Users/Orlandary/AppData/Roaming/marktext/images/2026-09-15-15-01-51-image.png" alt="" width="307">

## 流程控制 - 逻辑表达式

本质区别：

算术表达式使用算术运算符，运算对象是数值，运算结果仍然是一个数字。

逻辑表达式使用关系、逻辑运算符，运算对象可以是数值或者关系比较结果，运算结果只有两种：真（非 0）或者假（0）。
 && 代表与，只有两边表达式全部为真，整体结果才为真；

|| 代表或，两边只要有一个表达式为真，整体结果就为真；

! 代表非，对表达式结果取反，真变假，假变真；

```

#include <stdio.h>
int main(void)

{

    int age = 19;

    int score = 61;

    if(age > 18 && score >= 60){

        printf("yesyesyes\n");

    }

    else{

        printf("yesyes\n");

    }

    return 0;

}



```

**预测输出**yesyesyes

<img src="file:///C:/Users/Orlandary/AppData/Roaming/marktext/images/2026-09-15-15-31-45-image.png" title="" alt="" width="328">

## 姓名与年龄 次数

```
#include <stdio.h>

int main(void)

{

    int age,judge=1,add=0;

    char name[99];

    printf("age name\n");

    while(judge){

        scanf("%d %s",&age,name);

        printf("%d %s\n",age,name);

        printf("继续输入请填写数字1,否则填写0\n");

        scanf("%d",&judge);

        add++;

    }

    printf("%d\n",add);

    return 0;

}

```

![](C:/Users/Orlandary/AppData/Roaming/marktext/images/2026-09-15-15-53-43-image.png)

## 函数封装更改程序代码

```
#include <stdio.h>

    int zh(int x1,int x2,int x3){

        int p1 = (x1 + x2 + x3) / 3;

        int f1 = ((p1 - x1) * (p1 - x1) + (p1 - x2) * (p1 - x2) + (p1 - x3) * (p1 - x3)) / 3;

        int zh1 = 3 * p1 - f1 / 3;

        return zh1;

    }

    void rank(int zh1,int zh2,int zh3){

        if (zh1 >= zh2 && zh2 >= zh3) {

            printf("小明 > 小强 > 小林");

        } else if (zh1 >= zh3 && zh3 >= zh2) {

            printf("小明 > 小林 > 小强");

        } else if (zh2 >= zh1 && zh1 >= zh3) {

            printf("小强 > 小明 > 小林");

        } else if (zh2 >= zh3 && zh3 >= zh1) {

        printf("小强 > 小林 > 小明");

        } else if (zh3 >= zh1 && zh1 >= zh2) {

        printf("小林 > 小明 > 小强");

        } else { // zh3 >= zh2 && zh2 >= zh1

        printf("小林 > 小强 > 小明");

        }

    }

int main(void)

{

    int x1, x2, x3;

    int y1, y2, y3;

    int z1, z2, z3;

    printf("请输入小明的三项成绩（顺序为A B C,以一个空格为间隔）：");

    scanf("%d %d %d", &x1, &x2, &x3);

    printf("请输入小强的三项成绩（顺序为A B C,以一个空格为间隔）：");

    scanf("%d %d %d", &y1, &y2, &y3);

    printf("请输入小林的三项成绩（顺序为A B C,以一个空格为间隔）：");

    scanf("%d %d %d", &z1, &z2, &z3);

    int zh1=zh(x1, x2, x3);

    int zh2=zh(y1, y2, y3);

    int zh3=zh(z1, z2 ,z3);

    rank(zh1,zh2,zh3);

    return 0;

}

```

## 失败的交换函数

![](C:/Users/Orlandary/AppData/Roaming/marktext/images/2026-09-15-17-48-01-image.png)

原因分析：main函数中，调用swap时，相当于把实参的值复制了一份代入形参中运算，此时swap函数内改变的是形参的值，而该函数最终没有任何返回值来对外部实参产生影响。
