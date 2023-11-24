---
title: C语言函数指针理解
date: 2022-10-13
tags: 
    - C语言 
    - 嵌入式
---

之前写了很长时间的PHP，现在对PHP一些扩展以及swoole感兴趣，但是自己的c语言的基础太差几乎忘记一干二净。首先学一下c语言，c语言核心之一就是指针，所以这里应该记录一下这边学到函数指针，整理一下自己理解。


粗暴的理解，函数指针也是指针，只是存放了函数访问地址，**函数名称可以理解为指针**，涉及&与*互逆操作。程序员通过函数指针里面地址去访问函数，也就是调用函数！意味着如下代码是互等的：

```c

(*pointer_foo_func)(10);
// 等同于
pointer_foo_func(10);

```



## c语言如何定义函数指针

其实按照C语言规定，函数名本身就是指向函数代码的指针，通过函数名就能获取函数地址，同时也支持通过&获取函数地址，这一点比较特殊。也就是说调用函数可以如下：

```c

void foo_func(int a);

void (*pointer_foo_func)(int) = &foo_func;
// 或
void (*foo_func_ptr)(int) = foo_func;

if (foo_func_ptr == foo_func) // true

```

## 使用




```c
/**
 * 函数指针理解
 */

#include <stdio.h>

// 定一个函数
void foo_func(int a, void foo_func_call(int b));

// 定义回调函数
void foo_func_call(int b);

int main() {
    int a = 1;
    //函数名本质是函数指针常量
    foo_funcf("函数名调用:\n");
    foo_func(a, foo_func_call);
    foo_funcf("函数指针调用:\n");
    //定义一个函数指针
    //指针其实是一个变量，所以要预先申明先声明后才能使用的。所以函数指针变量要先声明。
    void (*foo_func_p)(int a, void (*foo_func_call)(int b)) = &foo_func; // 等同void (*foo_func_p)(int a, foo_func_call(int b)) = foo_func;
    (*foo_func_p)(a, foo_func_call);
    return 0;
}

/**
 * foo_func
 *
 * @param a
 * @param foo_func_call
 */
void foo_func(int a, void (*foo_func_call)(int b)) {
    foo_funcf("input:%d \n", a);
    foo_funcf("调用回调函数: \n");
    int b = 12;
    (*foo_func_call)(b);
}

/**
 * 回调函数
 *
 * @param  b
 */
void foo_func_call(int b) {
    foo_funcf("回调结果:%d \n", b);
}

```


可以通过typedef定一个函数类型简化调用代码

```c

/**
 * 函数指针理解
 */

#include <stdio.h>

// 定义一个函数类型 定义函数类型不需要别名
typedef void (*func_type)(int b);

void foo_func(int a, func_type);

void foo_func_call(int b);


int main() {
    int a = 1;
    //函数指针
    void (*foo_func_p)(int a, func_type) = foo_func;
    printf("函数名调用:\n");
    foo_func(a, foo_func_call);
    printf("函数指针调用:\n");
    (*foo_func_p)(a, foo_func_call);
    return 0;
}

/**
 * foo_func
 *
 * @param a
 * @param foo_func_call
 */
void foo_func(int a, void (*foo_func_call)(int b)) {
    printf("input:%d \n", a);
    printf("调用回调函数: \n");
    int b = 12;
    (*foo_func_call)(b);
}

/**
 * 回调函数
 *
 * @param  b
 */
void foo_func_call(int b) {
    printf("回调结果:%d \n", b);
}





```