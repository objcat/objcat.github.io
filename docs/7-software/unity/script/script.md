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

启动游戏后可以在控制台上看到输出, 我们可以看到`Console.WriteLine`是不会显示在`unity`界面中的

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

## 🌲 Start

自己被创建出来后, 第一帧更新之前调用, 一个对象只会调用一次

## 🌲 FixedUpdate

物理帧更新, 固定间隔时间执行, 间隔时间可以设置

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

这是我们游戏中最重要的一个, 使用它来处理每一帧的逻辑

## 🌲 LateUpdate

每帧执行, 在`Update`之后执行

## 🌲 OnDisable

对象禁用时调用

## 🌲 OnDestroy

对象销毁时调用

