# C#基础

### 变量

1、无符号
byte ushort uint ulong
有符号
//sbyte
short
int long
浮点数
//float
double decimal
特殊
char bool string




复杂数据类型

枚举：整形常量的集合，可以自定义
数组：任意变量类型顺序存储的数据
结构体：任意变量的数据集合，可以自定义

### 枚举

一个被命名的整形常量的集合,一般用来表示状态，类型等等

作用：可以用来表示玩家当前处于哪种状态，待机，行走，奔跑，攻击等，可以清晰的分清状态含义

PS：要声明在namespace语句块或class语句块中，不能再函数语句块中声明

```c#
//声明枚举
    enum E_xxx  //命名规范
    {
        enum1,  //枚举中的整形常量
        enum2	//第一个默认值是0，当枚举项被赋值过后，后面的会在上一个的基础上依次累加
    }

//声明枚举变量  自定义枚举类型 变量名 = 默认值
	E_xxx xxx = E_xxx.enum1; 

//枚举 与 switch 共同使用，方便制作逻辑
    switch(xxx)
    {
        case E_xxx.enum1:
            //逻辑
            break;
        case E_xxx.enum2:
            //逻辑
            break;
        default:
            //逻辑
            break;
    }

//枚举的类型转换
    //枚举转int
    int i = (int)枚举变量;  
    //枚举转字符串
    string str = 枚举变量.ToString();  
    //字符串转枚举
    str = (强转的目标枚举类型E_xxx)Enum.Parse(typeof(转换的枚举类型E_xxx),"转换的对应枚举项字符串（enum1）");  

```



### 交错数组

交错数组是由多个长度不同的一维数组组成的数组，每个一维数组的长度可以不同

区别：二维数组的每行列数相同，交错数组每行的列数可能不同

```c#
int[][] arr = new int[[1,2,3],[4,5],[6]];  //交错数组
int[][] arr = new int[[1,2,3],[4,5,6]];  //二维数组
```



### 值类型和引用类型

值类型：结构体，其他

引用类型：string，数组，类 ，值类型的集合

```C#
int a = 10;  //声明一个值类型
int[] arr = new int[] {1,2,3};  //声明一个引用类型

int b = a;  //此时a==10,b==10，arr[0]==1,arr2[0]==1
int[] arr2 = arr;

//赋值
b = 20;  //此时a==10,b==20，arr[0]==10,arr2[0]==10
arr[0] = 10;

//值类型 在相互赋值时，会将内容拷贝给对方，它变我不变
//引用类型 相互赋值，是让两者指向同一个值，它变我也变
    
//因为存储方式的不同造成了使用上的区别
    //值类型存储在栈空间，系统自动分配，自动回收，小而快
    //引用类型存储在堆空间，手动申请手动释放，大而慢
    //值类型拷贝会开辟一个新的空间，所以不会影响被拷贝的原内容
	//引用类型是直接拷贝地址，指向的是同一个东西，修改的也就是堆空间里的同一个内容
    
//特殊引用类型string
    //具备值类型的特征 它变我不变
    //string在重新赋值时会在堆里开辟一个新的空间，重新指向新建的那个房间，原本的房间就失去作用变成垃圾，所以反复对string赋值会产生内存垃圾
```



### ref、out 关键字

```C#
//作用：当传入的值参数在函数内部修改时，或者引用类型参数在内部重新声明时，让外部的值也能发生变化
static void Xxx(ref int value,ref int[] arr)
{
    value = 6;
    arr = new int[] {1,2,3}
}
Xxx(ref val,ref arr)

//ref传入的变量必须初始化，out不用   类似买票上车和上车买票
//out传入的变量必须在内部赋值，ref不用

```



### Params：变长参数 

可以传入多个同类型参数

```C#
static int Sum(params int[] arr)
{
    int sum = 0;
    for(int i = 0;i<arr.Length;i++)
        sum += arr[i]
    return sum
}
Sum();
Sum(1,2,3,4,5,6)
//params int[] 意味着可以传入n个int参数，n可以大于0，传入的参数会存在arr数组中，
//params关键字后面必须是数组，数组类型任意
//params可以与其他参数一起使用
//函数参数最多出现一个params，并且必须是在最后一组参数，前面可以有n个参数
```



### 可选参数：参数默认值

作用：可以给参数默认值，使用时可以不传参，不传用默认值，传了就用传的

```C#
static void Xxx(string str = "我爱学习")
{
    ...
}
//支持多参数默认值
//如果存在普通参数，带默认值的参数要写在最后面
```



### 递归

函数自己调用自己

```C#
//必须有结束调用的条件
//用于条件判断的条件必须改变，能够停止递归
static void Func()
{
    Func();
}
```



### 结构体

是值类型

```C#
//结构体是一种自定义变量类型，是数据和函数的集合，在结构体中可以声明各种变量和语法
//直接写在namespace下

struct XXX
{
   	//变量
    int name;  //结构体中声明的变量不能直接初始化，可以写任意类型，包括结构体，但不能是自己的结构体
    //函数  不用加static
    void Func()
    {
        ...
    }
}
```



### Sort 排序

```C#
int[] arr = new int[]{1,5,8,3}
//从小到大，两种写法
arr.Sort();
Array.Sort(arr);

//从大到小
arr.Sort();
arr.Reverse(); //排序后反转
```



### 冒泡排序

```C#
// 外层循环：需要进行n-1次冒泡操作
for (int i = 0; i < len - 1; i++)
{
    // 内层循环：每次比较相邻两个数，如果前一个数比后一个数大，则交换它们的位置
    for (int j = 0; j < len - i - 1; j++) 
    {
        if (nums[j] > nums[j + 1])
        {
            int temp = nums[j];
            nums[j] = nums[j + 1];
            nums[j + 1] = temp;
        }
    }
}
```



### 选择排序

会新建一个中间商，从头遍历到尾，找出极值（最大或最小），和当前一轮的最后一个位置交换

中间商用来记录索引，每一轮开始，默认第一个是极值，然后依次比较，找到最大值的索引，存储在中间商中，然后与当前轮的最后一个位置交换

```C#
for(int j = 0; j < arr.Length; j++)  //比较j轮
{
    int index = 0;  //中间商初始索引
    //开始比较,arr.Length-j:排除上一轮已经放置好的数
    for(int i = 1; i < arr.Length - j; i++)
    {
        if(arr[index] < arr[i])  //找出极值
            index = n
    }
    //当前极值不处于目标位置时进行交换
    if(index != arr.Length - 1 - j)
    {
        int temp = arr[index];
        arr[index] = arr[arr.Length-1-m];
        arr[arr.Length-1-m] = temp;
    }
}

//升序
for (int i = 0; i < lst.Count - 1; i++)
{
    var minIndex = i;
    for (int j = i+1; j < lst.Count; j++)
    {
        if (lst[j] < lst[minIndex])
        {
            minIndex = j;
        }
    }

    int temp = lst[minIndex];
    lst[minIndex] = lst[i];
    lst[i] = temp;
}
```





# C#核心：面向对象



