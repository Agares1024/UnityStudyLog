# Unity框架

### 1、单例模块

最常用，不继承MonoBehaviour

```C#
public class BaseManager<T> where T : new()  
{
    private static T instance;

    public static T GetInstance()
    {
        if (instance == null)
            instance = new T();
        return instance;
    }
}
```



继承了MonoBehaviour，需要在Awake生成

```C#
//继承了monoBehaviour的单例模式对象必须保证唯一性，
//因为单例模式只会指向最后一个挂载的对象
public class SingletonMono<T> : MonoBehaviour where T : MonoBehaviour
{
    private static T instance;
    public static T GetInstance()
    {
        return instance;
    }

    //子类使用awake时，会顶替父类的awake，所以需要使用虚方法
    protected virtual void Awake()
    {
        //继承了monobehaviour的类不能直接new
        //只能通过编辑器内拖动对象，或者addComponent
        //as：将一个对象转换为指定类对象，成功返回指定类型对象，失败返回null
        instance = this as T;  //通过里氏转换原则，用父类装子类

    }
}
```



自动生成

```C#
public class SingletonAutoMono<T> : MonoBehaviour where T : MonoBehaviour
{
    private static T instance;
    public static T GetInstance()
    {
        if(instance == null)
        {
            GameObject obj = new GameObject();
            obj.name = typeof(T).ToString();
            //过场景不删除该对象，因为单例模式往往处于程序的整个生命周期中
            DontDestroyOnLoad(obj); 
            instance = obj.AddComponent<T>();
        }
        return instance;
    }
}
```



### 2、缓存池模块



```C#
public class PoolManager : BaseManager<PoolManager>
{
    public Dictionary<string, List<GameObject>> pool = new Dictionary<string, List<GameObject>>();
    
    //获取缓存池中的对象
    public GameObject GetPool(string name)
    {
        GameObject obj = null;

        if(pool.ContainsKey(name) && pool[name].Count > 0)  //池不为空，取出对象
        {
            obj = pool[name][0];
            pool[name].RemoveAt(0);
        }
        else  //池为空，生成游戏对象
        {
            obj = GameObject.Instantiate(Resources.Load<GameObject>(name));
            obj.name = name;
        }
        obj.SetActive(true);
        return obj;
    }

    //将暂时不用的物体存入缓存池
    public void PushPool(string name, GameObject obj)
    {
        obj.SetActive(false);
        if (pool.ContainsKey(name))  //池不为空
        {
            pool[name].Add(obj);
        }
        else  //池为空
        {
            pool.Add(name, new List<GameObject> { obj });
        }
    }

}
```



使用方法

```C#
public class Test01 : MonoBehaviour
{

    void Update()
    {
        if (Input.GetMouseButtonDown(0))
        {
            PoolManager.GetInstance().GetPool("Prefab/Cube");
        }

        if(Input.GetMouseButtonDown(1))
        {
            PoolManager.GetInstance().GetPool("Prefab/Sphere");
        }
    }
}
```





#### 优化版本

```C#

//细分对象池层级
public class PoolData
{
    //一个层级对象和它的子物体们
    public GameObject fatherObj;
    public List<GameObject> sonList;

    //将传进来物体保存到对应的层级中
    public PoolData(GameObject pushObj,GameObject poolObj) //压进来的对象，对象池最高层级
    {
        fatherObj = new GameObject(pushObj.name); //新建层级
        fatherObj.transform.parent = poolObj.transform; //将新建层级归类到对象池中
        sonList = new List<GameObject>(); //将压进来的对象，归类到新建层级中
        PushObj(pushObj);
    }

    public GameObject GetObj()
    {
        GameObject obj = null;
        obj = sonList[0];
        sonList.RemoveAt(0);
        obj.SetActive(true);
        obj.transform.parent = null;
        return obj;
    }

    public void PushObj(GameObject obj)
    {
        sonList.Add(obj);
        obj.transform.parent = fatherObj.transform;
        obj.SetActive(false);
    }

}

public class PoolManager : BaseManager<PoolManager>
{
    public Dictionary<string, PoolData> pool = new Dictionary<string, PoolData>();
    public GameObject poolObj;

    public GameObject GetPool(string name)
    {
        GameObject obj = null;

        if(pool.ContainsKey(name) && pool[name].sonList.Count > 0)
        {
            obj = pool[name].GetObj();
        }
        else
        {
            obj = GameObject.Instantiate(Resources.Load<GameObject>(name));
            obj.name = name;
        }
        return obj;
    }

    //压
    public void PushPool(string name, GameObject obj)
    {
        if (poolObj == null)
            poolObj = new GameObject("Pool");

        if (pool.ContainsKey(name))
        {
            pool[name].PushObj(obj);
        }
        else
        {
            pool.Add(name, new PoolData(obj,poolObj));
        }
    }

    //清空缓存池（例如切换场景时）
    public void ClearPool()
    {
        pool.Clear();
        poolObj = null;
    }


}

```

