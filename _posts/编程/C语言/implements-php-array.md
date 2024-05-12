---
title: C 语言实现类似php的array数据类型
date: 2023-03-30
tags: 
    - C语言
---

语言实现类似php的array数据类型！

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

// 定义动态数组结构体
typedef struct {
    void** data;    // 数据
    int size;       // 大小
    int capacity;   // 容量
} Array;

// 初始化动态数组
void initArray(Array* arr, int capacity) {
    arr->data = (void**)malloc(capacity * sizeof(void*));
    arr->size = 0;
    arr->capacity = capacity;
}

// 释放动态数组
void freeArray(Array* arr) {
    free(arr->data);
    arr->size = 0;
    arr->capacity = 0;
}

// 向动态数组中添加一个元素
void push(Array* arr, void* element) {
    if (arr->size == arr->capacity) {
        arr->capacity *= 2;
        arr->data = (void**)realloc(arr->data, arr->capacity * sizeof(void*));
    }
    arr->data[arr->size++] = element;
}
<!--more-->
// 从动态数组中删除一个元素
void removeElement(Array* arr, int index) {
    if (index < 0 || index >= arr->size) {
        return;
    }
    arr->size--;
    for (int i = index; i < arr->size; i++) {
        arr->data[i] = arr->data[i + 1];
    }
}

// 根据索引从动态数组中获取一个元素
void* get(Array* arr, int index) {
    if (index < 0 || index >= arr->size) {
        return NULL;
    }
    return arr->data[index];
}

int main() {
    Array arr;
    initArray(&arr, 10);
    int a = 10, b = 20, c = 30;
    push(&arr, &a);
    push(&arr, &b);
    push(&arr, &c);
    for (int i = 0; i < arr.size; i++) {
        printf("%d ", *((int*)get(&arr, i)));
    }
    printf("\n");
    removeElement(&arr, 1);
    for (int i = 0; i < arr.size; i++) {
        printf("%d ", *((int*)get(&arr, i)));
    }
    printf("\n");
    freeArray(&arr);
    return 0;
}

```

上面code中使用了动态数组来表示类似于PHP的array数据类型。动态数组包含一个void指针数组和数组的大小和容量。使用realloc函数来实现动态扩容。在添加元素时，先判断数组是否已满，如果已满则动态扩容。在删除元素时，将数组中的元素向前移动一个位置，覆盖掉要删除的元素。在获取元素时，根据索引返回相应的元素。需要注意的是，由于数组中的元素是void指针类型，因此需要进行类型转换后才能使用。

这种实现方式可以用于存储任意类型的数据，包括基本类型、结构体、指针等。但需要注意的是，由于C语言中没有自带的动态类型或泛型机制，因此在使用动态数组时需要手动进行类型转换和类型检查，否则可能会导致程序出错或崩溃。
