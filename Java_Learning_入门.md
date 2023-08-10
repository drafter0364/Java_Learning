# Java Leaning - 入门

`Java是一种面向对象的编程语言。面向对象编程，英文是Object-Oriented Programming，简称OOP。`

start: 2023.7.25

## 1 程序基础

### 1.1  浮点数运算

#### 1.1.1 浮点数的运算误差

```java
public class Main {
    public static void main(String[] args) {
        double x = 1.0 / 10;
        double y = 1 - 9.0 / 10;
        // 观察x和y是否相等:
        System.out.println(x);	//输出0.1
        System.out.println(y);	//输出0.09999999999999998
    }
}
```

#### 1.1.2 四舍五入

要进行四舍五入，可以对浮点数加上0.5再强制转型

```java
public class Main {
    public static void main(String[] args) {
        double d = 2.6;
        int n = (int) (d + 0.5);
        System.out.println(n);	//输出3
    }
}
```

#### 1.1.3 求平方根

求平方根可用 Math.sqrt().

##### 练习1

```java
// 一元二次方程
public class Main {
    public static void main(String[] args) {
        double a = 1.0;
        double b = 3.0;
        double c = -4.0;
        // 求平方根可用 Math.sqrt():
        // System.out.println(Math.sqrt(2)); ==> 1.414
        // TODO:
        double r1 = 0;
        double r2 = 0;
		r1 = (-b + Math.sqrt(b * b - 4 * a * c))/(2 * a);
		r2 = (-b - Math.sqrt(b * b - 4 * a * c))/(2 * a);
        System.out.println(r1);
        System.out.println(r2);
        System.out.println(r1 == 1 && r2 == -4 ? "测试通过" : "测试失败");
    }
}
```

### 1.2 布尔运算

#### 1.2.1 短路运算

布尔运算的一个重要特点是短路运算。如果一个布尔运算的表达式能提前确定结果，则后续的计算不再执行，直接返回结果。

##### 练习2

判断指定年龄是否是小学生（6～12岁）：

```java
// 布尔运算
public class Main {
    public static void main(String[] args) {
      int age = 7;
        // primary student的定义: 6~12岁
        boolean isPrimaryStudent = age >= 6 && age <= 12 ? true : false;
        System.out.println(isPrimaryStudent ? "Yes" : "No");
   }
}
```

### 1.3 字符串

#### 1.3.1 字符串连接

Java的编译器对字符串做了特殊照顾，可以使用`+`连接任意字符串和其他数据类型，这样极大地方便了字符串的处理。

如果用`+`连接字符串和其他数据类型，会将其他数据类型先自动转型为字符串，再连接：

```java
public class Main {
    public static void main(String[] args) {
        int age = 25;
        String s = "age is " + age;
        System.out.println(s);	//输出 age is 25
    }
}
```

从Java 13开始，字符串可以用`"""..."""`表示多行字符串（Text Blocks).

关于其中的空格：总是以最短的行首空格为基准。

##### 练习3

```java
public class Main {
    public static void main(String[] args) {
        int a = 72;
        int b = 105;
        int c = 65281;
        // FIXME:
        String s = "" + (char)a + (char)b + (char)c;	//先有串，才可以用+拼接
        System.out.println(s);	//输出 Hi!
    }
}
```

### 1.4 数组

定义一个数组类型的变量，使用数组类型“类型[]”，例如，`int[]`。和单个基本类型变量不同，数组变量初始化必须使用`new int[5]`表示创建一个可容纳5个`int`元素的数组。

可以用`数组变量.length`获取数组大小：

```java
public class Main {
    public static void main(String[] args) {
        // 5位同学的成绩:
        int[] ns = new int[5];
        System.out.println(ns.length); //输出 5
    }
}
```

注意数组是引用类型，并且数组大小不可变。

## 2 流程控制

### 2.1 输入输出

#### 2.1.1 输出

`println`是print line的缩写，表示输出并换行。

如果输出后不想换行，可以用`print()`。

格式化输出使用`System.out.printf()`。

#### 2.1.2 输入

```java
import java.util.Scanner;
```

`System.out`代表标准输出流，而`System.in`代表标准输入流。

##### Scanner对象

有了`Scanner`对象后，

要读取用户输入的字符串，使用`scanner.nextLine()`，

要读取用户输入的整数，使用`scanner.nextInt()`。

`Scanner`会自动转换数据类型，因此不必手动转换。

```java
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in); // 创建Scanner对象
        System.out.print("Input your name: "); // 打印提示
        String name = scanner.nextLine(); // 读取一行输入并获取字符串
        System.out.print("Input your age: "); // 打印提示
        int age = scanner.nextInt(); // 读取一行输入并获取整数
        System.out.printf("Hi, %s, you are %d\n", name, age); // 格式化输出
    }
}
```

### 2.2 if判断

要判断引用类型的变量内容是否相等，必须使用`equals()`方法：

```java
		String s1 = "hello";
        String s2 = "HELLO".toLowerCase();
		if (s1.equals(s2)) {
            System.out.println("s1 equals s2");
        } else {
            System.out.println("s1 not equals s2");
        }
```

### 2.3 switch多重选择

**类似C.**

#### 2.3.1 穿透效应

IDE的编译检查功能可以检查是否漏写`break`语句和`default`语句.

从Java 12开始，`switch`语句升级为更简洁的表达式语法，使用类似模式匹配（Pattern Matching）的方法，保证只有一种路径会被执行，并且不需要`break`语句：

