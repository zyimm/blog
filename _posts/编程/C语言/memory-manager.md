---
title: 🤲C语言程序的内存管理
date: 2025-02-16
tags: 
    - C语言 
    - 嵌入式
---

## 内存分布说明

C语言程序的内存通常分为以下几个区域：

- 栈（Stack）：
    1. 用于存储局部变量、函数参数和函数调用的上下文。
    2. 内存由编译器自动分配和释放。
    3. 大小有限，通常较小（几MB）。

- 堆（Heap）：

    1. 用于动态内存分配。
    2. 内存由程序员手动管理（分配和释放）。
    3. 大小较大，受系统内存限制。

- 全局/静态区（Global/Static Area）：

    1. 用于存储全局变量和静态变量。
    2. 内存在程序启动时分配，程序结束时释放。

- 常量区（Constant Area）：
    1. 用于存储字符串常量和其他常量。
    2. 内存在程序启动时分配，程序结束时释放。

- 代码区（Code Area）：用于存储程序的二进制代码。

<!--more-->
## 内存函数

### 动态内存管理

1. malloc：分配指定大小的内存块。

```c
// size：需要分配的内存大小（以字节为单位）。
void* malloc(size_t size);
// 返回值：
// 成功：返回指向分配内存的指针。
// 失败：返回 NULL。
// 示例：
int* ptr = (int*)malloc(10 * sizeof(int));  // 分配10个int大小的内存
if (ptr == NULL) {
    // 处理内存分配失败
}
```

2. calloc:分配指定数量的内存块，并将内存初始化为0
   
```c

    void* calloc(size_t num, size_t size);
    // 参数：
    //     num：需要分配的元素数量。
    //     size：每个元素的大小（以字节为单位）。
    // 返回值：
    //     成功：返回指向分配内存的指针。
    //     失败：返回 NULL。
    //示例：
    int* ptr = (int*)calloc(10, sizeof(int));  // 分配10个int大小的内存，并初始化为0
    if (ptr == NULL) {
        // 处理内存分配失败
    }
```

3. realloc：调整已分配内存块的大小。

```c
    // 参数：
    //     ptr：指向已分配内存的指针。
    //     size：新的内存大小（以字节为单位）。
    // 返回值：
    //     成功：返回指向新内存的指针。
    //     失败：返回 NULL，原内存块保持不变。
    void* realloc(void* ptr, size_t size);

    // 示例：
    int* ptr = (int*)malloc(10 * sizeof(int));
    ptr = (int*)realloc(ptr, 20 * sizeof(int));  // 将内存块大小调整为20个int
    if (ptr == NULL) {
        // 处理内存分配失败
    }

```

4. free：释放动态分配的内存。

```c
    // 参数：
    // ptr：指向需要释放的内存块的指针。
    // 注意：
    //    只能释放由 malloc、calloc 或 realloc 分配的内存。
    //    释放后应将指针设置为 NULL，以避免悬空指针。
    void free(void* ptr);
    //示例：
    int* ptr = (int*)malloc(10 * sizeof(int));
    // 使用内存
    free(ptr);  // 释放内存
    ptr = NULL; // 避免悬空指针
```

## 内存管理最佳实践

1. 在定义指针时初始化为 NULL，避免野指针的出现。
2. 每次调用 malloc、calloc 或 realloc 后检查返回值是否为 NULL，以避免内存分配失败。
3. 尽量减少频繁的内存分配和释放，尤其是小块内存。
4. 使用内存检测工具（如 Valgrind）检查内存泄漏和越界访问
5. 释放内存后立即将指针设置为 NULL，避免悬空指针。
6. 确保每次 malloc 或 calloc 后都有对应的 free 调用，以避免内存泄露。