## Table of Contents

- [PHP Classes and Objects](./README.md/#php-classes-and-objects)
  - [Properties](./README.md/#oop-properties)
  - [Class Constants](./README.md/#oop-class-constants)
  - [Autoloading Classes](./README.md/#oop-autoloading-classes)
  - [Constructors and Destructors](./README.md/#oop-constructors-and-destructors)
  - [Visibility](./README.md/#oop-visibility)
  - [Object Inheritance](./README.md/#oop-object-inheritance)
  - [Scope Resolution Operator \(::\)](./README.md/#oop-scope-resolution-operator)
  - [Static Keyword](./README.md/#oop-static-keyword)
  - [Class Abstraction](./README.md/#oop-class-abstraction)
  - [Object Interfaces](./README.md/#oop-object-interfaces)
  - [Traits](./README.md/#oop-traits)
  - [Anonymous classes](./README.md/#oop-anonymous-classes)
  - [Overloading](./README.md/#oop-overloading)
  - [Object Iteration](./README.md/#oop-object-iteration)
  - [Magic Methods](./README.md/#oop-magic-methods)
  - [Final Keyword](./README.md/#oop-final-keyword)
  - [Object Cloning](./README.md/#oop-object-cloning)
  - [Comparing Objects](./README.md/#oop-comparing-objects)
  - [Late Static Bindings](./README.md/#oop-late-static-bindings)
  - [Objects and references](./README.md/#oop-objects-and-references)
  - [Object Serialization](./README.md/#oop-object-serialization)
  - [Covariance and Contravariance](./README.md/#oop-covariance-and-contravariance)
- [PHP OOP Advance Concepts](./README.md/#oop-advance-concepts)
  - [Dependency Injection](./README.md/#oop-advance-dependency-injection)
  - [Repository Pattern](./README.md/#oop-advance-repository-pattern)
- [S.O.L.I.D Principles](./README.md/#solid-principles)
  - [Single-responsibility principle](./README.md/#solid-1)
  - [Open-closed principle](./README.md/#solid-2)
  - [Liskov substitution principle](./README.md/#solid-3)
  - [Interface segregation principle](./README.md/#solid-4)
  - [Dependency inversion principle](./README.md/#solid-5)

## <span id="php-classes-and-objects">PHP Classes and Objects</span>

### <span id="oop-properties">Properties</span>

Properties in a class are member variables that can be defined with various modifiers such as visibility (**public**, **protected**, **private**), the **static** keyword, or as **readonly** in PHP 8.1.0. In PHP 7.4 and later versions, properties can also have a type declaration, such as **string**, **integer**, or an **object**. They can be initialized with a constant value, but this initialization is optional.

```php
class Person {
  public $name;
  protected $age;
  private $email;
}
```

### <span id="oop-class-constants">Class Constants</span>

Class constants in PHP are values associated with a class that remain constant. They are declared using the const keyword and cannot be changed once defined. Class constants provide a way to define meaningful, fixed values throughout the code and are accessed using the **"::"** operator. They are inherited by subclasses and offer a convenient way to use constant values in a class.

```php
class Circle {
  const PI = 3.14159;
}

echo Circle::PI; // Output: 3.14159
```

### <span id="oop-autoloading">Autoloading Classes</span>

Many developers writing object-oriented applications create one PHP source file per class definition. One of the biggest annoyances is having to write a long list of needed includes at the beginning of each script (one for each class).

The **spl_autoload_register()** function registers any number of autoloaders, enabling for classes and interfaces to be automatically loaded if they are currently not defined. By registering autoloaders, PHP is given a last chance to load the class or interface before it fails with an error.

Any class-like construct may be autoloaded the same way. That includes classes, interfaces, traits, and enumerations.

This example attempts to load the classes **MyClass1** and **MyClass2** from the files **MyClass1.php** and **MyClass2.php** respectively.

```php
spl_autoload_register(function ($class_name) {
  include $class_name . '.php';
});

$obj  = new MyClass1();
$obj2 = new MyClass2();
```

**Note**: **spl_autoload_register()** may be called multiple times in order to register multiple autoloaders. Throwing an exception from an autoload function, however, will interrupt that process and not allow further autoload functions to run. For that reason, throwing exceptions from an autoload function is strongly discouraged.

### <span id="oop-constructors-and-destructors">Constructors and Destructors</span>

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

### <span id="oop-visibility">Visibility</span>

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

### <span id="oop-inheritance">Object Inheritance</span>

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

### <span id="oop-scope-resolution-operator">Scope Resolution Operator "::"</span>

The Scope Resolution Operator "**::**" is used to access **static methods**, **static properties**, and **constants** within a class. It allows you to refer to the class itself rather than an instance of the class. This operator helps differentiate between static and non-static elements of a class and provides a way to invoke or access them without needing an object instance.

```php
class MathUtils {
  public static function square($num) {
    return $num \* $num;
  }
}

echo MathUtils::square(3); // Output: 9
```

### <span id="oop-static-keyword">Static Keyword</span>

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

### <span id="oop-class-abstraction">Class Abstraction</span>

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

### <span id="oop-object-interfaces">Object Interfaces</span>

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

### <span id="oop-traits">Traits</span>

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

### <span id="oop-anonymous-classes">Anonymous classes</span>

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

### <span id="oop-overloading">Overloading</span>

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

### <span id="oop-object-iteration">Object Iteration</span>

Object iteration in PHP refers to the process of traversing and accessing the properties and values of an object. When iterating over an object, you can access its properties and perform operations on them.

In the example below, we have a "**Person**" class with three properties: "**name**", "**age**", and "**gender**". We create a new instance of the Person class and assign values to its properties. Then, we use a "**foreach**" loop to iterate over the object "**\$person**". During each iteration, the "**\$property**" variable will hold the name of the property, and the $value variable will hold the corresponding value. We can then perform any desired operations or display the property and its value.

Please note that in order to iterate over an object using "**foreach**", the object must implement the "**Traversable**" interface or have properties accessible from outside the class scope.

```php
class Person {
  public $name;
  public $age;
  public $gender;

  public function __construct($name, $age, $gender) {
    $this->name = $name;
    $this->age = $age;
    $this->gender = $gender;
  }
}

$person = new Person('John Doe', 30, 'Male');

foreach ($person as $property => $value) {
  echo "$property: $value\n";
}
```

### <span id="oop-magic-methods">Magic Methods</span>

Magic methods in PHP are special methods that provide functionality to classes and objects that goes beyond regular method definitions. These methods are automatically invoked by PHP in response to certain events or actions. They are prefixed with a double underscore ( \_ \_ ) to indicate their special nature.

Here are some magic methods in PHP:

- **\_\_construct()**: This is the constructor method that is automatically called when a new instance of a class is created. It allows you to initialize the object and perform any necessary setup.
- **\_\_toString()**: This method is called when an object is treated as a string, such as when using echo or print on the object. It allows you to define how the object should be represented as a string.

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

### <span id="oop-final-keyword">Final Keyword</span>

In PHP, the "**final**" keyword is used to indicate that _a class, method, or property cannot be overridden or extended by subclasses_. When a class or method is declared as final, it means that it cannot be further subclassed or overridden.

Here's how the "**final**" keyword can be used:

1. **Final Class**: When a class is declared as "\*\*final\*\*", it cannot be extended by any other class. This means that it cannot be used as a parent class, and no subclasses can be created for it.

```php
final class FinalClass {
  // Class implementation
}
```

2. **Final Method**: When a method is declared as final within a class, it cannot be overridden by any subclass. This ensures that the behavior of the method remains unchanged throughout the inheritance hierarchy.

```php
class ParentClass {
  final public function finalMethod() {
    // Method implementation
  }
}

class ChildClass extends ParentClass {
  // Error: Cannot override final method
}
```

3. **Final Property**: The final keyword is not applicable to properties directly. However, it can indirectly affect properties when used in conjunction with a final class. Since a final class cannot be extended, its properties are also not subject to further modification or extension.

```php
final class FinalClass {
  public $finalProperty;
}

class ChildClass extends FinalClass {
  // Error: Cannot extend final class
}
```

By using the final keyword appropriately, you can enforce restrictions on class inheritance, method overriding, and property modification, ensuring the stability and integrity of your code.

### <span id="oop-object-cloning">Object Cloning</span>

Object cloning in PHP allows you to create a duplicate or clone of an existing object. When an object is cloned, a new instance is created with the same properties and values as the original object. However, they are separate instances in memory, and modifications to one object do not affect the other.

To clone an object in PHP, you can use the "**clone**" keyword followed by the object you want to clone. The process of cloning involves two steps:

1. **Shallow Copy**:

- By default, object cloning in PHP performs a shallow copy. It means that the properties of the cloned object are copied by value, but if the properties are references to other objects, the references themselves are copied, not the referenced objects.
- This means that changes made to the referenced objects within the cloned object will also affect the original object and vice versa, as they both point to the same object.

```php
class MyClass {
  public $property;
}

$obj1 = new MyClass();
$obj1->property = 'Value';

$obj2 = clone $obj1;

$obj2->property = 'New Value';

var_dump($obj1->property);  // Output: string(4) "Value"
var_dump($obj2->property);  // Output: string(9) "New Value"
```

2. **Deep Copy**:

- If you want to create an independent clone of an object, including all referenced objects, you need to implement the \_\_clone() magic method within the class.
- In the \_\_clone() method, you can explicitly clone any referenced objects and assign them to the corresponding properties of the cloned object.

```php
class MyClass {
  public $property;

  public function __clone() {
    $this->property = clone $this->property;
  }
}

$obj1 = new MyClass();
$obj1->property = new AnotherClass();

$obj2 = clone $obj1;

$obj2->property->value = 'New Value';

var_dump($obj1->property->value);  // Output: NULL
var_dump($obj2->property->value);  // Output: string(9) "New Value"
```

**Note**: It's important to note that cloning an object can have performance implications, especially when dealing with complex objects or large data structures. Therefore, it's recommended to use object cloning judiciously and consider the potential impact on memory usage and performance.

### <span id="oop-comparing-objects">Comparing Objects</span>

When comparing objects in PHP, there are two types of comparison to consider: identity comparison and value comparison.

1. **Identity Comparison**:

- Identity Comparison is performed using the === operator, which checks if two objects refer to the exact same instance. It compares the memory address of the objects rather than their contents.
- Two objects are considered identical (===) if they are references to the same instance in memory.

```php
$obj1 = new MyClass();
$obj2 = $obj1;

var_dump($obj1 === $obj2);  // Output: bool(true)
```

2. **Value Comparison**:

- Value comparison involves comparing the properties and values of two objects to determine if they are considered equal based on the specific criteria defined in the code.
- Value comparison typically involves comparing individual properties or using custom comparison logic implemented in methods within the class.

```php
class MyClass {
  private $property;

  public function __construct($property) {
    $this->property = $property;
  }

  public function isEqual(MyClass $other) {
    return $this->property === $other->property;
  }
}

$obj1 = new MyClass('value');
$obj2 = new MyClass('value');

var_dump($obj1->isEqual($obj2));  // Output: bool(true)
```

It's important to note that by default, PHP's built-in comparison operators (== and !=) perform the identity comparison for objects. To perform a value comparison, you need to implement custom comparison logic within the class by overriding methods such as \_\_toString(), \_\_equals(), or defining your own comparison methods.

Keep in mind that object comparison depends on the specific implementation and comparison criteria defined within the class, so the behavior may vary depending on how the class is designed.

### <span id="oop-late-static-bindings">Late Static Bindings</span>

Late static binding is a feature in PHP that allows a static method in a class to reference the class that it is called from, rather than the class in which it is defined. It enables you to achieve polymorphic behavior when calling static methods.

In PHP, normally, when a static method is called within a class hierarchy, the method is resolved based on the class where it is defined, not the class from which it is called. This behavior is known as early binding. However, with late static binding, you can override this behavior and bind the static method at runtime.

To use late static binding, you need to use the static keyword instead of the class name within the static method. This allows the method to be dynamically resolved based on the calling class. Late static binding enables you to access static properties and methods of the class that called the method.

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

In the example above, "**ParentClass**" defines a static method "**whoAmI()**", which uses "**static::class**" to print the name of the calling class. When "**whoAmI()**" is called from "**ChildClass**" late static binding is applied, and **"ChildClass**" is printed.

Late static binding is particularly useful when implementing static methods in abstract classes or when working with class inheritance, allowing child classes to override static methods and still reference their own class context.

### <span id="oop-objects-and-references">Objects and References</span>

Objects and references are closely related concepts that play a significant role in how data is stored and accessed.

1. **Objects**:

- Objects are instances of classes, representing a specific entity or concept in your application.
- Each object has its own set of properties (variables) and methods (functions) defined by its class.
- Objects store their data internally and can hold complex data structures.
- When you assign an object to a variable or pass it as a parameter, the variable holds a reference to the object, rather than the actual object data.

2. **References**:

- References allow multiple variables to refer to the same underlying data.
- When you assign an object to a variable or pass it as a function argument, you are creating a reference to that object.
- Changes made to the object through one reference will be reflected in other references to the same object.
- References are particularly useful when working with large objects to avoid unnecessary memory duplication.

```php
$person1 = new Person('John');
$person2 = $person1;

$person2->name = 'Jane';

echo $person1->name; // Output: Jane
echo $person2->name; // Output: Jane
```

### <span id="oop-object-serialization">Object Serialization</span>

Object serialization in PHP refers to the process of converting an object into a format that can be stored or transmitted, and later reconstructed back into an object. Serialization allows objects to be saved persistently or sent across different systems or platforms.

Serialization is useful in various scenarios, such as:

1. **Storing Objects**: You can serialize an object and save it to a file or a database. Later, you can retrieve the serialized data and unserialize it to recreate the original object.
2. **Interprocess Communication**: Objects can be serialized and sent over a network or between different processes to exchange data or state information.
3. **Caching**: Serialized objects can be cached in memory or on disk for faster retrieval, reducing the need to recreate the object from scratch.

PHP provides built-in functions for object serialization:

- **serialize()**: This function converts an object into a string representation (serialization). It traverses the object's properties and converts them into a serialized format.
- **unserialize()** This function recreates an object from its serialized string representation. It reconstructs the object's properties and their values.

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

### <span id="oop-covariance-and-contravariance">Covariance and Contravariance</span>

Covariance and contravariance are concepts related to the type compatibility of objects or functions when dealing with inheritance or type hierarchies. They describe how the type relationships between different objects or functions can be preserved or modified in relation to their parameter types and return types.

1. **Covariance**:

- Covariance allows a method in a derived class to have a return type that is more specific than the return type of the same method in the base class. In other words, it allows a method to return a subtype of the original return type.

In the example, the **play()** method in the **PetDog** class is an example of covariance. It is implementing the **play()** method from the **Pet** interface, which is more specific than the **eat()** method from the **Animal** interface. The **play()** method returns a more specific result related to playing, which is allowed in covariance.

2. **Contravariance**:

- Contravariance allows a method in a derived class to accept parameters of a type that is less specific than the parameter type of the same method in the base class. In other words, it allows a method to accept a supertype of the original parameter type.

In the example, there is no explicit use of contravariance. However, if we had a method in the **Pet** interface that accepts an **Animal** parameter, and the **PetDog** class implemented that method by accepting a **Dog** parameter (which is a subtype of **Animal**), it would be an example of contravariance.

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

To summarize, covariance allows a more specific return type in a derived class, while contravariance allows a less specific parameter type in a derived class. These concepts help ensure type compatibility and flexibility in object-oriented programming languages.

## <span id="oop-advance-concepts">PHP OOP Advance Concepts</span>

### <span id="oop-advance-dependency-injection">Dependency Injection</span>

Dependency injection is a concept in PHP where the dependencies of a class are provided from the outside rather than being created internally within the class. This allows for better flexibility, testability, and decoupling of components.

By using dependency injection, the code becomes more modular and easier to test. It also promotes the **Single Responsibility Principle (SRP)** by ensuring that each class has a single responsibility and relies on external dependencies for other tasks.

In the example below, the **User** class **depends** on the **Logger** class for logging purposes. Instead of creating an instance of the **Logger** class within the **User** class, the dependency is injected through the constructor of the **User** class. This allows for better flexibility, as different implementations of the **Logger** class can be easily substituted.

```php
class Logger {
  public function log() {
    echo "[Logger Class] logging activities...";
  }
}

class User {
  private $logger;

  public function __construct(Logger $logger) {
    $this->logger = $logger;
  }

  public function searchById($username) {
    echo "[User Class] searchById logic";

    $this->logger->log();
  }
}

/* Output:
[User Class] searchById logic.
[Logger Class] Logging activities...
*/
```

### <span id="oop-advance-repository-pattern">Repository Pattern</span>

The repository pattern is a **software design pattern** that separates the **data access logic** from the **business logic** in an application. It provides an abstraction layer between the application and the underlying data source (such as a database), allowing for decoupling and easier maintenance.

In the given example, the repository pattern is implemented for the User entity. Here's an explanation of each component:

1. **UserInterface**: This is an interface that defines the contract for interacting with user data. It specifies two methods: **getAllUsers()** and **deleteUser(int $id)**. Any class that implements this interface must provide the implementation for these methods.

```php
# UserInterface.php
interface UserInterface {
  public function getAllUsers() : array;
  public function deleteUser(int $id): void;
}
```

2. **UserRepository**: This class implements the **UserInterface** and serves as the concrete implementation of the **data access logic** for the User entity. In this example, the user data is stored in an array. The **getAllUsers()** method returns all the users, and the **deleteUser(int $id)** method handles the deletion of a user.

```php
# UserRepository.php

class UserRepository implements UserInterface {
  private $users = ['Jane', 'John', 'Goku', 'Vegeta'];

  public function getAllUsers() : array {
    return $this->users;
  }

  public function deleteUser(int $id) {
    // delete user logic
  }
}
```

3. **UserService**: This class represents the **business logic layer**. It depends on the **UserInterface** (in this case, the **UserRepository**) through **dependency injection**. The constructor injection allows the **UserService** to work with any implementation of the **UserInterface**. It provides a method **getAllUsers()** that delegates the call to the repository to fetch all users.

```php
# UserService.php

class UserService {
  private $userRepository;

  public function __construct(UserInterface $userRepo) {
    $this->userRepository = $userRepo;
  }

  public function getAllUsers() : array {
    return $this->userRepository->getAllUsers();
  }
}

$userRepository = new UserRepository();
$userService = new UserService($userRepository);
print_r($userService->getAllUsers());
```

## <span id="#solid-principles">S.O.L.I.D principles</span>

### <span id="solid-1">Single responsibility principle</span>

In this example, there are two classes, **UserManager** and **UserNotifier**, each representing a separate responsibility.

The **UserManager** class handles operations related to user management, such as creating a new user, deleting a user, and updating user information. It encapsulates the logic for user-related operations, ensuring that each method focuses on a single responsibility related to user management.

The **UserNotifier** class, on the other hand, is responsible for sending notifications to users. It contains methods for sending welcome emails and password reset links. By separating the notification logic into its own class, we adhere to the SRP, as the class has a single responsibility of handling user notifications.

```php
class UserManager
{
  public function createUser($userData)
  {
    // Validation and creation logic for a new user
  }

  public function deleteUser($userId)
  {
    // Deletion logic for a user
  }

  public function updateUser($userId, $userData)
  {
    // Update logic for a user
  }
}

class UserNotifier
{
  public function sendWelcomeEmail($userEmail)
  {
    // Sending welcome email to a user
  }

  public function sendPasswordResetLink($userEmail)
  {
    // Sending password reset link to a user
  }
}
```

By keeping each class focused on a specific responsibility, the code becomes more modular, maintainable, and easier to understand and test. Applying the **Single Responsibility Principle** helps prevent a class from becoming bloated with unrelated responsibilities and reduces the impact of changes in one area on other parts of the codebase.

### <span id="solid-2">Open-closed principle</span>

The **Open-Closed Principle (OCP)** states that software entities (classes, modules, functions, etc.) should be **open for extension** but **closed for modification**. In other words, you should be able to extend the behavior of a system without modifying its existing code.

Here's an example in PHP that demonstrates the **Open-Closed Principle**:

```php
interface Shape
{
  public function area();
}

class Rectangle implements Shape
{
  public function __construct(private int $width, private int $height)
  {
    // Constructor property promotion 😃
  }

  public function area()
  {
    return $this->width * $this->height;
  }
}

class Circle implements Shape
{
  public function __construct(private int $radius)
  {
    // Constructor property promotion 😃
  }

  public function area()
  {
    return pi() * $this->radius * $this->radius;
  }
}
```

In this example, the **Shape** interface defines a contract that specifies the "**area()**" method. The **Rectangle** and **Circle** classes implement the **Shape** interface and provide their **own implementations** of the "**area()**" method.

The key aspect of the **Open-Closed Principle** is that you can introduce new shapes (such as a triangle or square) by creating new classes that implement the **Shape** interface without modifying the existing **Rectangle** and **Circle** classes. This allows the system to be easily extended to support new shapes without altering the code that depends on the existing shapes.

By adhering to the **Open-Closed Principle**, you achieve code that is more robust, maintainable, and flexible. It promotes code reuse and allows for easier extension of the system's functionality without the risk of introducing bugs or breaking existing code.

### <span id="solid-3">Liskov substitution principle</span>

The **Liskov Substitution Principle (LSP)** states that objects of a superclass should be replaceable with objects of its subclasses without affecting the correctness of the program. In other words, if S is a subtype of T, then objects of type T can be replaced with objects of type S without causing any errors or violating the expected behavior of the program.

Here's an example in PHP that demonstrates the **Liskov Substitution Principle**:

```php
class Vehicle
{
  public function startEngine()
  {
    // Start the engine
  }

  public function accelerate()
  {
    // Accelerate the vehicle
  }
}

class Car extends Vehicle
{
  public function startEngine()
  {
    // Start the car's engine
  }

  public function accelerate()
  {
    // Accelerate the car
  }

  public function openTrunk()
  {
    // Open the car's trunk
  }
}

class Motorcycle extends Vehicle
{
  public function startEngine()
  {
    // Start the motorcycle's engine
  }

  public function accelerate()
  {
    // Accelerate the motorcycle
  }

  public function tilt()
  {
  // Tilt the motorcycle
  }
}
```

In this example, we have a **Vehicle** superclass and two subclasses: **Car** and **Motorcycle**. Both subclasses inherit from the **Vehicle** class and provide their own implementations of the **startEngine()** and **accelerate()** methods. The **Car** class also introduces a new method, **openTrunk()**, while the **Motorcycle** class introduces a new method, **tilt()**.

The **Liskov Substitution Principle** ensures that you can use objects of the **Car** and **Motorcycle** classes interchangeably with objects of the **Vehicle** class without causing any issues. For example, if you have a method that expects a **Vehicle** object, you can safely pass a **Car** or **Motorcycle** object to that method without violating the expected behavior of the program.

Adhering to the **Liskov Substitution Principle** promotes code reusability, modularity, and polymorphism. It allows for more flexible and extensible code, as new subclasses can be introduced without affecting the existing code that depends on the superclass.

### <span id="solid-4">Interface segregation principle</span>

Imagine you have an interface called "**ElectronicDevice**" that represents different electronic devices. The interface has methods like **turnOn()**, **turnOff()**, **playSound()**, and **displayContent()**. However, not all electronic devices can perform all of these actions. Some devices, like a **TV**, can turn on and off, play sound, and display content. Other devices, like a speaker, can only play sound.

According to the **Interface Segregation Principle (ISP)**, it's better to have smaller and more focused interfaces instead of one large interface that encompasses all possible actions. So, we can split the **ElectronicDevice** interface into smaller, more specific interfaces:

```php
interface PowerSwitchable {
  public function turnOn();
  public function turnOff();
}

interface SoundPlayable {
  public function playSound();
}

interface ContentDisplayable {
  public function displayContent();
}
```

Now, we can have classes that implement only the interfaces they need:

```php
class TV implements PowerSwitchable, SoundPlayable, ContentDisplayable {
  // Implement methods for turning on, turning off, playing sound, and displaying content
}

class Speaker implements SoundPlayable {
  // Implement method for playing sound
}
```

In this example, the **TV** class implements all three interfaces because it can perform all the related actions. On the other hand, the **Speaker** class only implements the **SoundPlayable** interface since it's not concerned with turning on/off or displaying content.

By following the **Interface Segregation Principle**, we create interfaces that are focused on specific behaviors, allowing classes to implement only the interfaces relevant to them. This promotes code flexibility, modularity, and avoids forcing classes to implement unnecessary methods. It also helps to prevent code bloat and reduces the impact of changes in one part of the system on unrelated parts.

### <span id="solid-5">Dependency inversion principle</span>

Suppose we have a **MusicPlayer** class that needs to play music. Initially, the **MusicPlayer** class directly depends on a concrete implementation of the **Mp3Player** class, like this:

```php
class MusicPlayer {
  public function __construct(private Mp3Player $mp3Player) {
    // Constructor property promotion 😀
  }

  public function playMusic() {
    $this->mp3Player->play();
  }
}

class Mp3Player {
  public function play() {
    // Logic for playing music in an MP3 player
  }
}
```

However, this implementation violates the **Dependency Inversion Principle** because the **MusicPlayer** class is tightly coupled to the specific implementation of the **Mp3Player**. If we decide to change the music player implementation to a different type, such as a **SpotifyPlayer**, we would need to modify the **MusicPlayer** class, which can lead to code fragility and dependencies on concrete implementations.

To adhere to the **Dependency Inversion Principle**, we can introduce an abstraction in the form of an interface or an abstract class that both the **MusicPlayer** and the different player implementations can depend on:

```php
interface MusicPlayerInterface {
  public function play();
}

class MusicPlayer {
  public function __construct(private MusicPlayerInterface $player) {
    // Constructor property promotion 😀
  }

  public function playMusic() {
    $this->player->play();
  }
}

class Mp3Player implements MusicPlayerInterface {
  public function play() {
    // Logic for playing music in an MP3 player
  }
}

class SpotifyPlayer implements MusicPlayerInterface {
  public function play() {
    // Logic for playing music in a Spotify player
  }
}
```

Now, the **MusicPlayer** class depends on the **MusicPlayerInterface** abstraction instead of a specific implementation. We can inject different player implementations, such as **Mp3Player** or **SpotifyPlayer**, into the **MusicPlayer** class, allowing for more flexibility and extensibility.

By applying the **Dependency Inversion Principle**, we invert the dependencies, rely on abstractions, and decouple the high-level **MusicPlayer** class from the low-level implementations. This promotes modularity, reusability, and makes it easier to introduce new player implementations or swap existing ones without modifying the core **MusicPlayer** class.
