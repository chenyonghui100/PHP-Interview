## 设计模式
### <div id="设计模式概述"> 设计模式概述</div> 
代码无错即是优？

#### 设计模式的六大原则
  1 开闭原则（Open Close Principle）

开闭原则的意思是：对扩展开放，对修改关闭。在程序需要进行拓展的时候，不能去修改原有的代码，实现一个热插拔的效果。简言之，是为了使程序的扩展性好，易于维护和升级。想要达到这样的效果，我们需要使用接口和抽象类，后面的具体设计中我们会提到这点。

  2 里氏代换原则（Liskov Substitution Principle）

里氏代换原则是面向对象设计的基本原则之一。 里氏代换原则中说，任何基类可以出现的地方，子类一定可以出现。LSP 是继承复用的基石，只有当派生类可以替换掉基类，且软件单位的功能不受到影响时，基类才能真正被复用，而派生类也能够在基类的基础上增加新的行为。里氏代换原则是对开闭原则的补充。实现开闭原则的关键步骤就是抽象化，而基类与子类的继承关系就是抽象化的具体实现，所以里氏代换原则是对实现抽象化的具体步骤的规范。

  3 依赖倒转原则（Dependence Inversion Principle）

这个原则是开闭原则的基础，具体内容：针对接口编程，依赖于抽象而不依赖于具体。

  4 接口隔离原则（Interface Segregation Principle）

这个原则的意思是：使用多个隔离的接口，比使用单个接口要好。它还有另外一个意思是：降低类之间的耦合度。由此可见，其实设计模式就是从大型软件架构出发、便于升级和维护的软件设计思想，它强调降低依赖，降低耦合。

  5 迪米特法则，又称最少知道原则（Demeter Principle）

最少知道原则是指：一个实体应当尽量少地与其他实体之间发生相互作用，使得系统功能模块相对独立。

  6 合成复用原则（Composite Reuse Principle）

合成复用原则是指：尽量使用合成 / 聚合的方式，而不是使用继承。
### <div id="单例模式"> 单例模式</div> 
>确保只创建了一个特别类的对象。保证类只实例化一次，减少开销。

```php
<?php
class Foo
{
    public $bar = 'A string';
    //1.创建一个静态私有属性,用于保存当前类的实例
    private static $instance = null;

    //2.构造方法私有化,从而禁止从外部用new关键字创建实例
    private function __construct()
    {

    }

    //3.克隆方法私有化,从而禁止从外部通过clone关键字创建实例
    private function __clone()
    {

    }

    //4.定义公共静态方法,用于生成当前类的实例
    public static function getInstance()
    {
        //如果$instance变量中保存的不是当前类的实例
        if (!self::$instance) {
            //那么就new一个当前类,并保存在$instance中
            self::$instance = new self();
        }
        //否则直接返回$instance
        return self::$instance;
    }
}
```
### <div id="工厂模式"> 简单工厂模式</div> 
>就是本来把实例化一个类换成了用工厂代替实例化，从而隐藏了具体的类

```php
interface Door
{
    function getWidth();
    function getHeight();
}

class WoodenDoor implements Door
{
    protected $height;
    protected $width;
    public function __construct($width, $height)
    {
        $this->height = $height;
        $this->width = $width;
    }
    public function getHeight()
    {
        return $this->height;
    }
    public function getWidth()
    {
        return $this->width;
    }
}

class DoorFactory
{
    public static function makeDoor($width, $height)
    {
        return new WoodenDoor($width, $height);
    }
}
```


​	
### <div id="IOC和DI"> 控制反转与依赖注入</div> 
IOC 控制反转
先说 IOC 和 DI 的区别吧！

IOC 是一种设计思想
DI 是一种设计模式
所以两者有本质上的区别。DI 是 IOC 的一种实现方法（还有 ServiceLocator 等其他设计模式）。
所谓的反转，主要指由 主动依赖 到 被动依赖 。

```php
//主动依赖
function __construct() {
        $this->user = new UserModel();
}

//被动依赖
function __construct(UserModel $user) {
        $this->user = $user;
}	
```

IOC 思想实现了高度解耦，那么，问题来了，如何管理这些分散的模块呢？这就是容器的任务了！

可以想象成，在 IOC 容器中，装着（注册）很多模块。当用户需要一个模块的时候，可以从中拿出来。当提取的模块依赖另一个模块的时候，容器会自动注入，再返回给用户（反射机制实现）。大批互相依赖的模块被完美解耦并统一管理了！
### <div id="观察者模式"> 观察者模式</div> 
>观察者模式，有时又被称为发布（publish ）- 订阅（Subscribe）模式。是软件设计模式的一种。在此种模式中，一个目标物件管理所有相依于它的观察者物件，并且在它本身的状态改变时主动发出通知。这通常透过呼叫各观察者所提供的方法来实现。此种模式通常被用来实现事件处理系统。

典型的观察者模式中会有两个角色，观察者和被观察者

**被观察者**

被观察对象发生了某种变化时，会从存有自己观察者的数组中得到所有注册过的观察者，然后调用观察者的指定方法，将变化通知观察者。

```php
<?php
class Paper
{ 
    private $observers = [];

    /*  注册观察者 */
    public function register($sub)
    { 
        $this->observers[] = $sub;
    }

    /*  外部统一访问    */
    public function trigger()
    { 
        if(! empty($this->observers)) {
            foreach($this->observers as $observer) {
                $observer->update();    //通知变化给观察者
            }
        }
    }
}
```

**观察者**

（Observer）将自己注册到被观察对象（Subject）中，被观察对象将观察者保存在自己类中的一个数组中。

```php
<?php
/**
 * 观察者要实现的接口
 */
interface Observerable
{
    public function update();
}
class Subscriber implements Observerable
{
    public function update(){
        echo "do something";
    }
}
```

然后在某个地方将观察者注册到被观察者上，需要的时候调用观察者的 trigger () 方法，通知观察者

```php
$paper = new Paper();
$paper->register(new Subscriber());	
$paper->trigger();
```
