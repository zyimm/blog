---
title: 👶算法中的空间复杂度与时间复杂度理解
date: 2023-11-04
tags: 算法
---

## 算法的本质

算法（algorithm）的本质是将问题划分为一系列可执行的步骤，并通过合理的计算和操作来达到预期的结果。同一个问题可以使用不同算法解决，但计算过程中消耗的时间和资源可能千差万别。

那如何比较不同算法之间的优劣呢？目前分析算法主要从时间和空间两个维度进行。

1. 时间维度:时间复杂度(time complexity),算法需要消耗的时间。
2. 空间维度:空间复杂度(space complexity),算法需要占用的内存空间。

因此，分析算法利弊主要从时间复杂度和空间复杂度进行。大多时候二者不可兼得，有时用时间换空间，有时用空间换时间，来满足所在场景需要！

<!--more-->
## 1. 时间复杂度 Time Complexity

时间复杂度是衡量算法执行时间的增长率，表示算法在处理输入规模增大时所需的时间。时间复杂度通常用大O符号（O）表示，后跟一个表示增长率的函数.常见的时间复杂度有：O(1)、O(log n)、O(n)、O(n log n)、O(n^2)等。

### O(1) (常数时间 Contant Time)

常数时间算法不会随数据量变化而变，时间固定。如下算法：

```php
/**
 * 获取数组的第一个元素
 */
function array_first(array $array) {
    return $array[0] ?? null;
}

// 示例用法
$array = [1, 2, 3, 4, 5];
$result = array_first($array);
echo $result; // 输出：1
```

该函数执行所需时间与 `$array` 数组大小无关。无论数组有十个元素，还是一万个元素，该函数都只检查数组第一个元素。数据量变大时，算法所需时间保持不变。

为简便起见，用`O(1)`来表示常数时间。

### O(n)（线性时间 Linear Time）

线性时间复杂度是最好理解的。随着数据量增加，耗费时间同步增加。如下算法：

```php
/**
 * 数组求和
 */
function array_sum_items(array $array) {
    $sum = 0;
    foreach ($array as $element) { // 遍历数组中的每个元素
        $sum += (int)$element; // 将元素累加到总和中
    }
    return $sum;
}

// 示例用法
$array = [1, 2, 3, 4, 5];
$result = array_sum_items($array);
echo $result; // 输出：15
```

为简便起见，用`O(n)`表示线性时间复杂度。

### O(n^2)（平方时间 Quadratic Time）

平方时间（Quadratic Time）也称为n的平方，平方时间复杂度算法耗费时间是数据量的平方。参考以下代码：

```php
function array_multiplication(array $array) {
    $result = [];
    foreach ($array as $element1) { // 遍历数组中的每个元素
        foreach ($array as $element2) { // 遍历数组中的每个元素
            $product = $element1 * $element2; // 计算两个元素的乘积
            $result[] = $product; // 将乘积添加到结果数组中
        }
    }
    return $result;
}

// 示例用法
$array = [1, 2, 3];
$result = array_multiplication($array);
print_r($result); // 输出：Array ( [0] => 1 [1] => 2 [2] => 3 [3] => 2 [4] => 4 [5] => 6 [6] => 3 [7] => 6 [8] => 9 )
```

上述代码中，`array_multiplication`函数接受一个数组作为输入，并计算数组中每两个元素的乘积，并将乘积存储在结果数组中。算法使用了嵌套的foreach循环来遍历数组中的每个元素，因此算法的执行时间与输入数组大小的平方成正比。所以它的时间复杂度是O(n^2)，即平方时间复杂度。

常规来讲同一问题下线性时间复杂度要好于平方时间复杂度。

### O(log n)（对数时间 Logarithmic Time）

对数时间（Logarithmic Time）也称为n的对数，对数时间复杂度算法耗费时间与数据量呈现对数走势。参考以下代码：

```php
function array_add_log(int $n) {
    $result = 0;
    for ($i = 1; $i <= $n; $i *= 2) { // 每次将 i 乘以 2
        $result++;
    }
    return $result;
}

// 示例用法
$n = 16;
$result = array_add_log($n);
echo $result; // 输出：5
```

上述代码中，`array_add_log`函数接受一个整数n作为输入，并使用一个循环来以2的幂次递增的方式遍历从1到n的范围。在每次循环迭代中，$i的值将乘以2。算法的循环次数取决于$i的增长速度，即取决于n的对数走势。因此，该算法的时间复杂度是O(log n)，即对数时间复杂度。

### O(n log n)（ 准线性时间 Quasilinear Time）

