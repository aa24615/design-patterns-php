# 原型模式（Prototype）

原型模式（Prototype Pattern）是用于创建重复的对象，同时又能保证性能。         
这种类型的设计模式属于创建型模式，它提供了一种创建对象的最佳方式。   
这种模式是实现了一个原型接口，该接口用于创建当前对象的克隆。当直接创建对象的代价比较大时，则采用这种模式。   
例如，一个对象需要在一个高代价的数据库操作之后被创建。     
我们可以缓存该对象，在下一个请求时返回它的克隆，在需要的时候更新数据库，以此来减少数据库调用。     

## 目地

- 相比正常创建一个对象 (new Foo () )，首先创建一个原型，然后克隆它会更节省开销。

## 例子

- 大数据量 (例如：通过 ORM 模型一次性往数据库插入 1,000,000 条数据) 。

## 代码

[BookPrototype.php](BookPrototype.php)

```php
<?php

namespace DesignPatterns\Creational\Prototype;

abstract class BookPrototype
{
    /**
    * @var string
    */
    protected $title;

    /**
    * @var string
    */
    protected $category;

    abstract public function __clone();

    public function getTitle(): string
    {
        return $this->title;
    }

    public function setTitle($title)
    {
        $this->title = $title;
    }
}
```

[BarBookPrototype.php](BarBookPrototype.php)

```php
<?php

namespace DesignPatterns\Creational\Prototype;

class BarBookPrototype extends BookPrototype
{
    /**
    * @var string
    */
    protected $category = 'Bar';

    public function __clone()
    {
    }
}
```

[FooBookPrototype.php](FooBookPrototype.php)

```php
<?php

namespace DesignPatterns\Creational\Prototype;

class FooBookPrototype extends BookPrototype
{
    /**
    * @var string
    */
    protected $category = 'Foo';

    public function __clone()
    {
    }
}
```


## 测试

[Tests/PrototypeTest.php](Tests/PrototypeTest.php)

```php
<?php

namespace DesignPatterns\Creational\Prototype\Tests;

use DesignPatterns\Creational\Prototype\BarBookPrototype;
use DesignPatterns\Creational\Prototype\FooBookPrototype;
use PHPUnit\Framework\TestCase;

class PrototypeTest extends TestCase
{
    public function testCanGetFooBook()
    {
        $fooPrototype = new FooBookPrototype();
        $barPrototype = new BarBookPrototype();

        for ($i = 0; $i < 10; $i++) {
            $book = clone $fooPrototype;
            $book->setTitle('Foo Book No ' . $i);
            $this->assertInstanceOf(FooBookPrototype::class, $book);
        }

        for ($i = 0; $i < 5; $i++) {
            $book = clone $barPrototype;
            $book->setTitle('Bar Book No ' . $i);
            $this->assertInstanceOf(BarBookPrototype::class, $book);
        }
    }
}
```
