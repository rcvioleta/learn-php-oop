### Properties

Properties in a class are member variables that can be defined with various modifiers such as visibility (**public**, **protected**, **private**), the **static** keyword, or as **readonly** in PHP 8.1.0. In PHP 7.4 and later versions, properties can also have a type declaration, such as **string**, **integer**, or an **object**. They can be initialized with a constant value, but this initialization is optional.

```php
class Person {
  public $name;
  protected $age;
  private $email;
}
```

### Class Constants

Class constants in PHP are values associated with a class that remain constant. They are declared using the const keyword and cannot be changed once defined. Class constants provide a way to define meaningful, fixed values throughout the code and are accessed using the **"::"** operator. They are inherited by subclasses and offer a convenient way to use constant values in a class.

```php
class Circle {
  const PI = 3.14159;
}

echo Circle::PI; // Output: 3.14159
```

### Autoloading Classes

Assuming autoloading is set up properly

### Constructors and Destructors

Constructors and destructors are special methods in PHP classes that are used for initializing and cleaning up objects, respectively.

- Constructors are methods that are automatically called when an object is created from a class. They are used to initialize the object's properties or perform any necessary setup tasks. Constructors are defined with the **\_\_construct()** method name.
- Destructors, on the other hand, are methods that are automatically called when an object is no longer referenced or goes out of scope. They are used to perform cleanup operations, such as closing database connections or releasing resources. Destructors are defined with the **\_\_destruct()** method name.

Constructors and destructors provide a way to control the initialization and cleanup processes of objects, ensuring proper setup and resource management during the object's lifecycle.

**Note:** The **\_\_destruct()** method is optional and should only be defined if the class requires specific cleanup operations. If not explicitly defined, PHP's garbage collector will still handle the necessary cleanup for the object. However, if you need to perform any custom cleanup tasks, you can define the **\_\_destruct()** method within the class to implement the desired behavior.

```php
class Book {
  public function __construct() {
    echo "Book object created.";
  }

  public function __destruct() {
    echo "Book object destroyed.";
  }
}

$book = new Book(); // Output: Book object created.
unset($book); // Output: Book object destroyed.
```

### Visibility

Visibility defines the accessibility of class properties and methods from different parts of the code. There are three levels of visibility:

- **Public**: Public properties and methods can be accessed from anywhere, both inside and outside the class. In the given example, the $name property and the **getType()** method have public visibility, allowing them to be accessed directly.
- **Protected**: Protected properties and methods can only be accessed within the class itself and its subclasses (inheritance). They are not directly accessible from outside the class. In the example, the **$age** property is declared as protected, meaning it can only be accessed within the Animal class or its subclasses.
- **Private**: Private properties and methods are accessible only within the class where they are defined. They cannot be accessed from outside the class, including its subclasses. In the example, the **$type** property is declared as private, restricting direct access to it.

Using visibility modifiers allows for proper encapsulation and access control, ensuring that class members are accessed and manipulated appropriately based on their intended usage and level of exposure.

```php
class Animal {
  public $name;
  protected $age;
  private $type;

  public function __construct($name, $age, $type) {
    $this->name = $name;
    $this->age = $age;
    $this->type = $type;
  }

  public function getType() {
    return $this->type;
  }
}

$animal = new Animal('Lion', 5, 'Mammal');
echo $animal->getType(); // Output: Mammal
```

### Object Inheritance

In PHP, object inheritance allows you to create new classes (child classes) based on existing classes (parent classes). To establish inheritance in PHP, you can use the "**extends**" keyword followed by the name of the parent class when defining a child class. The child class can then add its own properties and methods or override the ones inherited from the parent class to tailor its behavior. This promotes hierarchical relationships, code organization, and the ability to specialize or modify behavior in child classes.

```php
class Shape {
  protected $color;

  public function __construct($color) {
    $this->color = $color;
  }
}

class Circle extends Shape {
  private $radius;

  public function __construct($color, $radius) {
    parent::__construct($color);
    $this->radius = $radius;
  }

  public function getArea() {
    return Shape::PI * $this->radius * $this->radius;
  }
}

$circle = new Circle('Red', 5);
echo $circle->getArea(); // Output: 78.53975
```

### Scope Resolution Operator "::"

The Scope Resolution Operator "**::**" is used to access **static methods**, **static properties**, and **constants** within a class. It allows you to refer to the class itself rather than an instance of the class. This operator helps differentiate between static and non-static elements of a class and provides a way to invoke or access them without needing an object instance.

