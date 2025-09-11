# 🍎 简介

`Unity`可以使用多种语言做脚本, 比如`Js`, `C#`, 我们本节课使用的是`C#`

# 🍎 快速开始

## 🌲 创建脚本

我们右键点击项目窗口, 在里面选择`Create -> C# Script`

![](images/Pasted%20image%2020250823004647.png)

然后就能看到一个默认名字的脚本

![](images/Pasted%20image%2020250823004754.png)

双击这个脚本使用VS打开脚本, 脚本中的初始代码是这样的, 可以看到他默认继承`MonoBehaviour`, 类名和文件名必须一致, 只有继承于这个类才能挂载到`游戏对象`上

```cs
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class NewBehaviourScript : MonoBehaviour
{
    // Start is called before the first frame update
    void Start()
    {
        
    }

    // Update is called once per frame
    void Update()
    {
        
    }
}
```

- 继承MonoBehavior的类不能写构造函数, 因为MonoBehavior不能通过new实例化
- `Start`生命周期表示`开始`, 在物体启用后第一帧调用一次
- `Update`生命周期表示`更新`, 在物体启用后每一帧都会调用一次

## 🌲 挂脚本

所谓挂脚本, 就是我们在创建一个`游戏对象`后可以在上面绑定`一个或多个脚本`, 这些脚本可以修改游戏物体的属性和行为, 操作方法是「拖拽」

![](images/Pasted%20image%2020250823005300.png)

比如我要给这个`Cube`来挂载脚本, 我只需要把这个脚本拖拽到上面即可

![](images/Pasted%20image%2020250823005433.png)

在`Inspector`窗口中可以看到这个脚本

![](images/Pasted%20image%2020250823005529.png)

然后我们可以打开脚本进行编写

## 🌲 定义变量

挂属性就是把脚本中的属性暴露在`Inspector`中, 方便开发者进行观测和调整

```cs
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class NewBehaviourScript : MonoBehaviour
{
    public GameObject GameObject;
    public Vector3 Transform;
    void Start()
    {
        
    }

    // Update is called once per frame
    void Update()
    {
        
    }
}
```

我们定义了两个变量`gameObject`和`transform`, 分别用来表示游戏对象和物体的位置, 我们切换到`Unity`界面可以看到两个变量可以显示出来了

![](images/Pasted%20image%2020250823010230.png)

## 🌲 修改变量

我们可以通过`Inspector`来修改变量

- GameObject 我们直接把`Cube`拖拽到文本框上就可以绑定了

![](images/Pasted%20image%2020250823010324.png)

拖拽成功后可以看到, 文本框捕获了这个游戏对象的引用

![](images/Pasted%20image%2020250823010403.png)

- Transform

我们可以拖拽`X, Y, Z`来改变属性, 但是我们发现拖拽后物体并没有改变位置

那是因为我们并没有`给物体赋值`

在脚本里, 我们可以直接获取到内部变量`transform`, 用它可以控制游戏对象的位置, 我们在代码里把定义的`Transform`变量赋值到`position`属性上

```cs
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class NewBehaviourScript : MonoBehaviour
{
    public GameObject GameObject;
    public Vector3 Transform;
    void Start()
    {
        
    }

    // Update is called once per frame
    void Update()
    {
        transform.position = Transform;
    }
}
```

运行游戏, 发现可以通过改变`transform`来控制`Cube`的位置了, 你有没有想过为什么可以移动呢, 其实是因为`Update()`这个回调函数每一帧都会执行, 相当于一个循环在不断的读取数据

## 🌲 打印变量

# 🍎 生命周期

生命周期顺序图

![](images/Pasted%20image%2020250909234910.png)

## 🌲 Awake

在脚本出生时调用, 类似构造函数, 一个只会调用一次

### 🌸 打印

我们来使用一下这个生命周期函数吧, 我在里面加了几个打印

```csharp
public class NewBehaviourScript : MonoBehaviour
{
    private void Awake()
    {
        Debug.Log("Awake");
        print("1234567");
        Console.WriteLine("78888999");
    }
}
```

![](images/Pasted%20image%2020250910000141.png)

启动游戏后可以在控制台上看到输出, 我们可以看到`Console.WriteLine`是不会显示在`unity`界面中的, 那你可能会有疑问, 我以后用哪个打印呢, 我这里推荐的是, 随便用, 我们可以看看`print`源码, 其实就是封装了`Debug.Log`, 我不知道这是否算封装...

```csharp
public static void print(object message)
{
    Debug.Log(message);
}
```

### 🌸 骚操作

我们可以在游戏启动后去添加脚本

这个操作听起来很玄学, 其实可以, 并且这个还可以证明`Awake`是在脚本创建的时候调用的, 因为游戏运行后`游戏对象`已经被创建了, 比如

![](images/Pasted%20image%2020250910002225.png)

然后我们可以看到`Inspector`

![](images/Pasted%20image%2020250910002245.png)

脚本挂上去了, 然后会立刻打印出下面的内容

![](images/Pasted%20image%2020250910002307.png)

当我们停止游戏后, 我们发现刚才挂载的脚本只是临时的, 那个对象上面的组件会回到游戏运行前的状态

