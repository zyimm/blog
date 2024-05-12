---
title: 🎉PHP 8.3发布与新特性
date: 2023-11-23
tags: PHP
---
PHP 8.3已经如期发布，看一下新版特带来哪些特性！

## 常量类型化(Typed class constants)

定义常量目前可以标识类型了！

8.3 之前

```php
interface I {
    // 过往历史长河中总是认为PHP常量总是一个字符串
    const PHP = 'PHP 8.2';
}

class Foo implements I {
    // 但是实现类可能会将其定义为数组。
    const PHP = [];
}
```

8.3 之后

```php
interface I {
    const string PHP = 'PHP 8.3';
}

class Foo implements I {
    const string PHP = [];
}

//代码会发生致命错误：无法将数组用作类常量的值
```
<!--more-->
## 动态类常量获取(Dynamic class constant fetch)

8.3 之前

```php
class Foo {
    const PHP = 'PHP 8.2';
}

$searchableConstant = 'PHP';

var_dump(constant(Foo::class . "::{$searchableConstant}"));
```

8.3 之后

```php
class Foo {
    const PHP = 'PHP 8.3';
}

$searchableConstant = 'PHP';

var_dump(Foo::{$searchableConstant});
```

## #[\Override] 新注解

通过将#[\Override]属性添加到方法中，PHP将确保在父类或实现的接口中存在具有相同名称的方法。如下面`MyTest`的父类`TestCase`中没有`taerDown`方法将抛出异常

```php

use PHPUnit\Framework\TestCase;

final class MyTest extends TestCase {
    protected $logFile;

    protected function setUp(): void {
        $this->logFile = fopen('/tmp/logfile', 'w');
    }

    #[\Override]
    protected function taerDown(): void {
        fclose($this->logFile);
        unlink('/tmp/logfile');
    }
}

// Fatal error: MyTest::taerDown() has #[\Override] attribute,
// but no matching parent method exists

```

## 深度克隆只读属性(Deep-cloning of readonly properties)

只读属性现在可以在__clone方法中修改一次，以启用只读属性的深度克隆。

8.3 之前

```php
class PHP {
    public string $version = '8.2';
}

readonly class Foo {
    public function __construct(
        public PHP $php
    ) {}

    public function __clone(): void {
        $this->php = clone $this->php;
    }
}

$instance = new Foo(new PHP());
$cloned = clone $instance;

// Fatal error: Cannot modify readonly property Foo::$php

```

8.3 之后

```php
class PHP {
    public string $version = '8.2';
}

readonly class Foo {
    public function __construct(
        public PHP $php
    ) {}

    public function __clone(): void {
        $this->php = clone $this->php;
    }
}

$instance = new Foo(new PHP());
$cloned = clone $instance;

$cloned->php->version = '8.3';
```

## json_validate 函数

8.3 之前验证json 需要json_decode之后根据json_last_error()错误去判断，如今8.3新增json_validate函数支持

## Randomizer 新特性

1. getBytesFromString()
2. getFloat()
3. nextFloat()

## 其他

其余bug和特性请移步到，[php官方8.3发布页面介绍](https://www.php.net/releases/8.3/en.php)。
