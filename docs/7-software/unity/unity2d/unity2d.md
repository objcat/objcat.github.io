# 🍎 快速开始

安装的话去看3d的教程, 这个主要是2d上手的文章

## 🌲 新建项目

不教了, 直接建, 建好是这样的

![](images/Pasted%20image%2020250811232734.png)

我们可以看到和3d的不一样, 它只有一个相机, 没有灯光

## 🌲 创建空对象

![](images/Pasted%20image%2020250811233034.png)

我们可以在层级上点击创建空对象来创建一个对象, 这个对象可以想象成一个容器

![](images/Pasted%20image%2020250811233140.png)

创建好后是这样, 在视图上可以看到一个圆圈

![](images/Pasted%20image%2020250811233201.png)

## 🌲 Transform

我们可以通过`transform`改变这个小圆圈的位置旋转和缩放

![](images/Pasted%20image%2020250811233316.png)

鼠标按住`X`, 可以拖动

![](images/Pasted%20image%2020250811233451.png)

我们可以看到圆圈的位置相对改变了

![](images/Pasted%20image%2020250811233505.png)

点击左上角的十字可以看到坐标系

![](images/Pasted%20image%2020250811233615.png)

我们可以拖动这个圆圈, 但这个是个空对象, 现在它没有什么意义, 我们可以尝试给他弄上一个图片

## 🌲 添加图片

unity在空对象上添加图片需要使用组件, 那我们接下来就试试吧

![](images/Pasted%20image%2020250811233854.png)

我们点击添加组件, 给它添加一个`Sprite Renderer`

![](images/Pasted%20image%2020250811234016.png)

点击精灵给他选择一个背景图片, 然后你这时候还是啥也看不到, 不要急, 你可以点击缩放把X和Y设置成50, 这样就能看到这个空对象了

![](images/Pasted%20image%2020250811234113.png)
但是这个图片太丑了, 我想把它换成我们想要的图片这怎么弄呢, 不要急我们一步一步来, 首先我们找个好的图片导入项目, 并且图片的纹理要设置为`Sprite(2D和UI)`

![](images/Pasted%20image%2020250811234551.png)

我新建了文件夹叫main, 然后把一个兔子放在里面, 那我们怎么把他应用到我们的空对象上面去呢, 很简单我们只需要拖动上去即可

![](images/Pasted%20image%2020250811234634.png)

按住兔子拖动到精灵上面即可

![](images/Pasted%20image%2020250811234802.png)

再调整一下缩放可以看到兔子上来了

你也可以直接把图片拖到场景中 这样unity也会自动给我们创建一个对象

![](images/Pasted%20image%2020250811235010.png)

## 🌲 层级

我们把图片挨的紧凑一点, 发现两张图片是有遮挡的, 那我们怎么决定哪个图片在上面呢

![](images/Pasted%20image%2020250811235711.png)

答案就是调整层级, 我们选中小的图片, 把它的层级调成1

![](images/Pasted%20image%2020250811235804.png)

然后发现它就盖到大图片上面去了

![](images/Pasted%20image%2020250811235829.png)

所以我们可以看得出来, 图层顺序变大, 就会在上层

## 🌲 脚本

我们可以使用脚本来操作图片, 这样就能实现动态控制了, 我们从最简单的移动开始

我首先创建了一个`src`用于写脚本, 然后在里面新建了一个`Move`, 双击可以使用`Visual Studio`打开, 我们定义一个变量叫`offset`

