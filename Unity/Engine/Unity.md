# Unity



### 生命周期

默认修饰符为private和protected，只能在继承了MonoBehavior中使用

![uTools_1686894785264](C:\Users\Agares\Downloads\uTools_1686894785264.png)



Awake：对象出生时马上调用，类似于构造函数

OnEnable：对象激活时调用

Start：对象第一次帧更新时调用

FixedUpdate：用于进行物理更新，每一帧都执行（与游戏帧不同，可以在设置中控制这个帧的间隔时间）

Update：游戏逻辑更新，每一游戏帧更新

LateUpdate：晚于update的帧更新函数，一般用来处理摄像机位置更新处理

OnDisable：对象每次失活时调用，失活时所有挂载的组件都停止执行

OnDestroy：对象被销毁时调用





### MonoBehavior基类

1.创建的脚本默认都继承MonoBehaviour继承了它才能够挂载在
GameObject.上
2.继承了MonoBehavior的脚本不能new只能挂！！！！！！！I
3继承了MonnBehavior的脚本不要去写构造函数，因为我们不会去
new它，写构造函数没有任何意义
4.继承了MonoBehavior的脚本可以在一个对象上挂多个（如果没有加
DisallowMultipleComponent特t性)
5.继承MonoBehavior的类也可以再次被继承，遵循面向对象继承多态
的规则



不继承MonoBehavior

1.不继承Mono的类不能挂载在GameObject.上
2.不继承Mono的类想怎么写怎么写如果要使用需要自己new
3.不继承Mono的类一般是单例模式的类（用于管理模块）或者数据
结构类（用于存储数据）
4.不继承Mono的类不用保留默认出现的几个函数



#### 重要成员

```C#
//当前脚本依附的对象
gameObject
transform.position  //位置
this.transform.position  //角度
gameObject.transform.position  //缩放大小

//获取脚本是否激活
this.enabled = true; //激活，false失活

//得到依附对象上挂载的其他脚本
//根据脚本名获取
Test t = this.GetComponent("脚本名") as Test;

//根据Type获取
t = this.GetComponent(typeof(Test)) as Test;

//根据泛型获取(最好用)
t = this.GetComponent<Test>();

//获取子对象上挂载的脚本,默认也会找自己身上是否挂载,儿子的儿子也会算在其中
t = this.GetComponentInChildren<Test>();
t = this.GetComponentInChildren<Test>(true); //参数加上true后，即使子对象失活也会查找，默认false不查找

//获取多个
Test[] t = this.GetComponentsInChildren<Test>(true);

List<Test> list = new List<Test>();
this.GetComponentsInChildren<Test>(true,list)
    
    
//尝试获取脚本(更加安全的获取单个脚本)
this.TryGetComponent<Test>(out t);
```





### Inspector

```C#
//让脚本中私有和受保护的变量显示在unity中
[SerializeField]  //强制序列化
protected int a;

//不显示在unity窗口中：在变量前加上特性
[HideInInspector]
public bool b;

//让自定义类型可以被访问：加上序列化特性
[System.Serializable]

//一些特性
//分组
[Header("角色")]

//修饰数值的滑条范围
[Range(min,max)]
public float c;

//为变量添加快捷方法
[ContextMenuItem("显示的按钮名","方法名")]
```







### GameObject

##### 成员变量

```C#
//名字
this.gameObject.name
   
//是否激活
this.gameObject.activeSelf
    
//是否是静态
this.gameObject.isStatic
    
//层级(int)
this.gameObject.layer
    
//标签
this.gameObject.tag
```

静态方法

```C#
//创建自带几何体
GameObject.CreatePrimitive(PrimitiveType.Cube);

//查找对象,无法找到失活的对象
//查找单个（如果存在多个满足条件的对象，无法准确确定找到的是谁）
//通过对象名查找
GameObject.Find("对象名");  //效率低下，会遍历场景的所有对象
//通过标签
GameObject.FindWithTage("Tag");


//查找多个对象（只能通过tag）
GameObject.FindGameObjectsWithTag("Tag");

//实例化(克隆)对象：根据一个游戏对象，创建出一个一样的对象
public GameObject obj;  //准备克隆的对象(场景对象/预设体)
GameObject.Instantiate(obj);

//删除对象
GameObject.Destroy(obj);
//延迟删除
GameObject.Destroy(obj,5); //延迟5秒删除

//过场景不移除
GameObject.DontDestroyOnLoad(obj);
```



成员方法

```C#
//创建空物体
GameObject obj = new GameObject();
GameObject obj = new GameObject("超级小自在哈哈哈"); //命名
GameObject obj = new GameObject("name",typeof(Script)); //命名，顺便添加脚本

//为对象添加脚本
Script s = obj.AddComponent<Script>();

//标签比较
this.gameObject.CompareTag("Tag");

```





### Time

主要用于游戏中位移，计时，时间暂停等

```C#
//时间缩放比例
Time.timeScale = 0;  //时间停止
Time.timeScale = 1;  //正常（默认）
Time.timeScale = 2;  //2倍速

//帧间隔时间：最近的一帧，用了多长时间
Time.deltaTime  //受scale（时间比例）影响，游戏暂停时不动
Time.unscaledDeltaTime  //不受scale影响，不受游戏暂停影响

//游戏开始到现在的时间
Time.time
Time.unscaledTime

//物理帧间隔时间 一般写在FixedUpdate
private void FixedUpdate()
{
	print(Time.fixedDeltaTime);
    print(Time.fixedUnscaledDeltaTime);
}
    
//帧数：一次循环就是一帧
Time.frameCount
```



### Transform

##### Vector3：标识三维坐标系的一个向量（结构体）

```C#
Vector3 v3 = new Vector3();
Vector3 v3 = new Vector3(x,y,z); //不传参默认是0

//常用静态
Vector3.zero  //000
Vector3.right  //100
Vector3.left  //-100
Vector3.forward  //001
Vector3.back  //00-1
Vector3.uo  //010
Vector3.down  //0-10
    
//计算两个点之间的距离
Vector3.Distance(v1,v2)
    
```



位置

```C#
//相对世界坐标系
this.transform.position
    
//相对父对象
this.transform.localPosition

//对象当前各朝向（对象自己的坐标轴）
transform.forward
transform.up
......
```

