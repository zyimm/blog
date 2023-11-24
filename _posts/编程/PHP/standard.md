---
title: PHP代码统一规范规则细节
date: 2023-06-09
tags: 
    - PHP
---

## PHPDoc

1. 变量以及属性的辅助注解.方便IDE跳转和追踪

```php
 <?php
/** @var int $foo */
 $foo = 2 + 2;


 final class Foo
 {
     /**
      * @var int
      */
     public $bar;

     /**
      * @type float
      */
     public $baz;
 }
```
<!--more-->>
2. 变量以及属性多态类型优先级

```php
# 修正前： 

/**
 * @param null|string|int|\Foo $bar
 */
# 修正后：
/**
 * @param \Foo|int|null|string $bar
 */
```

3. 注释对齐（居中非依左或不对齐）

```php
/**
 
# 修改前
 * @param  EngineInterface $templating
 * @param string      $format
 * @param  int  $code       an HTTP response status code
 * @param    bool         $debug
 * @param  mixed    &$reference     a parameter passed by reference

 
# 修改后
 * @param EngineInterface $templating
 * @param string          $format
 * @param int             $code       an HTTP response status code
 * @param bool            $debug
 * @param mixed           &$reference a parameter passed by reference

```

4. 未确定类型使用`mixed`声明,默认值带为null需要用`?`修饰符

```php
/**
  * @param string|null   $foo 
  * @param int           $bar
  * @param mixed         $baz
  *
  * @return void
  */
 function foo(?string = null $foo, int $bar, $baz) {}

```

5. 数组key以及类属性对齐

```php
# 修改前
class Foo
{
    public  $a = 0;

    public $b = 0;
    public $c = 0;

        public$d = 0;
}
# 修改后
class Foo
{
    public $a = 0;

    public $b = 0;

    public $c = 0;

    public $d = 0;
}
# 修改前
$arr = [
    'a'=>'tom',
    'c' =>  'sevne',
    'd'=>  'mazda',
    'b'  =>'cross m78 unm',
]
# 修改后
$arr = [
    'a' => 'tom',
    'c' => 'sevne',
    'd' => 'mazda',
    'b' => 'cross m78 unm',
]

```

## 代码约束

1. declare_strict 严格声明。主要解决程序在处理类型时候避免隐性转换

```php
declare(strict_types=1);
//code ....

```

2. 使用常量`PHP_EOL`替代 `"\n"` 换行符号

```php

 echo  "some thing \n";
 echo  "some thing PHP_EOL";

```

3. 声明函数必须使用`function_exists`包裹判断,避免函数重复声明

```php
if (!function_exists('dd')) {
    /**
     * dd 调试
     *
     * @param ...$vars
     *
     * @return void
     */
    function dd(...$vars)
    {
        foreach ($vars as $v) {
            VarDumper::dump($v);
        }
        exit(1);
    }
}
```

1. 函数或类的方法没有返回类型需要声明`void`类型

```php
function foo(string $a): void {}


class Foo
{
    public function handle(): void;
    {
        //code ...
    }
}
```

5. 命名空间导入，导入或完全限定全局类/函数/常量

```php
<?php

# 修改前
$d = new \DateTimeImmutable();
# 修改后
$d = new DateTimeImmutable();

```

6. 使用`::class`替换完整类名使用,以及替代`get_class`函数

```php
# 修改前
$className = 'Foo\Bar\Baz';
# 修改后
$className = Baz::class;
```

7. 多次调用转换成单次调用

```php
# 修改前
$a = isset($a) && isset($b);
# 修改后
$a = isset($a, $b)  ;

# 修改前
unset($a);
unset($b);
# 修改后
unset($a, $b);
```

8. 使用语法糖简化调用

```php
# 修改前
$boo = isset($foo) ? $foo : null;
# 修改后
$boo = $foo ?? null; 
//或
$foo ??= null;

# 修改前
$boo = $foo ? $foo : '';
# 修改后
$boo = $foo ?:'';


# 修改前
list($a, $b) = $foos;
# 修改后
[$a, $b] = $foos;
```

9. 变量占位符由`${` 转换`{$)` php的高版本已经抛弃`${`用法

```php
$name = 'World';
# 修改前
echo "Hello ${name}!";
# 修改后
echo "Hello {$name}!";
```

10. PHP常量true、false和null须使用小写

```php
# 修改前
$a = FALSE;
$b = True;
$c = NULL;
# 修改后
$a = false;
$b = true;
$c = null;
```

11. 使用动态属性或方法，属性需要实现`__get`和`__set`，方法需要`_call()`或`__callStatic`以及在class 头部声明,便于ide识别以及查阅

```php
/**
 * Class Foo
 * 
 * @property string $attr  
 * @method Foo handleA(string $arg) 
 */
class Foo
{
    public function __set($name, $value)
    {
        //code 
    }

    
    public function __get($name)
    {
        //code 
    }


    public function __call($method, $args)
    {
        //code 
    }

    public static function __callStatic($method, $args)
    {
        //code 
    }
}
```

## 耦合优化

1. 使用依赖对象处理多参数方法或函数,避免后面迭代出现超级函数

```php
# 修改前
function  foo(string $argv, int $argc, array $arg, \Closure $argz) :void
{
    //code 
}
# 修改后

Class Boo
{
    public string $argv;

    public string $argc;

    public string $argg;

    public function argz() : Closure
    {
        return  function (){ };
    }
}

function  foo(Boo $boo) :void
{
    //code 
}
```