```java
 String fruit = "apple";
        switch (fruit) {
        case "apple" -> System.out.println("Selected apple");
        case "pear" -> System.out.println("Selected pear");
        case "mango" -> {
            System.out.println("Selected mango");
            System.out.println("Good choice!");
        }
        default -> System.out.println("No fruit selected");
        }
    }
}

```

**!!**不要写`break`语句，因为新语法只会执行匹配的语句，没有穿透效应。

#### 2.3.2利用switch语句给变量赋值

```java
public class Main {
    public static void main(String[] args) {
        String fruit = "apple";
        int opt = switch (fruit) {
            case "apple" -> 1;
            case "pear", "mango" -> 2;
            default -> 0;
        }; // 注意每一个赋值语句要以;结束
        System.out.println("opt = " + opt);
    }
}
```

`yield`返回一个值作为`switch`语句的返回值 (return)：

```java
default -> {
                int code = fruit.hashCode();
                yield code; // switch语句返回值
            }
```

### 2.4 循环

#### 2.4.1 while 循环

#### 2.4.2 do while 循环

#### 2.4.3 for 循环

#### 2.4.4 for each 循环

`for each`循环能够遍历所有“可迭代”的数据类型，如数组、List、Map等。

##### 练习4

圆周率π可以使用公式计算：
$$
pi/4 = 1-1/3+1/5-1/7+1/9-...
$$


```java
public class Main {
    public static void main(String[] args) {
        double pi = 0;
		int sign =1;
		double n = 1;
        for (int i=0; i<1000;i++) {
            pi+=(sign/n);
			n+=2;
			sign *= -1;// TODO
        }
		pi*=4;
        System.out.println(pi);	//输出 3.140592653839794

    }
}
```

#### 2.4.5 break和continue

`break`语句可以跳出当前循环，`continue`语句可以提前结束本次循环.

## 3 数组操作

### 3.1 数组排序

```java
// 降序排序
import java.util.Arrays;

public class Main {
    public static void main(String[] args) {
        int[] ns = { 28, 12, 89, 73, 65, 18, 96, 50, 8, 36 };
        // 排序前:
        System.out.println(Arrays.toString(ns));
        // TODO:
 		for (int i = 0; i < ns.length - 1; i++) {
            for (int j = 0; j < ns.length - i - 1; j++) {
                if (ns[j] < ns[j+1]) {
                    // 交换ns[j]和ns[j+1]:
                    int tmp = ns[j];
                    ns[j] = ns[j+1];
                    ns[j+1] = tmp;
                }
            }
        }
        // 排序后:
        System.out.println(Arrays.toString(ns));
        if (Arrays.toString(ns).equals("[96, 89, 73, 65, 50, 36, 28, 18, 12, 8]")) {
            System.out.println("测试成功");
        } else {
            System.out.println("测试失败");
        }
    }
}
```

`Arrays.toString(ns)` 是 Java 中的一个方法，它将数组转换为字符串表示形式。该方法会将数组元素用逗号分隔，并包含一个方括号作为起始和结束标记。因此，`Arrays.toString(ns)` 的结果类似于 `"[元素1, 元素2, 元素3, ...]"`。

`equals("[96, 89, 73, 65, 50, 36, 28, 18, 12, 8]")` 是字符串的比较操作，它判断前面生成的字符串是否与 `"[96, 89, 73, 65, 50, 36, 28, 18, 12, 8]"` 相等。

refer : [java.util.Arrays类中equals、toString、fill、sort、binarySearch方法介绍，_海风_C的博客-CSDN博客](https://blog.csdn.net/weixin_44514198/article/details/105344161)

### 3.2 多维数组

#### 3.2.1 二维数组

二维数组的每个数组元素的长度**并不要求相同**，例如，可以这么定义`ns`数组：

```java
int[][] ns = {
    { 1, 2, 3, 4 },
    { 5, 6 },
    { 7, 8, 9 }
};
```

使用Java标准库的`Arrays.deepToString()`输出二维数组：

```java
// 二维数组
import java.util.Arrays;

public class Main {
    public static void main(String[] args) {
        int[][] ns = {
            { 1, 2, 3, 4 },
            { 5, 6, 7, 8 },
            { 9, 10, 11, 12 }
        };
        System.out.println(Arrays.deepToString(ns));
    }
}

```

##### 练习5

使用二维数组可以表示一组学生的各科成绩，请计算所有学生的平均分：

```java
public class Main {
    public static void main(String[] args) {
        // 用二维数组表示的学生成绩:
        int[][] scores = {
                { 82, 90, 91 },
                { 68, 72, 64 },
                { 95, 91, 89 },
                { 67, 52, 60 },
                { 79, 81, 85 },
        };
        // TODO:
        double average = 0;
		double total = 0;
		for(int x = 0; x< scores.length;x++)
			for(int y = 0; y< scores[x].length;y++){
			total += scores[x][y]; 
			}
		average = total/((scores.length)*(scores[0].length));
        System.out.println(average);

        if (Math.abs(average - 77.733333) < 0.000001) {
            System.out.println("测试成功");
        } else {
            System.out.println("测试失败");
        }
    }
}
```

### 3.3 命令行参数

Java程序的入口是`main`方法，而`main`方法可以接受一个命令行参数，它是一个`String[]`数组。



```java
//打印命令行参数
public class Main {
    public static void main(String[] args) {
        for (String arg : args) {
            System.out.println(arg);
        }
    }
}
```