```php
class MathUtils {
  public static function square($num) {
    return $num \* $num;
  }
}

echo MathUtils::square(3); // Output: 9
```

### Static Keyword

The "**static**" keyword is used to declare class members (properties and methods) that belong to the class itself rather than instances of the class. When a member is declared as static, it can be accessed directly through the class name, **without creating an object instance**. Static members are shared among all instances of the class and can be accessed and modified globally within the program. They are useful for creating utility functions, maintaining global state, and organizing code that doesn't require object-specific data.

```php
class Counter {
  public static $count = 0;

  public function __construct() {
    self::$count++;
  }
}

$counter1 = new Counter();
$counter2 = new Counter();
echo Counter::$count; // Output: 2
```

### Class Abstraction

Class abstraction in PHP refers to the concept of creating abstract classes. An abstract class **serves as a blueprint** for other classes and **cannot be instantiated itself**. It defines common properties and methods that subclasses can inherit and implement.

An abstract class may **contain abstract methods**, which are **declared** but **not implemented** in the abstract class itself. Subclasses that inherit from the abstract class must implement these abstract methods, providing their own specific implementation.

Abstract classes are useful when you want to define a common interface or behavior that multiple classes should adhere to, while allowing each subclass to have its own implementation details. They help in enforcing consistency and promoting code reusability.

```php
abstract class Animal {
  abstract public function makeSound();
}

class Dog extends Animal {
  public function makeSound() {
    echo "Woof!";
  }
}

$dog = new Dog();
$dog->makeSound(); // Output: Woof!
```

### Object Interfaces

Object interfaces in PHP define a contract or set of rules that a class must follow. An interface defines the method signatures (name, parameters, and return types) that implementing classes must adhere to.

Interfaces allow you to specify a common behavior that multiple classes can share, regardless of their inheritance hierarchy. Any class that implements an interface must provide an implementation for all the methods defined in that interface.

Interfaces provide a way to achieve **abstraction** and **polymorphism** in PHP. They allow you to define a common interface that different objects can adhere to, enabling you to work with objects of different classes through a unified interface.

By implementing interfaces, you can ensure that classes fulfill specific requirements and can be used interchangeably in certain contexts. This promotes code modularity, flexibility, and code reuse.

```php
interface Logger {
  public function log($message);
}

class FileLogger implements Logger {
  public function log($message) {
    echo "Logging message to file: {$message}";
  }
}

$fileLogger = new FileLogger();
$fileLogger->log("Error occurred."); // Output: Logging message to file: Error occurred.
```

### Traits

Traits are a mechanism for **code reuse** that allows you to define reusable sets of methods that can be used by multiple classes. A trait **encapsulates** a set of methods that can be included in a class, providing additional functionality without requiring inheritance.

Traits offer a way to horizontally extend classes, enabling **code sharing** among unrelated classes. Unlike inheritance, which follows a hierarchical structure, traits provide a way to **mix in functionalities across different classes**, promoting code modularity and reducing code duplication.

A class can use multiple traits by including them using the "**use**" keyword. When a class uses a trait, it gains access to all the methods defined within that trait, as if those methods were directly defined within the class itself. This allows classes to inherit methods from multiple traits and their own parent class simultaneously.

Traits provide a flexible way to enhance the functionality of classes, allowing you to reuse code across different classes without the limitations of single inheritance. They help in organizing and structuring code, making it more modular, maintainable, and reusable.

```php
trait Discountable {
  public function applyDiscount($amount) {
    return $amount \* 0.8;
  }
}

class Product {
  use Discountable;
  public $price;

  public function __construct($price) {
    $this->price = $price;
  }
}

$product = new Product(100);
echo $product->applyDiscount($product->price); // Output: 80
```

### Anonymous classes

Anonymous classes are classes that are defined without a named identifier. They are **created dynamically at runtime** and are useful when you need to create a simple class that is used only once and doesn't require a specific name.

Anonymous classes allow you to define a class on the fly without the need to declare it in a separate file. They are typically used when you need to create an object with customized behavior or to implement interfaces or extend classes inline.

You can define **properties**, **methods**, and **implement interfaces** directly within an anonymous class. It allows you to **encapsulate** the necessary code within a single block, avoiding the need for separate class definitions.

Anonymous classes are instantiated using the "**new**" keyword immediately after their definition. You can also pass arguments to the constructor if needed. Once instantiated, you can use the anonymous class just like any other object.

Anonymous classes provide a convenient way to create small, self-contained classes without the need for separate files or class declarations. They are particularly useful for implementing simple interfaces, providing ad-hoc implementations, and creating objects with customized behavior on the fly.