## 1、封装

### 类与对象

```C#
class Person  //类声明，引用类型
{
    Person p1;  //栈上有地址，堆没有分配内存
    Person p2 = null;  //等同于上一句，null的意思是不分配堆内存，而不是将堆内存的东西变为空，只是修改地址，不修改对象
    Person p3 = new Person();  //类对象声明，实例化对象
}
```



### 成员变量和访问修饰符

用来描述对象的特征

```C#
class Person
{
    string name = "oo";  
    Person p;  //声明和自己同类型的成员变量时，不能初始化，负责会造成递归导致内存溢出
}

//访问修饰符
//public 公共的 内外部都能访问使用
//private 私有的 内部才能使用
//protected 保护的 内部和子类可以
//internal 程序集访问，只能在当前程序集中访问

//得到某个类型的默认值
default(int)  //得到int类型的默认值 0
```



### 构造函数

在实例化对象时会调用的用于初始化的函数，如果不写默认存在一个无参构造函数

**构造函数的写法：**

1.没有返回值
2.函数名和类名必须相同
3.没有特殊需求时一般都是public
4.构造函数可以被重载
5.this代表当前调用该函数的对象自己

PS：如果不自己实现无参构造函数而实现了有参构造函数，会失去默认的无参构造

```C#
class Person
{
    public string name;
    public int age;
    
    //类中允许自己声明无参构造函数，会顶替默认的无参构造函数
    public Person(string name,int age)
    {
        ...;
    }
    
    //构造函数特殊写法
    //通过this 重用构造函数代码
    //写法：访问修饰符 函数名(参数):this(参数)
    //:this() 会调用另一个构造函数，最先执行
    public Person(string name,int age):this()
    {
        ...;
    }
    
    //会优先调用带有一个age参数的构造函数后再执行当前函数逻辑
    public Person(string name,int age):this(age){...}
    
    //可以直接写常量，会自动匹配类型一直的构造函数
    public Person(string name,int age):this(18){...}
    
}
```



### 析构函数

当引用类型的堆内存被回收时，会调用该函数

对于需要手动管理内存的语言（比如C++)，需要在析构函数中做一些内存回收处理
但是C#中存在自动垃圾回收机制GC，所以几乎不会使用析构函数，除非你想在某一个对像被垃圾回收时，做一些特殊处理

```C#

public Person(){}

//当引用类型的堆内存垃圾被回收时，会自动调用该函数
~Person()  //无访问修饰符，无参数，无返回值
{
    ...
}

static void Main(string[] args)
{
    Person p = new Person();
    p = null;  //此时p栈与堆的联系断开，原本堆开辟的空间变成垃圾，但依然不会调用析构函数，只有垃圾被回收时才会调用
}
```



### 垃圾回收机制GC

