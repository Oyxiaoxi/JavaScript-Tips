### Hello World
>当才华还支持不起理想时，就应该静下心来好好学习了。

### UML 类图
**UML类图**是一种结构图，用于描述一个系统的静态结构。类图以反映类结构和类之间关系为目的，用以描述软件系统的结构，是一种静态建模方法。类图中的类，**与面向对象语言中的类的概念是对应的**。

#### 1. 类机构
> UML图中，使用长方形描述一个类的主要构成，长方形垂直地分为三层，以此放置类的名称、属性和方法。

| User |
|:----|
|+ $name: String|
|# $age|
|- $email|
|-------------------|
|+ getName():String|
|# getAge()|
|- getEmail|

+ 抽象类名用斜体字粗体，如 *User* ，接口则需在上方加上 <<interface>>
+ 属性和方法都需要标注可见性符号，**+代表public**，**#代表protected**，**-代表private**。
+ 冒号:表明属性的类型和方法的返回类型，如 +$name:string、+getName():string。 当然，类型说明并非必须。

#### 2. 类关系
> 类与类之间的关系主要有六种：继承、实现、组合、聚合、关联和依赖。

![类关系图](https://github.com/Oyxiaoxi/Code-Tips/blob/master/Laravel/PHP-UML/images/arrow.png)

#### 3. 6种类关系
> **组合、聚合、关联**这三种类关系的代码结构一样，都是用属性来保存另一个类的引用，所以要通过内容间的关系来区别。

##### 3.1 继承
> 继承关系也称泛化关系（Generalization），用于描述父类与子类之间的关系。父类又称作基类，子类又称作派生类。  
> 继承关系中，子类继承父类的所有功能，父类所具有的属性、方法，子类应该都有。子类中除了与父类一致的信息以外，还包括额外的信息。

+ 例如：公交车、出租车和小轿车都是汽车，他们都有名称，并且都能在路上行驶。
![](https://github.com/Oyxiaoxi/Code-Tips/blob/master/Laravel/PHP-UML/images/realization.png)

```php
class Car {
    public $name;

    public function run()
    {
        return '在行驶中';
    }
}

class Bus extends Car
{
    public function __construct()
    {
        $this->name = '公交车';
    }
}

class Taxi extends Car
{
    public function __construct()
    {
        $this->name = '出租车';
    }
}

// 输出
$line2 = new Bus;
echo $line2->name . $line2->run();
```

##### 3.2 实现关系
> 实现关系（Implementation），主要用来规定接口和实现类的关系。  
> 接口（包括抽象类）是方法的集合，在实现关系中，类实现了接口，类中的方法实现了接口声明的所有方法。

+ 例如：汽车和轮船都是交通工具，而交通工具只是一个可移动工具的抽象概念，船和车实现了具体移动的功能。
![](https://github.com/Oyxiaoxi/Code-Tips/blob/master/Laravel/PHP-UML/images/implement.png)

```php
interface Vehicle {
    public function run()
}

class Car implements Vehicle {
    public $name = '汽车';

    public function run()
    {
        return $this->name . '在路上行驶';
    }
}

class Car implements Vehicle {
    public $name = '轮船';

    public function run()
    {
        return $this->name . '在海上航行';
    }
}

// 输出
$car = new Car;
echo $car->run();
```

##### 3.3 组合关系
> 组合关系（Composition）：整体与部分的关系，但是整体与部分不可以分开。
> 组合关系表示类之间整体与部分的关系，整体和部分有一致的生存期。一旦整体对象不存在，部分对象也将不存在，是同生共死的关系。

+ 例如：人由头部和身体组成，两者不可分割，共同存在。
![](https://github.com/Oyxiaoxi/Code-Tips/blob/master/Laravel/PHP-UML/images/composition.png)

```php
class Head {
    public $name = "头部";
}

class Body {
    public $name = "身体";
}

class Human {
    public $head $body;

    public function setHead(Head $head) {
        $this->head = $head;
    }

    public function setBody(Body $body) {
        $this->body = $body;
    }

    // sprintf 格式化字符串
    public function display() {
        return sprintf('人由%s和%s组成', $this->head->name, $this->body->name);
    }
}

// 输出
$man = new Human();
$man->setHead(new Head());
$man->setBody(new Body());
echo $man->display();
```

##### 3.4 聚合关系
> 聚合关系（Aggregation）：整体和部分的关系，整体与部分可以分开。  
> 聚合关系也表示类之间整体与部分的关系，成员对象是整体对象的一部分，但是成员对象可以脱离整体对象独立存在。  

+ 例如：公交车司机和工衣、工帽是整体与部分的关系，但是可以分开，工衣、工帽可以穿在别的司机身上，公交司机也可以穿别的工衣、工帽。  

![](https://github.com/Oyxiaoxi/Code-Tips/blob/master/Laravel/PHP-UML/images/aggregation.png)

```php
class Clothes {
    public $name = "衣服";
}

class Hat {
    public $name = '工帽';
}

class Driver {
    public $clothes $hat;

    public function wearClothes(Clothes $clothes)
    {
        $this->clothes = $clothes;
    }

    public function wearHat(Hat $hat)
    {
        $this->hat = $hat;
    }

    public function show()
    {
        return sprintf('公交车司机穿着%s和%s', $this->clothes->name, $this->hat->name);
    }
}

// 输出
$driver = new Driver();
$driver->wearClothes(new Clothes());
$driver->wearHat(new Hat());
echo $driver->show();
```

##### 3.5 关联关系
> 关联关系（Association）：表示一个类的属性保存了对另一个类的一个实例（或多个实例）的引用。  
> 关联关系是类与类之间最常用的一种关系，表示一类对象与另一类对象之间有联系。组合、聚合也属于关联关系，只是关联关系的类间关系比其他两种要弱。  

+ 关联关系有四种：双向关联、单向关联、自关联、多重数关联。
+ 例如：汽车和司机，一辆汽车对应特定的司机，一个司机也可以开多辆车。  

![](https://github.com/Oyxiaoxi/Code-Tips/blob/master/Laravel/PHP-UML/images/association.png)

```php
class Driver
{
    public $cars = array();
    public function addCar(Car $car)
    {
        $this->cars[] = $car;
    }
}

class Car
{
    public $drivers = array();
    public function addDriver(Driver $driver)
    {
        $this->drivers[] = $driver;
    }
}

// 输出
$jack = new Driver();
$line1 = new Car();
$jack->addCar($line1);
$line1->addDriver($jack);
print_r($jack);
```

> 在多重性关系中，可以直接在关联直线上增加一个数字，表示与之对应的另一个类的对象的个数。  

+ 1..1：仅一个
+ 0..*：零个或多个
+ 1..*：一个或多个
+ 0..1：没有或只有一个
+ m..n：最少m、最多n个 (m<=n)

##### 3.6 依赖关系
> 依赖关系（Dependence）：假设A类的变化引起了B类的变化，则说名B类依赖于A类。  
> 大多数情况下，依赖关系体现在某个类的方法使用另一个类的对象作为参数。  
> 依赖关系是一种“使用”关系，特定事物的改变有可能会影响到使用该事物的其他事物，在需要表示一个事物使用另一个事物时使用依赖关系。

+ 例如：汽车依赖汽油，如果没有汽油，汽车将无法行驶。  

![](https://github.com/Oyxiaoxi/Code-Tips/blob/master/Laravel/PHP-UML/images/dependence.png)

```php
class Oil
{
    public $type = '汽油';
    public function add()
    {
        return $this->type;
    }
}

class Car
{
    public function beforeRun(Oil $oil)
    {
        return '添加' . $oil->add();
    }
}

// 输出
$car = new Car;
echo $car->beforeRun(new Oil());
```

#### 总结
> 这六种类关系中，组合、聚合和关联的代码结构一样，可以从关系的强弱来理解，各类关系从强到弱依次是：继承→实现→组合→聚合→关联→依赖。  

![](https://github.com/Oyxiaoxi/Code-Tips/blob/master/Laravel/PHP-UML/images/whole.png)


##### Laravel中 app 下的 user 类的各种关系
![](https://github.com/Oyxiaoxi/Code-Tips/blob/master/Laravel/PHP-UML/images/bemogrppo.png)


#### 工具推荐
[UMLet](https://www.umlet.com/)