```php
interface Greeting {
  public function greet();
}

$greeting = new class implements Greeting {
  public function greet() {
    echo "Hello!";
  }
}

$greeting->greet(); // Output: Hello!
```

### Overloading

In PHP, overloading refers to the ability to dynamically "**overload**" methods or properties in a class. It allows a class to define multiple methods or properties with the same name but different parameters or behaviors. There are two types of overloading in PHP:

1. **Method Overloading**: PHP does not support method overloading in the same way as some other languages, such as Java. However, you can simulate method overloading by using the magic method "**\_\_call()**". This method is invoked when a non-accessible or undefined method is called on an object. You can then handle the method call dynamically based on the provided parameters.
2. **Property Overloading**: PHP supports property overloading using the magic methods "**get()**" and "**set()**". These methods are called when an inaccessible or undefined property is accessed or modified. They allow you to define custom logic to handle property access or modification dynamically.

By leveraging method overloading and property overloading, you can create more flexible and dynamic classes in PHP.

```php
class Example {
  private $data = [];

  public function __call($name, $arguments) {
    if ($name === 'doSomething') {
      // Handle doSomething method with different number of parameters
      if (count($arguments) === 1) {
        echo "Doing something with one parameter: " . $arguments[0] . "\n";
        return;
      }
      echo "Doing something with multiple parameters\n";
    }
  }

  public function __get($name) {
    if ($name === 'property') {
      return $this->data[$name];
    }
  }

  public function __set($name, $value) {
    if ($name === 'property') {
      $this->data[$name] = $value;
    }
  }
}

$example = new Example();
$example->doSomething("parameter"); // Output: Doing something with one parameter: parameter
$example->doSomething("parameter1", "parameter2"); // Output: Doing something with multiple parameters

$example->property = "Hello, world!";
echo $example->property; // Output: Hello, world!
```

### Object Iteration

```php
class Playlist implements IteratorAggregate {
  private $songs = [];

  public function __construct($songs) {
    $this->songs = $songs;
  }

  public function getIterator() {
    return new ArrayIterator($this->songs);
  }
}

$songs = ['Song 1', 'Song 2', 'Song 3'];
$playlist = new Playlist($songs);

foreach ($playlist as $song) {
  echo $song . "\n";
}
```

### Magic Methods

```php
class Person {
  private $name;

  public function __construct($name) {
    $this->name = $name;
  }

  public function __toString() {
    return $this->name;
  }
}

$person = new Person('John Doe');
echo $person; // Output: John Doe
```

### Final Keyword

```php
final class FinalClass {
  // Class definition
}
```

### Object Cloning

```php
class Car {
  public $brand;

  public function __construct($brand) {
    $this->brand = $brand;
  }
}

$car1 = new Car('Toyota');
$car2 = clone $car1;
$car2->brand = 'Honda';

echo $car1->brand; // Output: Toyota
echo $car2->brand; // Output: Honda
```

### Comparing Objects

```php
class Fruit {
  public $name;

  public function __construct($name) {
    $this->name = $name;
  }
}

$apple1 = new Fruit('Apple');
$apple2 = new Fruit('Apple');

if ($apple1 != $apple2) {
  return "Not Equal"; // Output: Not Equal
}

return "Equal";
```

### Late Static Bindings

```php
class ParentClass {
  public static function whoAmI() {
    echo static::class;
  }
}

class ChildClass extends ParentClass {
  // Inherits whoAmI() method
}

ChildClass::whoAmI(); // Output: ChildClass
```

### Objects and references

```php
$person1 = new Person('John');
$person2 = $person1;

$person2->name = 'Jane';

echo $person1->name; // Output: Jane
echo $person2->name; // Output: Jane
```

### Object Serialization

```php
class User {
  public $name;

  public function __construct($name) {
    $this->name = $name;
  }
}

$user = new User('John Doe');

// Serialize the object to a string
$serialized = serialize($user);

// Deserialize the string to an object
$deserialized = unserialize($serialized);

echo $deserialized->name; // Output: John Doe
```

### Covariance and Contravariance

```php
interface Animal {
  public function eat();
}

class Dog implements Animal {
  public function eat() {
    echo "Dog is eating.";
  }
}

interface Pet {
  public function play();
}

class PetDog extends Dog implements Pet {
  public function play() {
    echo "Dog is playing.";
  }
}

$dog = new PetDog();
$dog->eat(); // Output: Dog is eating.
$dog->play(); // Output: Dog is playing.
```
