# 单例模式（Singleton）

这种模式涉及到一个单一的类，该类负责创建自己的对象，同时确保只有单个对象被创建。    
这个类提供了一种访问其唯一的对象的方式，可以直接访问，不需要实例化该类的对象。

## 目地
- 在应用程序调用的时候，只能获得一个对象实例。
## 例子

- 数据库连接
- 日志 (多种不同用途的日志也可能会成为多例模式)
- 在应用中锁定文件 (系统中只存在一个 ...)

## 代码
[Singleton.php](Singleton.php)
```php
<?php

namespace DesignPatterns\Creational\Singleton;

final class Singleton
{
    /**
    * @var Singleton
    */
    private static $instance;

    /**
    * 通过懒加载获得实例（在第一次使用的时候创建）
    */
    public static function getInstance(): Singleton
    {
        if (null === static::$instance) {
            static::$instance = new static();
        }

        return static::$instance;
    }

    /**
    * 不允许从外部调用以防止创建多个实例
    * 要使用单例，必须通过 Singleton::getInstance() 方法获取实例
    */
    private function __construct()
    {
    }

    /**
    * 防止实例被克隆（这会创建实例的副本）
    */
    private function __clone()
    {
    }

    /**
    * 防止反序列化（这将创建它的副本）
    */
    private function __wakeup()
    {
    }
} 
```

## 测试
[Tests/SingletonTest.php](Tests/SingletonTest.php)
```php
<?php

namespace DesignPatterns\Creational\Singleton\Tests;

use DesignPatterns\Creational\Singleton\Singleton;
use PHPUnit\Framework\TestCase;

class SingletonTest extends TestCase
{
    public function testUniqueness()
    {
        $firstCall = Singleton::getInstance();
        $secondCall = Singleton::getInstance();

        $this->assertInstanceOf(Singleton::class, $firstCall);
        $this->assertSame($firstCall, $secondCall);
    }
} 
```