![GC](D:\03-自学\C#\GC.png)

##### 堆（Heap） 栈（stack）

垃圾回收的过程是在遍历堆(Heap)上动态分配的所有对象
通过识别它们是否被引用来确定哪些对像是垃圾，哪些对像仍要被使用
所谓的垃圾就是没有被任何变量，对像引用的内容

注意：
GC只负责堆(Heap)内存的垃圾回收
引用类型都是存在堆(Heap)中的，所以它的分配和释放都通过垃圾回收机制来管理
栈(Stack)上的内存是由系统自动管理的，会自动分配和释放



C#中内存回收机制的大概原理
0代内存
1代内存
2代内存
代的概念：
代是垃圾回收机制使用的一种算法（分代算法）
新分配的对像都会被配置在第0代内存中
每次分配都可能会进行垃圾回收以释放内存（0代内存满时）
在一次内存回收过程开始时，垃圾回收器会认为堆中全是垃圾，
会进行以下两步
1.标记对像从根（静态字段、方法参数）开始检查引用对像，
标记后为可达对像，未标记为不可达对象
不可达对像就认为是垃圾
2.搬迁对象压缩堆
(挂起执行托管代码线程)
释放未标记的对象搬迁可达对象修改引用地址
大对象总被认为是第二代内存
目的是减少性能损耗，提高性能
不会对大对象（83kb以上）进行搬迁压缩

```C#
//手动垃圾回收
GC.Collect();
//不能频繁调用，一般在loading条过渡场景时调用
```



### 成员属性

1.用于保护成员变量
2.为成员属性的获取和赋值添加逻辑处理
3.解决3P的局限性

属性可以让成员变量在外部
只能获取不能修改或者
只能修改不能获取

```C#
class Person
{
    private string name;
    public string Name
    {
        get //不加会使用属性申明时的访问权限
        {
            //返回之前的逻辑
            return name;
        }
        private set  //不能让get和set的访问权限都低于属性的权限
        {
            //设置前的逻辑
            name = value; //value表示外部传入的值
        }
    }
    
    //自动属性
    public float Height
    {
        get;
        private set;  //能得不能改
    }
}

```

1.默认不加会使用属性申明时的访问权限
2.加的访问修饰符要低于属性的访问权限
3.不能让get和set的访问权限都低于属性的权限

#region知识点六自动属性
作用：外部能得不能改的特征
如果类中有一个特征是只希望外部能得不能改的又没什么特殊处理
那么可以直接使用自动属性



### 索引器

**基本概念**
让对象可以像数组一样通过索引访问其中元素，使程序看起来更直观，更容易编写

**总结**
索引器可以让我们以中括号的形式范围自定义类中的元素规则
比较适用于在类中有数组变量时使用，可以方便的访问和进行逻辑处理

```C#
calss Person
{
    pricate string name;
    private Person[] friends;
    
    //声明索引器
    //语法：访问修饰符 返回值 this[参数]{}
    public Person this[int index]
    {
        get
        {
            return friends[index];
        }
        set
        {
            friends[index] = value;
        }
    }
}
```



### 静态成员

static 在程序开始运行时就会分配内存空间，所以可以直接使用，直到程序结束时内存空间才会释放

静态成员与程序同生共死

静态变量/方法：常用的唯一变量，方便别人获取的对象声明

const和static的区别

1、const必须初始化，不能修改，static没有这个规则

2、const只能修饰变量



### 静态类

**特点**
只能包含静态成员
不能被实例化

**作用**
1.将常用的静态成员写在静态类中方便使用
2.静态类不能被实例化，更能体现工具类的唯一性
比如Console就是一个静态类

```C#
static class test
{
    static test()  //第一次使用类中内容时，自动调用一次，用来初始化，只会调用一次
    {
        ...
    }
}
```



### 拓展方法

为现有的非静态变量类型添加新方法

PS：当拓展方法名和原有的某个方法重名，会使用原有的方法方法失效

##### 作用：

提升程序拓展性
不需要再在对象中重新写方法
不需要继承来添加方法
为别人封装的类型写额外的方法

可以为自定义的非静态类添加拓展方法

```C#
//语法：访问修饰符 static 返回值 函数名(this 拓展类型 参数名,参数类型 参数名,...){}
//第一个参数代表拓展的目标，前面必须要加this
public static void Speak(this int value)  //为int拓展了一个成员方法
{
    Console.WriteLine(value);  //value表示使用该方法的实例化对象，因为成员方法需要实例化对象之后才能使用
}

int i = 10;
i.Speak();  //打印出10
```



### 运算符重载 operator

作用：让自定义类和结构体对象可以进行运算

PS：条件运算符需要成对实现，一个符号可以有多个重载，不能使用ref和out

可重载运算符：算数运算符（+ - * / ++ --），逻辑运算符（!），位运算符，条件运算符(> < >= <= == != 需要成对实现，比如有大于就要有小于)



```C#
//语法：public static 返回类型 operator 运算符(参数们)
class Point
{
    public int x;
    public int y;
    
    public static Point operator + (Point p1,Point p2) //最多两个参数
    {
        Point p = new Point();
        p.x = p1.x + p2.x;
        p.y = p1.y + p2.y;
 		return p;
    }
}

Point p1 = new Point();
p1.x = 1;
p1.y = 1;

Point p2 = new Point();
p2.x = 2;
p2.y = 2;

Point p3 = p1 + p2;
```



### 内部类

没啥用

```C#
//内部类：在一个类中再声明一个类，亲密关系的体现
class Person
{
    public Bodu bodu;
    public class Body
    {
        Arm leftArm;
        Arm rightArm;
        class Arm
        {
            
        }
    }
}

Person p = new Person();
//实例化内部类
Person.Body body = new Person.Body();
```



### 分部类 partial

把一个类分成几部分来声明

PS：分部类可以写在多个脚本中，访问修饰符要一致，分布类中不能有重复成员

```C#
partial class Student
{
    public string name;
}

partial class Student
{
    public int age;
}
```



## 2、继承

### 继承的基本规则

一个类A继承类B，类A将会拥有B类的所有特征和行为，并可以拥有自己的特征和行为

特点：子类只能有一个父类，子类可以间接继承父类的父类

```C#
class Teacher { ... }

class ChineseTeacher : Teacher  //继承Teacher类
{
    //继承Teacher中的所有成员方法和成员变量
    //可以自定义子类自己的特征和行为
}

//protected 保护 内部和子类可以访问
```



### 里氏替换原则

任何父类出现的地方，子类都可以替代

父类容器装子类对象

```C#
class GameObj{}
class Player : GameObj {}
class Monster : GameObj {}

//里氏替换 用父类容器装载子类对象
GameObj player = new Player();

//is和as
//is：判断一个对象是否是指定类对象，返回bool
is(player is Player)
{
    ...
}

//as：将一个对象转换为指定类对象，成功返回指定类型对象，失败返回null
Player p = player as Player  //将player转为Player类型
```



### 继承中的构造函数

执行顺序：声明子类对象时，先执行父类构造函数，再执行子类构造函数

子类实例化时，默认会自动调用父类的无参构造函数，如果父类的无参构造函数被顶替，会报错

父类必须有一个无参构造函数

this调用自己的构造函数，base调用父类的

```C#
//通过base调用指定的父类构造函数
public Son : Father
{
    public Son(int i) : base(...)
    {
        ...
    }
}
```



### 万物之父和装箱拆箱

万物之父object是所有类型的基类，是引用类型

```C#
//object转换
//引用类型
object obj = new Son();
o as Son
//值类型
object obj2 = 1;
//强转
float f = (float)obj2
```



### 装箱拆箱

基于里氏替换原则 可以用object容器装载一切类型的变量

##### 发生条件

用object存值类型（装箱）
再把object转为值类型（拆箱）

##### 装箱

把值类型用引用类型存储
栈内存会迁移到堆内存中

##### 拆箱

把引用类型存储的值类型取出来
堆内存会迁移到栈内存中
好处：不确定类型时可以方便参数的存储和传递
坏处：存在内存迁移，增加性能消耗

```C#
//装箱
object obj = 3;
//拆箱
int i = (int)obj
```



### 密封类 sealed

作用：让类无法再被继承

```C#
sealed class Father  //Father类不能再被继承，断子绝孙
{
    ...
}
```



## 3、多态

### 多态基本概念

多态按字面的意思就是“多种状态”，让继承同一父类的子类们，在执行相同方法时有不同的表现，状态
主要目的
同一父类的对象
执行相同行为（方法）有不同的表现
解决的问题：让同一个对象有唯一行为的特征

```C#
//虚函数：可以被子类重写
class GameObject
{
    public virtual void Attack()
    {
        ...
    }
}

//overvide重写虚函数
class Player : GameObject
{
    public override void Attack()
    {
        base.Attack(); //通过base保留父类的行为
    }
}
```



### 抽象类和抽象函数 abstract

特点：
1.不能被实例化的类
2.可以包含抽象方法
3.继承抽象类必须重写其抽象方法

一类对象的统称，现实不存在的东西

```C#
abstract class Thing
{
    
}
```



##### 抽象方法

抽象函数，又叫纯虚方法

##### 特点：

1.只能在抽象类中申明
2.没有方法体
3.不能是私有的
4.继承后必须实现用override重写

```C#
abstract class Thing
{
    public abstract void Water(); //没有方法体
}
```

##### 总结

抽象类被abstract修饰的类不能被实例化以包含抽象方法
抽象方法没有方法体的纯虚方法继承后必须去实现的方法

##### 注意：

不希望被实例化的对像，相对比较抽象的类可以使用抽象类
父类中的行为不太需要被实现的，只希望子类去定义具体的规则的可以选择抽象类然后使用其中的抽象方法来定义规则



### 接口 interface

接口是抽象行为的基类

作用：基类无法实现的方法用接口来实现


##### 接口声明的规范

1.不包含成员变量
2只包含方法、属性、索引器、事件
3.成员不能被实现
4.成员可以不用写访问修饰符，不能是私有的
5.接口不能继承类，但是可以继承另一个接口

##### 接口的使用规范

1.类可以继承多个接口
2.类继承接口后，必须实现接口中所有成员
特点：
1.它和类的申明类似
2.接口是用来继承的
3.接口不能被实例化，但是可以作为容器存储对象

```C#
interface I_Fly()
{
    
}

//接口继承接口 不需要实现
//等类继承接口后，类中实现所有内容
interface I_Walk(){}
interface I_Move : I_Fly,I_Wakj {}

//显式实现接口
//当一个类继承两个接口
//但是接口中存在着同名方法时
//注意：显示实现接口时不能写访问修饰符
class Player : I_Attack,I_SuperAttack
{
    void I_Attack.Attack(){}
    void I_SuperAttack.Attack(){}
}
```



##### 总结：

继承类：是对象间的继承，包括特征行为等等
继承接口：是行为间的继承，继承接口的行为规范，按照规范去实现内容
由于接口也是遵循里氏替换原则，所以可以用接口容器装对象
那么久可以实现装载各种毫无关系但是却有相同行为的对像

##### 注意：

1.接口值包含成员方法、属性、索引器、事件，并且都不实现，都没有访问修饰符
2.可以继承多个接口，但是只能继承一个类
3.接口可以继承接口，相当于在进行行为合并，待子类继承时再去实现具体的行为
4.接口可以被显示实现主要用于实现不同接口中的同名函数的不同表现
5.实现的接口方法可以加virtua1关键字之后子类再重写



### 密封方法

基本概念：用密封关键字sealed修饰的重写函数
作用：让虚方法或者抽象方法之后不能再被重写
特点：和override-一起出现



### 命名空间

概念：命名空间是用来组织和重用代码的
作用：就像是一个工具包，类就像是一件一件的工具，都是申明在命名空间中的

不同命名空间中相互使用，需要引用命名空间，或者指明出处

不同命名空间可以有重名类，如果要使用不同命名空间的重名类，则必须指明出处

命名空间可以包裹命名空间

命名空间的类默认未internal 内部的 只能在当前程序集使用

日#region知识点六关于修饰类的访问修饰符
日/pub1ic
一命名空间中的类默认为oublic
/internal一只能在该程序集中使用
//abstract一
抽象类
//sealed
密封类
//partial
一分部类

```C#
//不同命名空间中相互使用，需要引用命名空间
using test1;

namespace test1
{
    class Player{}
}

namespace test2
{
    test1.Player p = new test1.Player(); //或者指明出处
}

```



### 万物之父的方法

静态方法Equals判断两个对象是否相等
最终的判断权，交给左侧对像的Equals方法（就是虚方法的equals方法
不管值类型引用类型都会按照左侧对象Equals方法的规则来进行比较
静态方法ReferenceEquals
比较两个对象是否是相同的引用，主要是用来比较引用类型的对像象。
值类型对像象返回值始终是fa1se。

```C#
//值类型
object.Equals(1,1);  //true
//引用类型 判断是否指向一个房间
Test t1 = new Test();
Test t2 = new Test();
object.Equals(t1,t2);  //false
t2 = t1;
object.Equals(t1,t2);  //true
```

静态方法ReferenceEquals
比较两个对象是否是相同的引用，主要是用来比较引用类型的对象。
值类型对象返回值始终是fa1se。

```C#
Object.ReferenceEquals();//效果和上面差不多
//可以省略Object，因为obj是所有类型的基类
Equals用来比值，ReferenceEquals用来比引用
```



知识点二object中的成员方法
/普通方法GetType
/该方法在反射相关知识点中是非常重要的方法，之后我们会具体的讲解这里返回的Tye类型
/该方法的主要作用就是获取对象运行时的类型Type,
/通过Type结合反射相关知识点可以做很多关于对象的操作。
Test t new Test();
Type type t.GetType();
//普通方法Memberwiseclone
//该方法用于获取对象的浅拷贝对象，口语化的意思就是会返回一个新的对象，1
//但是新对象中的引用变量会和老对象中一致。 



#### #region知识点三object中的虚方法

//虚方法Equa1s
/默认实现还是比较两者是否为同一个引l用，即相当于ReferenceEquals。
C#
/但是微软在所有值类型的基类System..Va1 ueTypet中重写了该方法，用来比较值相等。
/我们也可以重写该方法，定义自己的比较相等的规侧
文件(E
//虚方法GetHashCode
//该方法是获取对象的哈希码
//(一种通过算法算出的，表示对象的唯一编码，不同对像哈希码有可能一样，具体值根据哈希算法决定)
/我们可以通过重写该函数来自己定义对像的哈希码算
正常情况下，我们使用的极少，基本不用。
//虚方法Tostring
//该方法用于返回当前对像代表的字符串，我们可以重写它定义我们自己的对像转字符串规则
/该方法非常常用。当我们调用打印方法时，默认使用的就是对象的Tostring方法后打印出来的内容。



### String

字符串本质是char数组

```C#
//字符串指定位置获取
string str = "hahaha";
Console.WriteLine(str[0]);  //打印 h
//转为char数组
char[] charArr = str.ToCharArray();
//字符串拼接
str = string.Format("{0}{1}",1,2); //打印 12
//正向查找字符位置
str = "一夜暴富";
str.IndexOf("夜"); //返回下标 1，没找到返回-1
//反向查找，但是打印的下标是从前往后数
str = "一夜暴富暴富";
str.LastIndexOf("暴富"); //打印 4
//移除指定位置后的字符
str = "一夜暴富";
str = str.Remove(2);  //返回一个新字符串，想要改变原来的字符，需要进行赋值
Console.WriteLine("一夜富");
//两个参数时
str = str.Remove(1,1);  //从下标1开始，删除一个字符

//替换字符串
str = "一夜暴富";
str = str.Replace("富","穷");
Console.WriteLine(str); //打印 "一夜穷"

//大小写转换
str = "abc";
str = str.ToUpper(); //转大写
str = str.ToLower(); //转小写

//字符串截取
//截取从指定位置开始之后的字符串
str = "一夜暴富";
str = str.Substring(1); //打印 暴富
str = str.Substring(2,1); //打印富 不会自动判断是否越界

//字符串切割
str = "1,2,3"
string[] strArr = str.split(","); //返回数组
//循环打印显示 123
```



### StringBuilder

可以优化内存

#region知识回顾
/string是特殊的引用
/每次重新赋值或者拼接时会分配新的内存空间
/如果一个字符串经常改变会非常浪费空间#region知识点StringBuilder
/C#提供的一个用于处理字符串的公共类
//主要解决的问题是：
//修改字符串而不创建新的对象，需要频繁修改和拼接的字符串可以使用它，可以提升性能
/使用前需要引用命名空间

有点像动态数组？

```C#
using System.Text;
StringBuilder str = new StringBuilder("啊哈哈");  //初始容量存在冗余，会留有一段空的空间，用空间换取性能
//StringBui1der存在一个容量的问题，每次往里面增加时会自动扩容
//填入的内容超出容量时，会开辟一个新房间，但产生的垃圾比string小，且新房间容量会扩容两倍

//获得容量
str.Capacity;
//获取长度
str.Length;

//增加
str.Append("哈哈哈");
str.AppendFormat("嘎嘎嘎");
//插入
str.Insert(插入下标;插入字符串);
//删除
str.Remove(开始下标,删除个数);
//清空
str.Clear();
//查询
str[0];
//修改
str[0] = "哦哦哦"
//替换
str.Replace("哦哦哦","啊啊啊");
	//不需要赋值，会直接修改原内容，因为没有扩容，不会产生新字符串，不会产生垃圾
```



### 结构体和类的区别

。。。















# C#进阶

## 简单数据结构类

### ArrayList    动态数组

本质是object类型数组

```C#
//引入命名空间
    using System.Collections;

//创建一个arraylist
    ArrayList arr = new ArrayList();

//增
    arr.Add(6);  //单个增加
    arr.AddRange(arr2)  //范围增加
   	arr.Indert(插入位置,666) //中间插入666
//删
    //删除指定元素
    arr.remove(1);  //从头到尾遍历，找到1删除
    //删除指定位置元素
    arr.RemoveAt(2)
//查
        //查看元素是否存在
        arr.Contains("666")  //返回bool值
        //查找元素所在的位置
        int i = arr.IndexOf(66)  //从头找
        int ii = arr.LastIndexOf(66)  //从尾开始找
        
//查看arraylist的长度和大小
    arr.Count;  //长度
    arr.Capacity();  //容量

```





### 装箱拆箱

使用对象来存储数据，就存在装箱拆箱

装箱  将栈上的内存转移到堆上，往其中进行值类型存储时就是在装箱


拆箱  当将值类型对象取出来转换使用时


### stack栈

Stack(栈)是一个c#为我们封装好的类
它的本质也是object[]数组，只是封装了特殊的存储规则
Stack是栈存储容器，栈是一种先进后出的数据结构
先存入的数据后获取，后存入的数据先获取
栈是先进后出

```C#
//引入命名空间
using System.Collections
//创建
Stack stack = new Stack();
//压栈 增 只能一个个放
stack.Push()
//取 弹栈 不存在删除概念
stack.Pop()
//查 
stack.Peek()  //只能查看栈顶的内容
stack.Contains(xxx)   //查看元素是否存在栈中
    
//foreach遍历 顺序是从栈顶到栈底
foreach(object item in stack){ ... }
object[] arr = stack.ToArray()  //转数组后再 遍历
    
//循环弹栈
while(stack.Count > 0)
{
    stack.Pop()
}
```



### queue 队列

栈类似于弹夹，后塞入的子弹先打出，队列类似于一个双向管道，有进出口
Queue是一个C#为我们封装好的类
它的本质也是object[]数组，只是封装了特殊的存储规则
Queue是队列存储容器
队列是一种先进先出的数据结构
先存入的数据先获取，后存入的数据后获取
先进先出

```C#
//引入命名空间
using System.Collections
//创建实例
Queue queue = new Queue();
//增
queue.Enqueue();
//取  先取先加入的对象
queue.Dequeue();
//查 查看队列头部元素但不会移除
queue.Peek()  //方法和栈一样
queue.Contains(xxx)  //查看是否存在元素xxx
    
//改
queue.Clear()  //不能改只能清空队列
//遍历和栈一样
//循环出列，和栈一样
```



### HashTable 哈希表 散列表

Hashtable(又称散列表)是基于键的哈希代码组织起来的键值对
它的主要作用是提高数据查询的效率
使用键来访问集合中的元素

```C#
//声明
Hashtable hashtable = new Hashtable()
    
//增
    hashtable.Add(key,value)  //不能出现相同的键，值无所谓
    
//删 只能通过键取删除
    hashtable.Remove(key)
    
    hashtable.Clear()  //清空
    
//查
    hashtable[key]  //找不到返回null
    //通过键查找
	hashtable.Contains(key)
    hashtable.ContainsKey(key)
    //通过值查找
    hashtable.ConatinsValue(value)
    
//改 只能改值，不能改键
    hashtable[key] = xxx
    
//遍历
    //遍历所有键hashtable.Keys  遍历所有值hashtable.Values
    foreach(object item in hashtable.Keys)
    {
        //item  键  hashtable[item]  值
    }

//键值对一起遍历
	foreach(DictionaryEntey item in hashtable)
    {
		item.Key
        item.Value
    }

//迭代器遍历法
	IDictionaryEnumerator enumerator = hashtable.GetEnumerator()
    bool falg = enumerator.MoveNext()
    while(falg)
    {
        enumerator.Key  //键
        enumerator.Value  //值
        flag = enumrator.MoveNext()  //游标下移
    }
```



## 泛型

### 泛型

泛型相当于类型占位符，实现了类型参数化，达到代码重用目的
通过类型参数化来实现同一份代码上操作多种类型
定义类或方法时使用替代符代表变量类型，当真正使用类或者方法时再具体指定类型

```C#
//泛型类
    class TestClass<T> 
    {
        public T value;
    }

//使用
    TestClass<int> t = new TestClass<int>()  //具体指定类型
    t.value = 10
    TestClass<string>TestClass<string>()
    t.value = '妙阿'

//多个参数
    class TestClass2<T1,T2,T3>  //泛型类
    {
        public T1 value1;
        public T2 value2;
        public T3 value3;
    }

    TestClass2<int,float,TestClass<string>> t2 = new TestClass<int,float,TestClass<string>>()

    
//泛型接口
    interface TestInterface<T>
    {
        T value{get;set;}
    }

//继承
    class Test : TestInterface<int>
    {
        //使用自动补全实现接口
    }


//泛型方法
    public void TestFunc<T>(T value)
    {
        ...
    }

    TestFunc<string>('www')

//得到类型的默认值
    public void TestFunc<T>(T value)
    {
        T t = default(T)
    }

```





### 泛型约束 

泛型实现了数据类型自选，而约束就是为了一定程度上限制这种自选程度

6种 

```C#
//where 泛型字母 : (泛型类型)
//值类型约束 struct
class Test1<T> where T : struct { }
Test1<int> t1 = new Test1<int>();

//引用类型约束 class
class Test2<T> where T : class { }
Test1<object> t2 = new Test1<object>();

//存在无参公共构造约束 new()
class Test3<T> where T : new() { }
Test1<Test1> t3 = new Test1<Test1>(); //test1中必须要有无参构造函数

//某个类本身或其派生类  类名
class Test4<T> where T : Test1 { }
Test1<Test1> t4 = new Test1<Test1>(); //尖括号中只能填test1或者它的子类

//某个接口和其派生类  接口名
interface ITest { }
class Test5 : ITest { }
class Test6<T> where T : ITest { }

Test6<ITest> t6 = new Test6<ITest>(); //两种都行
Test6<Test5> t6 = new Test6<Test5>();

//另一个泛型类型本身或者派生类型 另一个泛型字母
...
    
```



##### 约束的组合使用

可以用逗号隔开写多个约束

```C#
class test7<T,K> where T : class,new()
```



##### 多个泛型字母有约束

```C#
class test7<T,K> where T : struct,where K : class{ }
```



## 常用泛型数据结构类

### List

list是一个可变类型的泛型数组，使用方法和arrayList差不多

##### list和arrayList的区别：

1、arrayList可以存储任何类型的对象，而list只能指定一个类型进行存储，list的安全性更高

2、list性能更好，因为它是泛型数组，会提前分配数组大小，而arrayList是object数组，再添加删除元素时会对数组进行扩容和移动



```C#
using System.Collections.Generic;
//声明
	List<int> list = new List<int>();
//增
	//一个个加
    list.Add(1); 

	//批量加
    List<int> list1 = new List<int>(){1,2,3};
    list.AddRange(list1); 
	
	//指定位置添加
	list.Insert(index,element);

//删
    //移除指定元素
    list.Remove(1);

    //移除指定位置元素
    list.RemoveAt(0);

    //清空
    list.Clear();

//查
    //根据下标查询指定位置的元素
    list[0];

    //查看指定元素是否存在
    list.Contains(1); //查看list中是否存在1

    //正向查找 返回下标，找不到返回-1
    list.IndexOf(1);

    //反向查找
    list.LastIndexOf(1);

//改
	list[0] = 114514;

```





### Dictionary 字典

字典就是拥有泛型的散列表，是基于键的哈希代码组织起来的键值对

键值对类型从散列表的object变成了泛型

```C#
using System.Collections.Generic;
//声明
Dictionary<键类型,值类型> dic = new Dictionary<>(键类型,值类型);

//增 不能出现相同键
dic.Add(键,值);

//删 通过键删除
dic.Remove(键);

//查
dic[键];
dic.ContainsKey(键);
dic.ContainsValue(值);

//遍历键
foreach(键类型 item in dic.Keys)
{
    ...
}

//同时遍历键值对
foreach(KeyValuePair<键类型,值类型> item in dic)
{
    ...
}
```



### 顺序存储

用一组地址连续的存储单元 依次存储 线性表的各个数据元素

数组，stack，queue，list，arrayList都是顺序存储

在同一个地址上连续的存储元素

![uTools_1684894851746](C:\Users\Agares\Downloads\uTools_1684894851746.png)

### 链式存储

优点：每次添加数据时不会像顺序存储一样扩容，不会产生垃圾

单向链表，双向链表，环形链表

用一组任意的存储单元 存储 线性表中的各个数据元素

每一个数据元素可能存在不同位置，通过链接连在一起，没有规则

![uTools_1684894906249](C:\Users\Agares\Downloads\uTools_1684894906249.png)



##### 单项链表实现

```C#
//链表节点
class Link<T>
{
    public T value;
    public Link<T> nextLink;
}

//链表类：管理节点
class LinkList<T>
{
    public Link<T> head;
    public Link<T> last;
    //添加节点
    public void Add(T value)
    {
        Link<T> link = new Link<T>(value);
        if(head == null)
        {
            head = link;
            last = link; //两者指向一个东西
        }
        else
        {
            last.nextLink = link;
            last = link;
        }
    }
    //删除节点
    public void Remove(T value)
    {
        if(head == null) //链表没有内容时
            return;
        if(head.value.Equals(value)) //找到要删除的节点
        {
            head = head.nextLink; //移除头节点
            if(head == null) //移除头节点后如果为空，说明只存在一个节点，所以要清空尾部
            {
                last = null;
            }
            return;
        }
        
        Link<T> linkNode = head;
        while(linkNode.nextLink != null)
        {
            if(linkNode.nextLink.value.Equals(value))
            {
                //找到元素后让该元素的上一个节点指向该元素的下一个节点
                linkNode.nextLink = linkNode.nextLink.nextLink;
                break;
            }
            //让删除元素的上一个节点和删除元素的下一个节点连接
            linkNode = linkNode.nextNode;
		}
    }
    
}
```



##### 顺序存储和链式存储的优缺点：

链式存储计算上优于顺序存储，中间增加和删除时不用像顺序存储一样需要移动位置

顺序存储使用上优于链式存储，顺序存储可以直接通过下标得到元素，而链式需要遍历



### LinkedList

LinkedList是一个可变类型的泛型双向链表



```C#
using System.CollectionsGeneric;

//声明
LinkedList<int> linkedList = new LinkedList<int>();

//增
linkedList.AddLast(10); //在链表尾部添加
linkedList.AddFirst(20); //在链表头部添加
//在某个节点之后添加一个节点,就是插入
LinkedListNode<int> node = linkedList.Find(3); //1、找到要插入的节点
linkedList.AddAfter(node,15); //2、在node节点后添加15节点


//删除
linkedList.RemoveFirst(); //删除头节点
linkedList.RemoveLast(); //删除尾节点
linkedList.Remove(20); //删除指定元素，无法通过位置删除
linkedList.Clear(); //清空

//查
LinkedListNode<int> first = linkedList.First; //得到头节点
LinkedListNode<int> last = linkedList.Last; //得到尾节点
LinkedListNode<int> node = linkedList.Find(3); //根据值查找，找不到返回空

//判断是否存在
linkedList.Contains(10); //判断是否存在10

//改 得到节点后通过.Value形式修改
linkedList.First.Value = 886; //修改头节点的值

//遍历
//1、foreach
foreach(int item in linkedList)
{
    Console.WriteLine(item); //得到每一个的值，而不是节点对象
}

//2、从头到尾
LinkedListNode<int> head = linkedList.First;
while(head != null)
{
    Console.WriteLine(head.Value);
    head = head.Next;
}

//3、从尾到头
LinkedListNode<int> node = linkedList.Last;
while(node != null)
{
    Console.WriteLine(node.Value);
    node = node.Previous; //让当前节点等于上一个节点
}
```



### 泛型栈和队列

```C#
Stack<int> stack = new Stack<int>();
Queue<object> queue = new Queue<object>();
```



### 委托 delegate

委托用来表示函数的变量类型，是用来存储，传递函数的容器，不同的函数必须对应与各自格式（参数/返回值）一致的委托

委托作用：作为类的成员/作为函数的参数

```C#
//书写位置：namespace
//定义自定义委托，访问修饰符默认public
delegate void Func();

//委托变量是函数的容器
static void Test(){}
////装载函数的两种写法
Func f1 = new Func(Test); //Test就是f1，只是转载，不会调用
Func f2 = Test; 
//调用函数的两种写法
f1.Invoke(); 
f2();

//用委托存储多个函数 也叫多播委托
Func f3 = Test; 
f3 += Test;
f3(); //此时会按照添加顺序执行委托中存储的所以方法

//移除指定的函数
Func f4 = Test; 
f4 -= Test;

//当容器为空时，如果声明方式是赋值为null，就可以在空的状态下添加删除，声明时未赋值，就不能直接增删，只能先赋值，最好先判空再调用

//系统自带的委托
//1、Action  无参无返回值委托
Action ac = Test;
//传递n个参数 
static void Test4(int a,string b,bool c){};
Action<int,string,bool> ac = Test4;

//2、Func<类型>  有指定类型返回值的泛型委托
static int Test1() { return 0; }
Func<int> func = Test1;
//Func<> 就相当于👇
delegate T Test2<T,K>(T t,K k);
//传递n个参数
static int Test5(int a){ return a; }
Func<int,int> func = Test5; //Func<参数类型,返回值类型>
//传递多个参数时，尖括号中最后一个是返回值的类型，其余是其他参数的类型
//例如Func<int,string,bool>就是一个有两个参数（int,string）的返回值为bool类型的委托
```



### 事件 event

事件就是受约束的委托

事件是一种特殊的变量类型，是基于委托的存在，它能让委托的使用更具有安全性

用法和委托一样，但是只能作为成员存在于类和接口以及结构体中

事件和委托的区别：事件不能再类外部赋值和调用，但是可以添加删除，要调用只能通过类中封装方法调用，比委托更安全

事件不能作为临时变量在函数中使用，委托可以

```C#
class Test
{
    //委托成员变量 
    public Action action;
    //事件成员变量 都是用来存储方法
    public event Action event;
    
    //使用方法和委托一模一样
    
    //可以使用 += / -= 来添加或者移除，但是不能拆解成 xx = xx + xx 的形式，大概是只重载了 += 和 -= 的运算符
    
}
```



### 匿名函数

匿名函数是用来配合委托和事件使用的，脱离委托和事件，就不会使用匿名函数

##### 匿名函数的使用

```C#
//无参数 无返回值
Action ac = delegate(){ ... }; //声明一个匿名函数并存储在委托ac中，如果只声明不使用就会报错，必须要与委托或事件一起使用

//有参数 有返回值
Func<string> func = delegate(int a)
{
    return "P5R天下第一"; //返回的是啥类型就是啥类型，不用主动声明
}

//匿名函数一般作为函数的参数传递 或者 作为函数的返回值
class Test
{
    public Action ac;
    public void Func1(Action ac){ ... }
    public Action Fun2()
    { 
        //作为函数返回值传递时
        return delegate(){...};
    }
}
//作为函数参数传递时
Test t = new Test();
t.Fun1(delegate(){...})

//作为函数返回值传递时
t.Fun2()(); //直接调用，Func2()表示方法本身，第二个()调用
Action ac = t.Fun2();
ac(); //第二种调用方式

//缺点：因为匿名函数没有名字，所以添加到委托后无法指定移除，只能清空

```



### lambda表达式

就是匿名函数的简写，使用上和匿名函数一样，都是与委托或者事件配合使用

```C#
//语法
(参数列表) => { ... }

//无参数 无返回值
Action ac = () => { ... };
//有参数 有返回值 （参数类型可以省略，类型要和委托/事件一致）
Func<int,string> func = (a) => {return "哈哈";};
```



##### 闭包

内层函数可以引用包含在它外出的函数的变量，即使外层函数已经执行完毕

该变量提供的值并非变量创建时的值，而是父函数范围内的最终值 

匿名函数体中如果使用了外部函数的变量，那么就会把这个变量拷贝到堆上，所以在外部函数执行完毕后还能使用

缺点和匿名函数一样，无法指定移除

```C#
public event Action ac;
public Test()
{
    int i = 10; 
    ac = () => 
    {
        Console.WriteLine(i); 
        //导致临时声明的变量i生命周期被改变，不再被释放，形成闭包
    }
    
    //该变量提供的值并非变量创建时的值，而是父函数范围内的最终值 
	for(int i = 0; i < 10; i++)
    {
        ac += () =>
        {
            Console.WriteLine(i);
            //每次循环都添加一个匿名函数，然后一次性打印
            //所以再执行打印时，循环早已结束，i的最后值是10，，所以会打印10个10 
            //如果想要打印0-9，可以临时声明一个变量存储，每次循环临时变量都不一样
        }
    }

}
```





### List排序

```C#
List<int> list = new List<int>(){1,2,3};
//升序
list.Sort();
//自定义类排序
class Test : IComparable<Test> //继承排序接口
{
    public int money;
    public Test(int money)
    {
        this.money = money;
    }
    //实现接口
    public int CompareTo([AllowNull] Test other) //[AllowNull]：不为空
    {
        //通过返回值排序，小于0放在传入对象的前面，等于0位置不变，大于0放在后面,
        //传入对象的位置就是0，返回负数，就排在它的前面
        if(this.money > other.money) //升序排，降序就反过来
            return 1;
        else return -1;
    }
}
List<Test> testList = new List<Test>();
testList.Add(new Test(20));
testList.Add(new Test(50));
testList.Add(new Test(10));
testList.Sort();

//委托排序：通过一个传递方法的sort重载
class Test
{
    public int id;
    public Test(int id)
    {
        this.id = id;
    }
}

class Program
{
    static void Main(string[] args)
    {
        List<Test> list = new List<Test>();
        list.Add(new Test(1));
        list.Add(new Test(3));
        list.Add(new Test(2));
        list.Sort(DeleSort);

        static int DeleSort(Test t1,Test t2)
        {
            return (t1.id > t2.id) ? 1 : -1;
        }

        for (int i = 0; i < list.Count; i++)
        {
            Console.WriteLine(list[i].id); //输出了 1 2 3
        }
        
        //最简介的写法
        list.Sort((t1,t2)=>{return (t1.id > t2.id) ? 1 : -1});
    }
}
```

系统自带的变量（int,float,double...) 一般可以直接使用sort

自定义类排序有两种方式

1、继承接口 IComparable

2、在sort中传入委托函数



### 协变逆变

就是里氏替换原则在泛型中的体现，用out和in修饰的泛型委托可以相互转载（有父子关系的泛型）

协变 out 子类变父类

逆变 in 父类变子类

```C#
//1、用于泛型接口和泛型委托中修饰泛型字母
//用out修饰的泛型 只能作为返回值
delegate T TestOut<out T>();
//用in修饰的泛型 只能作为参数
delegate void TestIn<in T>(T t);

//2、结合里氏替换原则
delegate T TestOut<out T>();
class Father { }
class Son : Father { }
TestOut<Son> s = () => { }
TestOut<Father> f = s; //加上out后会自动根据里氏替换原则判断返回值是否存在父子关系
```



### 多线程

##### 进程 process

![uTools_1685003453806](C:\Users\Agares\Downloads\uTools_1685003453806.png)



操作系统提供运行环境，每个运行的应用程序都是一个进程

进程是系统进行资源分配和调度的基本单位，是操作系统结构的基础

也是计算机中程序关于某数据集合的一次运行获得

##### 线程

是操作系统能够进行运算调度的最小单位

线程包含在进程之中，是进程中的实际运作单位

一条线程指的是进程中一个单一顺序的控制流，一个进程中可以并发多个线程

进程运行时就是通过执行线程的代码

线程就是代码从上到下运行的一条“管道”

![uTools_1685004096907](C:\Users\Agares\Downloads\uTools_1685004096907.png)

##### 多线程



![uTools_1685004478267](C:\Users\Agares\Downloads\uTools_1685004478267.png)

同时运行代码的多条“管道”，就叫多线程

可以同一时间分批处理不同的逻辑

用来处理一些比较复杂，可能会影响主线程流畅度的逻辑

##### 线程类 Thread

```C#
//声明一个线程（线程中的代码，需要用一个函数封装）
Thread t = new Thread(ThreadFunc);
static void ThreadFunc(){ ... }
    
//开启线程
t.Start();

//设置为后台线程
//比如有一个需要死循环的线程，就可以设置会后台线程
//前台线程结束后，即使后台进程还在运行，程序也会结束
//后台线程不会影响主线程的结束
//如果不设置为后台线程，可能导致进程无法正常关闭
t.isBackground = true;

//关闭释放一个线程
//如果开启的线程不是死循环，可以不用刻意去关闭它
//1、置空线程
t = null; 
//2、bool标识，通过bool变量控制循环
//3、中止线程 （直接在控制台执行会报错）
t.Abort();
```



##### 线程休眠

```C#
//在哪个线程执行，就休眠哪个线程
Thread.Sleep(毫秒数); //让线程休眠xx毫秒，xx秒后线程继续执行
```



##### 线程之间共享数据

多个内存使用的内存是共享的，都属于进程

所以当多线程同时操作同一片内存区域时就可能会出现问题，可以通过加锁避免

缺点：会影响线程的效率

```C#
//lock 当在多线程当中，对同一个东西进行逻辑处理时
//为了避免执行顺序混乱
lock(引用类型对象) //没有锁时才会执行里面的代码
{
    //执行里面的逻辑时会锁住lock括号内的对象
    //全部执行完成后会解锁
}
```



### 预处理器指令

预处理器指令可以知道编译器在编译开始之前对信息进行预处理

预处理器指令都是以开始

预处理器指令不是语句，所以它们不以分号；结束



##### 常见的预处理器指令

```C#
//1、折叠代码块
#region
    ...
#endregion
    
//2、定义一个符号，写在整个脚本的最前面，一般配合if使用
#define PC
#undef PC  //取消定义一个符号
    
#if PC  //如果有PC这个符号，那么其中包含的代码就会被编译
    Console.WriteLine("C#你让我疯狂")
    
#elif //可选
#else //可选
#endif //与if配对
//作用：可以用来判断是哪个平台和操作系统以及unity版本，分别处理不同的逻辑
    
//3、提示警告/错误，配合if使用
#if
	#waring 警告：不支持当前版本
    #error 报错啦
#endif
```



### 反射和特性

##### 程序集

程序集是由编译器编译后 供进一步编译执行的中间产物

一般表现为.dll(库文件)或者.exe(可执行文件)的格式

程序集就是将所有代码打包起来的集合

一个项目写的所有代码，最终都会被编译器翻译为一个程序集供别人使用



##### 元数据

元数据就是用来描述数据的数据

比如程序中的类，函数，变量等信息就是程序的元数据



##### 反射

程序正在运行时，可以查看其它程序集或者自身的元数据
个运行的程序查看本身或者其它程序的元数据的行为就叫做反射
说人话：
在程序运行时，通过反射可以得到其它程序集或者自己程序集代码的各种信息
类，函数，变量，对象等等，实例化它们，执行它们，操作它们

##### 反射的作用

因为反射可以在程序编译后获得信息，所以它提高了程序的拓展性和灵活性
1.程序运行时得到所有元数据，包括元数据的特性
2.程序运行时，实例化对象，操作对象
3.程序运行时创建新对像，用这些对象执行任务.



```C#
class Test
{
    public a = 0;
    public Test() { ... }
}

//Type：类的信息类，是访问元数据的主要方式
//1、object.GetType() 方法可以获取对象的Type
int a = 10;
Type type1 = a.GetType();

//2、typeof(类名) 一般在同一个程序集中使用
Type type2 = typeof(int);

//3、通过类的名字来获取类型，类名必须包含命名空间
Type type3 = Type.GetType("System.Int32");
//type1、2、3 指向堆中的同一块内存

//程序集类 Assembly
type1.Assembly //得到类的程序集信息
    
//获取类中的所有公共成员
Type t = typeof(Test);
MemberInfo[] infos = t.GetMembers();

//获取所有构造函数
ConstructorInfo[] ctors = t.GetConstructors();

//获取其中的一个构造函数并执行
//获取无参构造 
//获取构造函数传入type数组，数组内容按照顺序是参数类型
//执行构造函数传入object数组 标识按顺序传入的参数
ConstructorInfo info = t.GetConstructor(new Type[0]); //长度为0，没有参数
Test t = info.Invoke(null) as Test; //执行，无参传null即可

//获取有参构造
ConstructorInfo info = t.GetConstructor(new Type[]{ typeof(int)}) //决定类型
Test t = info.Invoke(new object[] { 10 }) as Test; //传对应的参数

//获取类的公共成员变量
//1、获取所有成员变量
FieldInfo[] f = t.GetFields();

//2、获取指定名称的公共成员变量
FieldInfo infoName = t.GetField("name");

//3、通过反射获取/设置对象的值
Test test = new Test(infoName.GetValue(test)); //获取test的infoName的值
infoName.SetValue(test,"666")
    
//获取类的公共成员方法
Type type = typeof(string);
MethodInfo[] ms = type.GetMethods("Substring",new Type[] {typeof(int),typeof(string)});
type.Invoke(要哪个对象执行,new object[]{111,"xxx"})
//注意：如果是静态方法Invoke中的第一个参数传null即可
```



####  Axtivator 

用于反射快速实例化对象

```C#
Type type = typeof(type);  //得到type
//默认调用无参构造
Activator.CreateInstance(type) as Test;  //Test是上面的类
//有参构造
Activator.CreateInstance(type,实参) as Test; //会自动判断参数类型

```



#### Assembly

程序集类，用来加载其他程序集

想要使用非己程序集中的内容，需要先加载程序集，加载后，才能用type来使用其他程序集中的内容

```C#
//加载同一文件下的其他程序集
Assembly ass1 = Assembly.Load("程序集名称");
//加载一个指定程序集
Assembly ass2 = Assembly.LoadFrom(@"路径"); //@可以取消转义字符

//获取程序集里元数据
Type[] types = ass2.GetTypes();
```



#### 特性

还没看



### 迭代器

迭代器提供一个方法顺序访问一个聚合对象中的各个元素，并不暴露其内部的标识

可以用foreach遍历的类，都实现了迭代器

```C#
class TestList : IEnumerable,IEnumertor / /继承接口
{
    private int[] arr;
    public Func()
    {
        arr = new int[]{1,2,3};
    }
    
    //接口实现GetEnumerator方法，想要通过foreach遍历，就必须实现该方法
}

TestList tList = new TestList();

//foreach的本质
//1、先获取in后面的对象的 IEnumertor，通过调用里面的GetEnumerator方法来获取
//2、执行得到的IEnumerator对象中的 MoveNext 方法（移动光标）
//3、如果MoveNext返回true，就会去得到Current（当前的元素）
//4、然后赋值给in前面的形参item
class Test : IEnumerable, IEnumerator
{
    private int[] arr;
    public Test()
    {
        arr = new int[] { 1, 2, 3, 4, 5, 6 };
    }
    public int cursor = -1;
    public object Current
    {
        get
        {
            return arr[cursor];
        }
    }

    public IEnumerator GetEnumerator()
    {
        Reset(); 
        return this;
    }

    public bool MoveNext()
    {
        cursor++;
        return cursor < arr.Length;
    }

    public void Reset()
    {
        cursor = -1;
    }
}

class Program
{
    static void Main(string[] args)
    {
        Test t = new Test();
        foreach(int item in t)
        {
            Console.WriteLine(item);
        }
    }
}




//语法糖实现迭代器
class Test : Ienumerable
{
    private int[] arr;
    public Test()
    {
        arr = new int[]{1,2,3}
    }
    
    public Ienumerator GetEnumerator()
    {
        for(int i = 0; i < arr.Length; i++)
        {
            //yield 配合迭代器使用，协程
            yield return arr[i];
        }
    }
}
```

