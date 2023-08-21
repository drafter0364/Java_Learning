# Java Learning - 面向对象基础

## 定义class

创建一个名为Person的类：

```java
class Person {
    public String name;
    public int age;
}
```



一个`class`可以包含多个字段/域（`field`）。

`public`是用来修饰字段的，它表示这个字段可以被外部访问。

## 创建实例

```java
Person ming = new Person(); //变量`ming`指向新创建的实例
```



一个Java源文件可以包含多个类的定义，但只能定义一个public类，且public类名必须与文件名一致。如果要定义多个public类，必须拆到多个Java源文件中。

##### 练习1

请定义一个City类，该class具有如下字段:

- name: 名称，String类型
- latitude: 纬度，double类型
- longitude: 经度，double类型

```java
public class Main {
    public static void main(String[] args) {
        City bj = new City();
        bj.name = "Beijing";
        bj.latitude = 39.903;
        bj.longitude = 116.401;
        System.out.println(bj.name);
        System.out.println("location: " + bj.latitude + ", " + bj.longitude);
    }
}

class City {
    public String name;
    public double latitude;
    public double longitude;
}
```



## 1 方法

修饰`field`

- public
- private：避免外部代码直接去访问`field`
- ...

❓ 如何让外部代码间接修改`field`

🚨使用方法（`method`）

```java
public class Main {
    public static void main(String[] args) {
        Person ming = new Person();
        ming.setName("Xiao Ming"); // 设置name
        ming.setAge(12); // 设置age
        System.out.println(ming.getName() + ", " + ming.getAge());
    }
}

class Person {
    private String name;
    private int age;
    
    public String getName() {
    	return this.name;
	}//定义Person类

	public void setName(String name) {
        if (name == null || name.isBlank()) {
        	throw new IllegalArgumentException("invalid name");
    	}
    	this.name = name.strip(); // 去掉首尾空格
        /*this.name 表示当前对象的成员变量 name。
        (在main中调用时 this 即为 ming )
		name.strip() 是调用了 name 字符串对象的 strip() 方法。
		strip() 方法用于去除字符串两端的空格。
		= 表示赋值操作，将 name.strip() 的结果赋值给 this.name。*/
	}//设置name域

	public int getAge() {
    	return this.age;
	}//设置age域

	public void setAge(int age) {
    	if (age < 0 || age > 100) {
        	throw new IllegalArgumentException("invalid age value");
    	}
    	this.age = age;
	}
}
```

:bulb:**一个类通过定义方法，就可以给外部代码暴露一些操作的接口，同时，内部自己保证逻辑一致性。**

:red_circle: 调用方法的语法是`实例变量.方法名(参数);`

### 1.1 定义方法

从上面的代码可以看出，定义方法的语法是：

```
修饰符 方法返回类型 方法名(方法参数列表) {
    若干方法语句;
    return 方法返回值;
}
```

### 1.2 private方法

`private`方法不允许外部调用，而**内部方法是可以调用`private`方法的**。

例如：

```java
public class Main {
    public static void main(String[] args) {
        Person ming = new Person();
        ming.setBirth(2008);
        System.out.println(ming.getAge());
    }
}

class Person {
    private String name;
    private int birth;

    public void setBirth(int birth) {
        this.birth = birth;
    }

    public int getAge() {
        return calcAge(2019); // 调用private方法
    }

    // private方法:
    private int calcAge(int currentYear) {
        return currentYear - this.birth;
    }
}

```

这个`Person`类只定义了`birth`字段，没有定义`age`字段，获取`age`时，通过方法`getAge()`返回的是一个实时计算的值，并非存储在某个字段的值。这说明方法可以**封装**一个类的对外接口，调用方不需要知道也不关心`Person`实例在内部到底有没有`age`字段。

### 1.3 this变量

隐含的变量`this`，始终指向当前实例。

如果没有命名冲突，可以省略`this`：

```java
class Person {
    private String name;

    public String getName() {
        return name; // 相当于this.name
    }
}
```

如果有局部变量和字段重名，那么局部变量优先级更高，就必须加上`this`：

```java
class Person {
    private String name;

    public void setName(String name) {
        this.name = name; // 前面的this不可少，少了就变成局部变量name了
    }
}
```

关于`this`测试：

:large_blue_circle: 含`this`：

```java
public class Main {
    public static void main(String[] args) {
        Person ming = new Person();
        ming.setName("ming");
        System.out.println(ming.Getname());
    }
}

class Person {
    private String name;

    public void setName(String name) {
        this.name = name;
    }
    
    public String Getname(){
        return this.name;
    }
}
//输出 ming
```

:red_circle: 不含`this`：

