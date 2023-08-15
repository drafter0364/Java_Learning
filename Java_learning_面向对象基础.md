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

:question: 如何让外部代码间接修改`field`

:rotating_light:使用方法（`method`）

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



