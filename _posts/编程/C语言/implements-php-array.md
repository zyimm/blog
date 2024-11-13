---
title: C 语言实现类似php的array数据类型
date: 2023-03-30
tags: 
    - C语言
---

语言实现类似php的array数据类型！

<!--more-->
```c
#include <assert.h>
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

//元素类型
typedef enum elementTypes { String, Inter } elementType;

// 定义动态数组元素结构体
typedef struct {
    void *data;
    elementType type;
} Element;


// 定义动态数组结构体
typedef struct {
    Element **data; // 数据
    size_t size; // 大小
    size_t capacity; // 容量
} Array;

// 初始化动态数组
void init_array(Array *arr, int capacity) {
    arr->data = (Element **) malloc(capacity * sizeof(Element *));
    arr->size = 0;
    arr->capacity = capacity;
}



// 释放动态数组
void free_array(Array *arr) {
    free(arr->data);
    arr->size = 0;
    arr->capacity = 0;
}

// 向动态数组中添加一个元素
void push(Array *arr, Element *element) {
    if (arr->size == arr->capacity) {
        arr->capacity *= 2;
        arr->data = (Element **) realloc(arr->data, arr->capacity * sizeof(Element *));
    }
    arr->data[arr->size++] = element;
}


// 从动态数组中删除一个元素
void remove_element(Array *arr, size_t index) {
    assert(index < arr->size); // 确保索引有效
    arr->size--;
    memmove(&arr->data[index], &arr->data[index + 1], (arr->size - index) * sizeof(Element *));
}


// 根据索引从动态数组中获取一个元素
Element *get(Array *arr, size_t index) {
    assert(index < arr->size); // 确保索引有效
    return arr->data[index];
}

void  print_array(Array arr) {
    for (int i = 0; i < arr.size; i++) {
        Element *el = get(&arr, i);
        switch (el->type) {
            case String:
                printf("%s ", (char *) el->data);
                break;
            case Inter:
                printf("%d ", *(int *) el->data);
                break;
            default:
                printf("Unknown Type\n");
        }
    }
}

int main() {
    Array arr;
    init_array(&arr, 10);
    push(&arr, &(Element){.data = malloc(sizeof(int)), .type = Inter});
    *((int *)arr.data[0]->data) = 10;
    push(&arr, &(Element){.data = malloc(sizeof(int)), .type = Inter});
    *((int *)arr.data[1]->data) = 20;
    push(&arr, &(Element){.data = malloc(sizeof(int)), .type = Inter});
    *((int *)arr.data[2]->data) = 30;
    push(&arr, &(Element){.data = strdup("zyimm"), .type = String});
    print_array(arr);
    printf("\n");
    remove_element(&arr, 0);
    print_array(arr);
    printf("\n");
    free_array(&arr);
    return 0;
}

```

上面code中使用了动态数组来表示类似于PHP的array数据类型。

1. 动态数组包含一个void指针数组和数组的大小和容量。
2. 使用realloc函数来实现动态扩容。
3. 在添加元素时，先判断数组是否已满，如果已满则动态扩容。
4. 在删除元素时，将数组中的元素向前移动一个位置，覆盖掉要删除的元素。
5. 在获取元素时，根据索引返回相应的元素,需要注意的是，由于数组中的元素是void指针类型，因此需要进行类型转换后才能使用。

这种实现方式可以用于存储任意类型的数据，包括基本类型、结构体、指针等。但需要注意的是，由于C语言中没有自带的动态类型或泛型机制，因此在使用动态数组时需要手动进行类型转换和类型检查，否则可能会导致程序出错或崩溃。
