---
title: 策略模式
date: 2023-07-30
tags: 设计模式
---

策略模式是一种行为设计模式， 它能让你定义一组算法和策略， 并将每种算法分别放入独立的类中， 根据不同场景使用不同算法和策略。

# 需求的场景
假设以需要一个服务或模块来实现消息通知功能，一开始只需要邮件通知，你实现发送邮件通知功能。。。。

然后几天后需求方提出能不能增加短信通知，你加班加点实现短信通知功能。。。

一段时间之后需求方还想提供站内信通知功能。。。

不久之后需求方角色能不能提供类似webstock即时消息通知。。。

如果一开始代码都写在一起，每种消息通知方式不同整体数据结构也大相径庭，那么后期维护可想而知。。。

# 如何解决
将每种通知方式设为一种通知策略，因此在整体模块或服务设计时候，应该做好如下几点：
1. 定义好策略统一入口
2. 定义好策略调度规则
3. 约定好策略算法接口

# 实现
这边使用PHP语言实现，其他具有oop编程语言逻辑类似

1. 定义策略算法接口

```php
interface StrategyInterface
{
    public function handle(ContextInterface $content): mixed;
}
```
接口StrategyInterface handle 依赖Context上下文对象。Context在这个需求中可以封装为需要发送消息对象实现依赖。如短信，邮件:

```php
class Sms implements StrategyInterface
{
    public function handle(ContextInterface $content):mixed
    {
        // code 
    }
}

class Email implements StrategyInterface
{
    public function handle(ContextInterface $content):mixed
    {
        // code 
    }
}
```

2. 定义好策略调度规则&统一入口

```php
class DispatchStrategy
{

    public function __contract(public StrategyInterface $strategy, public ContextInterface $content )

    public  function dispatch()
    {
        $this->strategy->handle($this->content);
    }
}
```


3. 运行调用

```php
  //Context 根据不同策略实现不同的消息上下文依赖
(new DispatchStrategy(new Email(), new Context()))->dispatch(); //邮件

(new DispatchStrategy(new Sms(), new Context()))->dispatch(); //短信
```