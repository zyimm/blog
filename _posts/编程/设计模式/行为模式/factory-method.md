---
title: 💫设计模式-工厂模式
date: 2023-09-02
tags: 设计模式
---

**工厂模式**是一种创建型设计模式， 其在父类中提供一个创建对象的方法， 允许子类决定实例化对象的类型。

# 需求的场景
这种模式在面试中经常会被问到，很多面试题的答案会表示该模式经常用于框架的db或cache组件设计。

假设你所在项目组在开发某个项目中，使用了多种缓存数据源，比如有内存，redis，本地文件。目前每次根据不同场景使用不同类型缓存，需要实例化不同缓存实例进行操作，比较繁琐。项目组开发人员希望统一调用缓存入口，简化缓存调用心智负担。


# 如何解决
1. 定义cache工厂类(父类)和依赖类
2. 编写各个类型cache子类
3. cache工厂类创建调用

# 实现
> 这边实现使用PHP代码作为演示，其他oop语言逻辑类似。


**😼1.定义cache工厂类(父类)和依赖类**

```php
// config 配置类 用于缓存实例化依赖配置数据
class Config
{
    private string $host; //连接host

    private string $dbName;// 数据库名称

    private string $password;// 密码

    private string $userName;// 用户名

    public function getHost(): string
    {
        return $this->host;
    }

    public function setHost(string $host): static
    {
        $this->host = $host;
        return $this; 
    }

    public function getDbName(): string
    {
        return $this->dbName;
    }

    public function setDbName(string $dbName): static
    {
        $this->dbName = $dbName;
        return $this;
    }

    public function getPassword(): string
    {
        return $this->password;
    }

    public function setPassword(string $password): static
    {
        $this->password = $password;
        return $this;
    }

    public function getUserName(): string
    {
        return $this->userName;
    }

    public function setUserName(string $userName): static
    {
        $this->userName = $userName;
        return $this;
    }
}

// 工厂类

class Cache
{
    protected  Config $config;

    public function __construct(Config $config)
    {
        //通过构造注入Config依赖
        $this->config = $config;
    }

    public static create(string $cache) 
    {
        //创建实例
        return   new $cache($this->config);
    }

    
}


```

**😸2.编写各个类型cache子类**

```php

class Redis extends Cache
{

    public function __construct(Config $config)
    {
        parent::__construct(Config $config);

        $this->connect()
    }

    protected function connect()
    {
        //根据Config进行缓存连接
    }
}


class Memory extends Cache
{

    public function __construct(Config $config)
    {
        
        parent::__construct(Config $config);
        
    }

   
}

class File extends Cache
{

    private string $cachePath = '/dev/logs/'

    public function __construct(Config $config)
    {
         parent::__construct(Config $config);

        
    }

   
}

```

**😺3.cache工厂类创建调用**

```php
class  Demo
{
    public  function  run()
    {
        $config = (new Config())->setHost('localhost')
        ->setUserName('demo')
        ->setPassword('*****');

        $cache = new Cache($config);
        //创建redis cache实例
        $redis = $cache->create(Redis::class);
        //创建本地file cache实例
        $file = $cache->create(File::class);
    }
}

```