## 🌲 OnEnable

脚本依附的`游戏对象`每次激活时调用

```csharp
private void OnEnable()
{
    Debug.Log("OnEnable");
}
```

![](images/Pasted%20image%2020250910200956.png)

然后`激活`俩字画重点, 啥叫激活呢? 

![](images/Pasted%20image%2020250910201537.png)

看到这个对钩了吗, 我们点击后`游戏对象`就会隐藏不见, 再次点击`游戏对象`就被激活了, 我们可以看到二次打印

![](images/Pasted%20image%2020250910201630.png)

除了这个会触发其实还有一个

![](images/Pasted%20image%2020250910225320.png)

脚本取消勾选再勾选也会触发, 自己玩一玩

## 🌲 Start

自己被创建出来后, 第一帧更新之前调用, 一个对象只会调用一次

```csharp
void Start()
{
    print("Start");
}
```

## 🌲 FixedUpdate

物理帧更新, 固定间隔时间执行, 间隔时间可以设置, 主要用于物理更新

```csharp
private void FixedUpdate()
{
    print("FixedUpdate");
}
```

与`Update`类似, 可以看到它一直在调用

![](images/Pasted%20image%2020250910225759.png)

![](images/Pasted%20image%2020250910225915.png)

不一样的是, 我们可以控制它的刷新时间

![](images/Pasted%20image%2020250910230101.png)

在这里可以设置, 默认是`0.02`, 我们把他设置为1, 发现刷新的就很慢了, 点到为止

## 🌲 Update

逻辑帧更新, 每帧执行

### 🌸 帧

游戏运行的核心是一个死循环，每次循环处理游戏逻辑并更新画面, `unity`的底层已经帮我们做好这个死循环了, 因为我以前是做移动端开发的, 里面有一个概念叫`runloop`与此概念高度相似, 感兴趣的可以查一下

60帧：每秒更新60次，单帧时间 ≈ 16.66ms
30帧：每秒更新30次，单帧时间 ≈ 33.33ms

所以我们会觉得60帧流畅, 因为它每一帧处理的足够快, 不会让画面停留太久

### 🌸 游戏卡顿原因

一帧中游戏逻辑的计算量过大, 导致处理的很慢, 所以会掉帧数

### 🌸 Update能做什么

这是我们游戏中最重要的一个方法, 使用它来处理每一帧的逻辑

## 🌲 LateUpdate

每帧执行, 在`Update`之后执行, 一般用于处理摄像机更新相关的内容, 如果在`Update`中更新摄像机可以会造成渲染的错误, 比如黑屏

```csharp
private void LateUpdate()
{
    print("LateUpdate");
}
```

## 🌲 OnDisable

对象失活时调用

```csharp
private void OnDisable()
{
    print("OnDisable");
}
```

## 🌲 OnDestroy

对象销毁时调用

```csharp
private void OnDestroy()
{
    print("OnDestroy");
}
```

## 🌲 支持继承

比如我们搞一个父类继承于`MonoBehaviour`, 新加一个`BaseBehaviour.cs`

```csharp
public class BaseBehaviour : MonoBehaviour
{
    public virtual void Awake()
    {
        print("BaseBehaviour Awake");
    }
}
```

然后我们让原来的脚本继承这个

```csharp
public class NewBehaviourScript : BaseBehaviour
```

然后我们可以看到`NewBehaviourScript`也是拥有全部声明周期, 我们可以尝试重写父类中的方法

```csharp
public override void Awake()
{
    base.Awake();
    print("NewBehaviourScript Awake");
}
```

然后我们可以看到里面是能打印两个声明周期的, 因为先执行了`base.Awake()`再执行了`Awake()`明白伐? 通过这个特性我们就可以做更多事了, 自己去探索吧

# 🍎 Inspector

## 🌲 挂变量

这个我们在`快速开始`中已经学习过了, 这里来做一下增强

### 🌸 哪些变量能挂

首先我们要从成员变量的权限修饰符上下手, 我们都知道有`public protected private`, 这里面只有`public`的能挂, 因为是公开的

```csharp
public class NewBehaviourScript : MonoBehaviour
{
    public GameObject GameObject;
    public Vector3 Transform;
}
```

可以看到都能显示出来

![](images/Pasted%20image%2020250911133731.png)

我们修改一下权限

```csharp
public class NewBehaviourScript : MonoBehaviour
{
    public GameObject GameObject;
    protected Vector3 Transform;
}
```

可以看到, `Transform`就不见了, 如果把`GameObject`也改成`private`那么两个变量就都不见了, 这与我们的认知相符

![](images/Pasted%20image%2020250911133908.png)

### 🌸 注解变量

那私有和受保护的变量就无法挂载吗? 其实也是可以的, `unity`为我们提供了`SerializeField`注解

```csharp
public class NewBehaviourScript : MonoBehaviour
{
    [SerializeField]
    private GameObject GameObject;
    [SerializeField]
    protected Vector3 Transform;
}
```

加上后我们可以看到这些变量又回来了

![](images/Pasted%20image%2020250911133731.png)