### 一. 测试代码：

####1. 两个辅助类 `Apple` 和 `Pen`

```java

class Apple {

    static {
        System.out.println("Apple's static code block1");
    }

    public Apple() {}
    public Apple(String str) {
        System.out.println("Apple construction, " + str);
    }
}

class Pen {
    public Pen() {}
    public Pen(String str) {
        System.out.println("Pen construction, " + str);
    }
}
```

#### 2. 主要测试类 `Father` 和 `Son`

```java
class Father {
    static {
        System.out.println("Father's static code block1");
    }

    static Pen pen = new Pen("Father's static pen");

    {
        System.out.println("Father's construction code block1");
    }

    Apple apple = new Apple("Father's apple");

    public Father() {
        System.out.println("Father construction");
    }

    {
        System.out.println("Father's construction code block2");
    }

    static Apple apple2 = new Apple("Father's static apple2");

    static {
        System.out.println("Father's static code block2");
    }
}

class Son extends Father {

    {
        System.out.println("Son's construction code block1");
    }

    static Pen pen = new Pen("Son's static pen");

    static {
        System.out.println("Son's static code block1");
    }

    Apple apple = new Apple("Son's apple");

    {
        System.out.println("Son's construction code block2");
    }

    public Son() {
        System.out.println("Son construction");
    }

    static {
        System.out.println("Son's static code block2");
    }
}

class DemoTest {
    public static void main(String[] args) {
        Son son = new Son();
        System.out.println("//------------------");
        System.out.println("New son2");
        System.out.println("//------------------");
        Son son2 = new Son();
    }
}
```
### 二. 输出结果
```
Father's static code block1
Pen construction, Father's static pen
Apple's static code block1
Apple construction, Father's static apple2
Father's static code block2
Pen construction, Son's static pen
Son's static code block1
Son's static code block2
Father's construction code block1
Apple construction, Father's apple
Father's construction code block2
Father construction
Son's construction code block1
Apple construction, Son's apple
Son's construction code block2
Pen construction, Son's pen2
Son's construction code block3
Son construction
//------------------
New son2
//------------------
Father's construction code block1
Apple construction, Father's apple
Father's construction code block2
Father construction
Son's construction code block1
Apple construction, Son's apple
Son's construction code block2
Pen construction, Son's pen2
Son's construction code block3
Son construction
```

### 三.分析
#### 1. 父类和子类的静态代码块和静态变量
当我们 new 一个 Son 对象出来的时候，由于 Son 继承自 Father，所以需要先将 Father 进行初始化，由于 Father 类中有静态代码块和静态变量，所以此时开始执行 Father 的静态代码块和静态变量，根据代码的先后顺序，依次打印：
- Father's static code block1
- Pen construction, Father's static pen

由于 Apple 类中也有静态代码块，所以在初始化静态变量 apple2 时，执行 Apple 类的静态代码块和 Apple 类的构造方法，继续打印:
- Apple's static code block1
- Apple construction, Father's static apple2

在 apple2 初始化完成后，执行剩下的静态代码块，输出：
- Father's static code block2

接下来开始执行 Son 类中的静态变量和静态代码块，输出：
- Pen construction, Son's static pen
- Son's static code block1
- Son's static code block2
#### 2. 父类的构造代码块和成员变量
在静态代码块和静态变量执行完毕后，开始进行构造代码块的执行以及成员变量的初始化。根据输出结果，我们可以看出，构造代码块的执行以及成员变量的初始化，代码运行顺序与其书写位置的前后顺序相同。
- Father's construction code block1
- Apple construction, Father's apple
- Father's construction code block2
#### 3. 父类的构造方法
- Father construction
#### 4. 子类的构造代码块和成员变量
- Son's construction code block1
- Apple construction, Son's apple
- Son's construction code block2
- Pen construction, Son's pen2
- Son's construction code block3
#### 5. 子类的构造方法
最后才是子类的构造方法执行。
- Son construction
### 四. 总结
1. 静态代码块和静态变量最先执行，对于有继承关系的类来说，父类的执行完毕后再执行子类的。（例子中用到了 Apple 类，所以夹杂了 Apple 类的静态代码块）
2. 同一个类中，构造代码块和成员变量初始化的执行优先于构造方法，并根据位置的前后依次执行。
3. static 随着类的加载而加载，且仅执行一次。