```cs
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Move : MonoBehaviour
{
    public int offset;
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

然后选中一个图片把脚本拖拽上去

![](images/Pasted%20image%2020250812001116.png)

然后我们就能够看见属性栏里面, 有这个脚本, 还有个偏移的值

![](images/Pasted%20image%2020250812001836.png)

这就属于绑定上了, 我们可以通过拖动修改这个数字, 但是图片并不会有任何变化, 那我们要如何让图片变化呢, 我们需要在脚本上去修改`transform`就可以控制了

这是写代码如果没有提示, 并且右侧显示不兼容, 就`右键从新载入一下项目`, 载入时候会出现下面的提示, 否则你即使写出来代码也不能运行

![](images/Pasted%20image%2020250812010452.png)

![](images/Pasted%20image%2020250812010320.png)
加载成功的项目是这样的

![](images/Pasted%20image%2020250812010517.png)

然后我们点击`Move.cs`编写代码, `Update`就是当变量改变的时候会自动调用方法

```cs
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Move : MonoBehaviour
{
    public int offset;
    // Start is called before the first frame update
    void Start()
    {
        
    }

    // Update is called once per frame
    void Update()
    {
        transform.position = new Vector3(offset, 0, 0);
    }
}
```

![](images/Pasted%20image%2020250812010648.png)

这时候我们运行游戏并拖拽平移发现可以进行平移移动了, 这就是我们最简单的控制了

一维控制我们学会了, 想要变成三维其实也不难了, 我们只需要声明一个`Vector3类型`的三维变量即可

```cs
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Move : MonoBehaviour
{
    public Vector3 position;
    // Start is called before the first frame update
    void Start()
    {
        
    }

    // Update is called once per frame
    void Update()
    {
        transform.position = position;
    }
}
```

界面上会出现xyz

![](images/Pasted%20image%2020250812011400.png)

我们运行游戏并拖动, 发现三维都可以更改了

## 🌲 键盘控制移动

我们既然可以通过修改数值就能移动 那我们是不是也可以通过键盘控制移动呢 当然可以

```cs
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Move : MonoBehaviour
{
    public Vector3 position;
    // Start is called before the first frame update
    void Start()
    {
        
    }

    // Update is called once per frame
    void Update()
    {
        if (Input.GetKey(KeyCode.A))
        {
            position += new Vector3(-0.1f, 0, 0);
        }
        else if (Input.GetKey(KeyCode.D))
        {
            position += new Vector3(0.1f, 0, 0);
        }
        else if (Input.GetKey(KeyCode.W))
        {
            position += new Vector3(0, 0.1f, 0);
        }
        else if (Input.GetKey(KeyCode.S))
        {
            position += new Vector3(0, -0.1f, 0);
        }

        transform.position = position;
    }
}

```

然后我们运行游戏, 并且鼠标点击下面的游戏屏幕, 如果没有这个焦点是无法捕获键盘事件的, 剩下的自己领悟

## 🌲 开关控制

我们可以通过增加一个开关来控制是否捕捉键盘事件, 我们这里写的脚本是如果不勾选键盘控制就无法用键盘移动

```cs
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Move : MonoBehaviour
{
    public Vector3 position;
    public bool keyboardControll;
    // Start is called before the first frame update
    void Start()
    {
        
    }

    // Update is called once per frame
    void Update()
    {
        if (keyboardControll)
        {
            if (Input.GetKey(KeyCode.A))
            {
                position += new Vector3(-0.1f, 0, 0);
            }
            else if (Input.GetKey(KeyCode.D))
            {
                position += new Vector3(0.1f, 0, 0);
            }
            else if (Input.GetKey(KeyCode.W))
            {
                position += new Vector3(0, 0.1f, 0);
            }
            else if (Input.GetKey(KeyCode.S))
            {
                position += new Vector3(0, -0.1f, 0);
            }
        }
        transform.position = position;
    }
}
```

保存代码后会多一个开关, 开关勾选键盘可以控制, 开关不勾选就不可以控制

![](images/Pasted%20image%2020250812014325.png)

其他的自己领悟

## 🌲 绑定渲染组件

我们先来看一下属性

![](images/Pasted%20image%2020250812122025.png)

这个属性界面是小兔子的, 我们把名字改成`rabbit small`, 把另外一个改成`rabbit big`

我们可以看到里面有一个`Sprite Render`的组件, 这个就是渲染组件

我们可以获取它并绑定到变量上

```cs
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Move : MonoBehaviour
{
    public SpriteRenderer spriteRenderer;
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

然后我们在程序中可以看到多出来一个选项

![](images/Pasted%20image%2020250812122753.png)

然后我们就可以通过拖拽来绑定了, 或者是点击最右边的小圆点使用下拉列表来选择绑定哪个对象

![](images/Pasted%20image%2020250812122924.png)

我们现在有两个兔子, 我们绑定小兔子

![](images/Pasted%20image%2020250812123017.png)

## 🌲 动态配置图片

我们拿到了这个东西就可以控制这个对象上面的所有东西了

比如我要在它启动的时候给他替换成别的兔子, 首先我找了一张别的兔子

![](images/rabbit_white.jpg)

之后我们导入项目

![](images/Pasted%20image%2020250812123541.png)

之后我们在代码里定义图片属性用于接收一个图片

```cs
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Move : MonoBehaviour
{
    public SpriteRenderer spriteRenderer;
    public Sprite picture;
    // Start is called before the first frame update
    void Start()
    {
        spriteRenderer.sprite = picture;
    }

    // Update is called once per frame
    void Update()
    {
        
    }
}
```

然后我们把图片挂在属性上, 没错就是通过拖拽或选择的

![](images/Pasted%20image%2020250812123726.png)

并在程序开始的时候把图片传给这个渲染组件的sprite属性, 这样就能实现程序在启动的时候变换图片了

![](images/Pasted%20image%2020250812123824.png)

经过上面的学习我们已经可以实现在动态的替换图片了

# 🍎 横屏模型

我们现在就来做一个横版2d的模型, 类似的游戏是魂斗罗, 超级玛丽

## 🌲 创建对象

我们的对象就使用案例中的兔子图片就行了