```java
public class Main {
    public static void main(String[] args) {
        Person ming = new Person();
        ming.setName("mingdakslk");
        System.out.println(ming.Getname());
    }
}

class Person {
    private String name;

    public void setName(String name) {
        name = name;
    }
    
    public String Getname(){
        return this.name;
    }
}
//输出 null，因为没有正确setName
```

### 1.4 方法参数

方法可以包含0个或任意个参数。

#### 可变参数

用`类型...`定义，可变参数相当于数组类型：

```java
class Group {
    private String[] names;

    public void setNames(String... names) {
        this.names = names;
    }
}
```

```java
Group g = new Group();
g.setNames("Xiao Ming", "Xiao Hong", "Xiao Jun"); // 传入3个String
g.setNames("Xiao Ming", "Xiao Hong"); // 传入2个String
g.setNames("Xiao Ming"); // 传入1个String
g.setNames(); // 传入0个String
```

:exclamation:：1. 调用方需要自己先构造`String[]`

​		2. 可变参数可以保证无法传入`null`，因为传入0个参数时，接收到的实际值是一个空数组而不是`null`。

#### 参数绑定

基本类型参数的传递，是调用方值的复制。双方各自的后续修改，互不影响。

引用类型参数的传递，调用方的变量，和接收方的参数变量，指向的是同一个对象。双方任意一方对这个对象的修改，都会影响对方（因为指向同一个对象）。

## 2 构造方法

在创建对象实例时就把内部字段全部初始化为合适的值

```java
public class Main {
    public static void main(String[] args) {
        Person p = new Person("Xiao Ming", 15);
        System.out.println(p.getName());
        System.out.println(p.getAge());
    }
}

class Person {
    private String name;
    private int age;

    // 构造方法，名称就是类名
    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }
    
    public String getName() {
        return this.name;
    }

    public int getAge() {
        return this.age;
    }
}
```

:exclamation:：1. 构造方法没有返回值，调用时必须使用`new`操作符。

### 默认构造方法

如果一个类没有定义构造方法，编译器会自动为我们生成一个默认构造方法，它没有参数，也没有执行语句，类似这样：

```java
class Person {
    public Person() {
    }
}
```

如果我们自定义了一个构造方法，那么，编译器就**不再**自动创建默认构造方法。

如果既要能使用带参数的构造方法，又想保留不带参数的构造方法，那么只能把两个构造方法都定义出来：

```java
public class Main {
    public static void main(String[] args) {
        Person p1 = new Person("Xiao Ming", 15); // 既可以调用带参数的构造方法
        Person p2 = new Person(); // 也可以调用无参数构造方法
    }
}

class Person {
    private String name;
    private int age;

    public Person() {
    }

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }
    
    public String getName() {
        return this.name;
    }

    public int getAge() {
        return this.age;
    }
}

```

没有在构造方法中初始化字段时，引用类型的字段默认是`null`，数值类型的字段用默认值，`int`类型默认值是`0`，布尔类型默认值是`false`：



在Java中，创建对象实例的时候，按照如下顺序进行初始化：

1. 先初始化字段，例如，`int age = 10;`表示字段初始化为`10`，`double salary;`表示字段默认初始化为`0`，`String name;`表示引用类型字段默认初始化为`null`；

2. 执行构造方法的代码进行初始化。

	后运行的构造方法决定初始值。

### 多构造方法

```java
class Person {
    private String name;
    private int age;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public Person(String name) {
        this.name = name;
        this.age = 12;
    }

    public Person() {
    }
}
```

调用时编译器根据参自动区分。

一个构造方法可以调用其他构造方法，调用其他构造方法的语法是`this(…)`：



## 3 方法重载`Overload`

方法名相同，但各自的参数不同。

在`Hello`类中，定义多个`hello()`方法：

```java
class Hello {
    public void hello() {
        System.out.println("Hello, world!");
    }

    public void hello(String name) {
        System.out.println("Hello, " + name + "!");
    }

    public void hello(String name, int age) {
        if (age < 18) {
            System.out.println("Hi, " + name + "!");
        } else {
            System.out.println("Hello, " + name + "!");
        }
    }
}
```

方法重载的返回值类型通常都是相同的。

方法重载的目的是，功能类似的方法使用同一名字，更容易记住，因此，调用起来更简单。

例如，`String`类提供了多个重载方法`indexOf()`，可以查找子串：

- `int indexOf(int ch)`：根据字符的Unicode码查找；
- `int indexOf(String str)`：根据字符串查找；
- `int indexOf(int ch, int fromIndex)`：根据字符查找，但指定起始位置；
- `int indexOf(String str, int fromIndex)`根据字符串查找，但指定起始位置。
