---
title: 🔱ReactPHP的promise使用
date: 2024-02-25
tags: 
    - PHP
    - promise
---

在php中我了解目前有reactphp和guzzle组件实现javascript的Promises/A标准。promise被定义一种用于表示异步操作结果的对象，该对象链式调用(then)避免了一些异步操作多层嵌套和回调地狱问题，提高了异步操作的处理更加清晰和可维护性。

promise 对象有三种状态：pending（进行中）、fulfilled（已成功）和 rejected（已失败）。当异步操作执行完成时，Promise 对象的状态会从 pending 转变为 fulfilled 或 rejected，表示操作成功或失败。我们可以通过 then() 方法来处理这些状态的变化，并执行相应的操作。

<!--more-->

在日常开发场景中，我很喜欢用reactphp的promise组件。使用promise可以简化try catch 异常捕获，条码结构简单清晰。

下面我举例一个常规的上传表格导入数据，要求记录每行导入的结果，可以把每行导入excel操作看作一个promise，包含如下过程：

1. 检查数据
2. 写入数据库
3. 写入日志

```php
<?php

namespace react;

use React\Promise\Promise;
use Throwable;

require '../vendor/autoload.php';

class PromiseDemo
{

    private array $data;

    public function main(): void
    {
        foreach ($this->parseExcel() as $item) {
            $this->data = $item;
            $this->getPromise()->then(function () {
                //校验数据
                $this->check();
            })->then(function () {
                //校验成功 保存数据
                $this->save();
            }, function (Throwable $reason) {
                //校验失败
                //当前数据标识为失败
                $this->data['error'] = [
                    'state'   => -1,
                    'message' => $reason->getMessage()
                ];
            })->then(function () {
                //保存数据成功 不做处理
            }, function (Throwable $reason) {
                //当前数据标识为失败
                $this->data['error'] = [
                    'state'   => -1,
                    'message' => $reason->getMessage()
                ];
            })->then(function () {
                //最终成功与否 记录日志
                $this->log();
            });
        }

    }

    /**
     * 返回每行表格数据
     *
     * @return array
     */
    private function parseExcel()
    {
        $data = []; //解析每行表格数据代码省略...
        yield $data;
    }

    /**
     * 定义并获取一个Promise
     */
    private function getPromise(): Promise
    {
        return new Promise(function (callable $resolver, callable $reject) {
            try {
                //成功 fulfilled
                $resolver();
            } catch (Throwable $e) {
                //失败 rejected
                $reject();
            }
        }, function ($caller) {
            $caller();
        });
    }

    private function check()
    {
        //校验数据
    }

    private function save()
    {
        //保存数据
    }

    /**
     * @return void
     */
    private function log()
    {
        //记录日志

        if (!empty($this->data['error'])) {
            // 错误处理
        }
    }
}
(new PromiseDemo())->main();
```

如上代码相比传统代码，有如下优点：  

1. 没有多余异常try catch 异常捕获。
2. 后期扩展简单，新增业务逻辑可以根据promise回调。
3. 每行数据操作独立，避免一行错误导致数据全部回滚。
4. 每行数据是否执行成功或失败，都会记录日志结果。

根据实际使用场景，使用promise可以简化代码层次和结构，方便写出可以很好维护的代码！