准线性时间算法比线性时间算法效率低，但比平方时间算法效率高。如下代码：

```php
function array_add_near_line(array $array) {
    $length = count($array);
    $result = 0;
    for ($i = 0; $i < $length; $i++) {
        $result += $array[$i];
    }
    return $result;
}

// 示例用法
$array = [1, 2, 3, 4, 5];
$result = array_add_near_line($array);
echo $result; // 输出：15
```

上述代码中，`array_add_near_line`函数接受一个数组作为输入，并遍历数组中的每个元素，将它们累加到结果中。该算法的执行时间与输入数组的大小成正比，因此它的时间复杂度可视为准线性时间复杂度。

## 2. 空间复杂度 Space Complexity

空间复杂度是衡量算法在执行过程中所需的额外空间的度量方式。它描述了算法在处理输入规模增大时所需的额外内存空间。

空间复杂度通常用大O符号（O）表示，后跟一个表示空间占用的函数。它表示算法所需的额外空间随着输入规模的增加而增加的趋势, 常见的空间复杂度有：
O(1)、O(n)、O(n^2)、O(log n)。从名字看出空间复杂度和时间复杂度表示差不多，即：常数复杂度、线性复杂度、平方复杂度、指数复杂度。

### O(1) (常数空间 Contant Space)

常数空间复杂度算法内存占用不会随数据量变化而变，占用空间固定。如下算法：

```php
function array_sum_contant(int $n) {
    $sum = 0;
    for ($i = 1; $i <= $n; $i++) {
        $sum += $i;
    }
    return $sum;
}

// 示例用法
$n = 5;
$result = array_sum_contant($array);
echo $result; // 输出：15
```

在上面的代码中，`array_sum_contant`函数接受一个整数n作为输入，并计算从1到n的所有数字的总和。该算法只使用了一个额外的变量$sum来存储计算结果，而不随着输入规模的增加而增加额外的空间。因此，该算法的空间复杂度是O(1)，即常数空间复杂度。

### O(n)（线性空间 Linear Spance）

线性空间复杂度是最好理解的。随着计算数据量增加，耗费空间同步线性增加。如下算法：

```php
function array_linear(int $n) {
    $result = [];
    for ($i = 1; $i <= $n; $i++) {
        $result[] = $i;
    }
    return $result;
}

// 示例用法
$n = 5;
$result = array_linear($n);
print_r($result); // 输出：Array ( [0] => 1 [1] => 2 [2] => 3 [3] => 4 [4] => 5 )
```

上面的代码中，`array_linear`函数接受一个整数n作为输入，并生成一个包含从1到n的所有数字的数组。该算法使用了一个额外的数组$result来存储生成的数字，其大小与输入规模n成正比。因此，该算法的空间复杂度是O(n)，即线性空间复杂度。。

### O(n^2)（平方空间 Quadratic Space）

平方空间（Quadratic Space）也称为n的平方，随着计算数据量增加，占用空间是增加数量平方。参考以下代码：

```php
function array_square(int $n) {
    $result = [];
    for ($i = 1; $i <= $n; $i++) {
        for ($j = 1; $j <= $n; $j++) {
            $result[] = $i * $j;
        }
    }
    return $result;
}

// 示例用法
$n = 3;
$result = array_square($n);
print_r($result); // 输出：Array ( [0] => 1 [1] => 2 [2] => 3 [3] => 2 [4] => 4 [5] => 6 [6] => 3 [7] => 6 [8] => 9 )
```

上面的代码中，`array_square`函数接受一个整数n作为输入，并生成一个包含从1到n的所有数字的平方的数组。该算法使用了一个额外的数组$result来存储生成的结果，其大小与输入规模n的平方成正比。因此，该算法的空间复杂度是O(n^2)，即平方空间复杂度。

### O(log n)（对数空间 Logarithmic Space）

```php
function array_space_log(int $n) {
    $result = [];
    $i = 1;
    while ($i <= $n) {
        $result[] = $i;
        $i *= 2;
    }
    return $result;
}

// 示例用法
$n = 10;
$result = logarithmicSpaceAlgorithm($n);
print_r($result); // 输出：Array ( [0] => 1 [1] => 2 [2] => 4 [3] => 8 )
```

上述代码中，`array_space_log`函数接受一个整数n作为输入，并生成一个包含从1到n之间的所有2的幂次的数组。该算法使用了一个额外的数组$result来存储生成的结果，其大小与输入规模n的对数成正比。因此，该算法的空间复杂度是O(log n)，即对数空间复杂度。
