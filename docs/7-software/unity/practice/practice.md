# 🍎 简介

主要来记录练习题

# 🍎 坐标转换

## 🌲 左前方置物

一个物体A, 不管它在什么位置, 只要执行这个方法, 就可以在它的左前方(-1, 0, 1)处创建一个物体

思路: 这道题还是比较简单的, 我们首先设置一个菜单`ContextMenu`也就是给脚本的...中添加一个按钮叫`左前方创建物体`

```cs
public class NewBehaviourScript : MonoBehaviour
{
    [ContextMenu("左前方创建物体")]
    private void CreateGameObject()
    {
        Vector3 vector3 = transform.TransformPoint(new Vector3(-1, 0, 1));
        GameObject gameObject1 = GameObject.CreatePrimitive(PrimitiveType.Cube);
        gameObject1.transform.position = vector3;
    }
}
```

然后当点击的时候把这个左前方的坐标转换成世界的坐标, 赋值给刚创建出来的对象即可

效果如下

![](images/Pasted%20image%2020250918124536.png)

## 🌲 三连球

一个物体不管它在什么位置, 写一个方法, 只要执行这个方法就可以在它的前方创建3个球体

位置分别是`(0, 0, 1), (0, 0, 2), (0, 0, 3)`

思路: 就是使用本地相对物体的三个坐标, 转化成世界坐标, 然后在场景上创建球体, 给球体放上`position`

```cs
public class NewBehaviourScript : MonoBehaviour
{
    [ContextMenu("创建三个球体")]
    private void create3Sphere()
    {
        GameObject sphere1 = GameObject.CreatePrimitive(PrimitiveType.Sphere);
        GameObject sphere2 = GameObject.CreatePrimitive(PrimitiveType.Sphere);
        GameObject sphere3 = GameObject.CreatePrimitive(PrimitiveType.Sphere);

        sphere1.transform.position = transform.TransformPoint(new Vector3(0, 0, 1));
        sphere2.transform.position = transform.TransformPoint(new Vector3(0, 0, 2));
        sphere3.transform.position = transform.TransformPoint(new Vector3(0, 0, 3));
    }
}
```

效果如下

![](images/Pasted%20image%2020250918124256.png)

# 🍎 方位控制

## 🌲 坦克移动

![](images/Pasted%20image%2020250922204500.png)

首先我们要有这个坦克, 拿走不谢

![](images/Tank.prefab.zip)

我来解释一下`带转弯`和`直来直去`的区别, 根据游戏类型的不同, 如果是cs类的fps游戏, 控制方向一般是鼠标, 而方向键只控制行走, 所以是直来直去的, 如果是吃鸡里面的汽车, 那就是带转弯的, 自己好好想一下, 你很快就能get到了

### 🌸 解法1(不推荐)

#### 🌼 带转弯

首先为了下面项目需要我封装了工具类

```cs
public static class ZYGameObjectExtension
{
    public static void AddX(this GameObject gameObject, float x)
    {
        Vector3 position = gameObject.transform.position;
        position.x += x;
        gameObject.transform.position = position;
    }

    public static void AddZ(this GameObject gameObject, float z)
    {
        Vector3 position = gameObject.transform.position;
        position.z += z;
        gameObject.transform.position = position;
    }

	public static void AddAnglesY(this GameObject gameObject, float y)
	{
    Vector3 angles = gameObject.transform.rotation.eulerAngles;
    angles.y += y;
    gameObject.transform.eulerAngles = angles;
	}
}
```

然后我们来看一下如何实现移动, 我们监听`ws`负责前进后退, 监听`ad`用来转向

```cs
public class NewBehaviourScript : MonoBehaviour
{
    public int speed = 10;
    private void Update()
    {
        if (Input.GetKey(KeyCode.W))
        {
            gameObject.AddZ(speed * Time.deltaTime);
        }

        if (Input.GetKey(KeyCode.S))
        {
            gameObject.AddZ(-speed * Time.deltaTime);
        }

        if (Input.GetKey(KeyCode.A))
        {
            gameObject.AddAnglesY(-speed * Time.deltaTime);
        }

        if (Input.GetKey(KeyCode.D))
        {
            gameObject.AddAnglesY(speed * Time.deltaTime);
        }
    }
}
```

实现上也看不出来啥问题, 但是就不是很正常, 不够圆润

#### 🌼 直来直去

本解法就是监听键盘的按键, 用`wasd`控制坦克的前进后退, 左右转向, 这里为了方便, 我就直接用封装类来做了

```cs
public class NewBehaviourScript : MonoBehaviour
{
	// 移动速度
    public int Speed = 10;
    private void Update()
    {
        if (Input.GetKey(KeyCode.W))
        {
            gameObject.AddZ(Speed * Time.deltaTime);
        }

        if (Input.GetKey(KeyCode.S))
        {
            gameObject.AddZ(-Speed * Time.deltaTime);
        }

        if (Input.GetKey(KeyCode.A))
        {
            gameObject.AddX(-Speed * Time.deltaTime);
        }

        if (Input.GetKey(KeyCode.D))
        {
            gameObject.AddX(Speed * Time.deltaTime);
        }
    }
}
```

因为`position`是一个结构体, 不能单独赋值, 为了简化我这里简单的封装了一下

### 🌸 解法2(推荐)

我们可以直接使用`translate`方法

#### 🌼 带转弯

```cs
public class NewBehaviourScript : MonoBehaviour
{
    // 移动速度
    public float MovieSpeed = 10;
    // 旋转速度
    public float RotateSpeed = 50;
    private void Update()
    {
        gameObject.transform.Translate(Vector3.forward * MovieSpeed * Time.deltaTime * Input.GetAxis("Vertical"));
        gameObject.transform.Rotate(Vector3.up * RotateSpeed * Time.deltaTime * Input.GetAxis("Horizontal"));
    }
}
```

#### 🌼 直来直去

```cs
public class NewBehaviourScript : MonoBehaviour
{
    public float MovieSpeed = 10;
    private void Update()
    {
        gameObject.transform.Translate(Vector3.forward * MovieSpeed * Time.deltaTime * Input.GetAxis("Vertical"));
        gameObject.transform.Translate(Vector3.right * MovieSpeed * Time.deltaTime * Input.GetAxis("Horizontal"));
    }
}
```

综合下来我们发现解法2更好, 更自然

## 🌲 坦克转头

在练习1的基础上让坦克头部根据鼠标来移动, 这个也是比较简单, 首先我们定义一个`headTransform`来接收坦克的头的变量, 这样就不用使用API去找, 省时省力

```cs
public class NewBehaviourScript : MonoBehaviour
{
    // 移动速度
    public float MovieSpeed = 10;
    // 转弯速度
    public float RotateSpeed = 50;
    // 接收坦克的头
    public Transform HeadTransform;
    // 坦克头转动的速度
    public float HeadSpeed = 500;
    private void Update()
    {
        gameObject.transform.Translate(Vector3.forward * MovieSpeed * Time.deltaTime * Input.GetAxis("Vertical"));
        gameObject.transform.Rotate(Vector3.up * RotateSpeed * Time.deltaTime * Input.GetAxis("Horizontal"));
        HeadTransform.Rotate(Vector3.up * HeadSpeed * Time.deltaTime * Input.GetAxis("Mouse X"));
    }
}
```

然后我们把坦克的头拖上去

![](images/Pasted%20image%2020250922195353.png)

然后我们运行代码试一下, 发现坦克的头可以控制了

## 🌲 坦克炮管角度

上面的我们知道后炮管就比较容易

直接上代码

```cs
public class NewBehaviourScript : MonoBehaviour
{
    // 移动速度
    public float MovieSpeed = 10;
    // 转弯速度
    public float RotateSpeed = 50;
    // 接收坦克的头
    public Transform HeadTransform;
    // 坦克头转动的速度
    public float HeadSpeed = 500;
    // 炮管儿
    public Transform GunTransform;
    // 坦克炮管速度
    public float GunSpeed = 500;
    private void Update()
    {
        gameObject.transform.Translate(Vector3.forward * MovieSpeed * Time.deltaTime * Input.GetAxis("Vertical"));
        gameObject.transform.Rotate(Vector3.up * RotateSpeed * Time.deltaTime * Input.GetAxis("Horizontal"));
        HeadTransform.Rotate(Vector3.up * HeadSpeed * Time.deltaTime * Input.GetAxis("Mouse X"));
        GunTransform.Rotate(Vector3.left * GunSpeed * Time.deltaTime * Input.mouseScrollDelta.y);
    }
}
```


![](images/Pasted%20image%2020250922203902.png)

## 🌲 转动视角

长按右键实现转动视角可以观察坦克

到这里我们的坦克还只能看到一个屁股, 那我们如何转动视角呢, 这就要用到摄像机了

### 🌸 摄像机跟着坦克

我们只需要把摄像机拖动到坦克上即可

![](images/Pasted%20image%2020250922233426.png)

然后回出现一个带绿色加号的摄像机, 说明添加成功了

![](images/Pasted%20image%2020250922233456.png)

运行游戏会看到, 摄像机跟着坦克的屁股

![](images/Pasted%20image%2020250922233657.png)

我们再给摄像机调整一个好的视角, 比如

```
UnityEditor.TransformWorldPlacementJSON:{"position":{"x":0.33000001311302187,"y":5.0,"z":-10.210000038146973},"rotation":{"x":0.1736481636762619,"y":0.0,"z":0.0,"w":0.9848077893257141},"scale":{"x":1.0,"y":1.0,"z":1.0}}
```

再给坦克加一个球做参照物

![](images/Pasted%20image%2020250925184906.png)

可以看到画面是这样的

我们目前可以看到移动坦克摄像机会跟着我们了

那如何点击右键旋转视角呢, 需要新创建一个脚本挂载摄像机上

```cs
public class CameraBehaviourScript : MonoBehaviour
{
    // 坦克
    public Transform Tank;
    // 旋转速度
    public float Speed = 100;
    // Update is called once per frame
    void Update()
    {
        // 让摄像机看向坦克
        transform.LookAt(Tank);
        // 右键按下拖动转动视角
        if (Input.GetMouseButton(1))
        {
            transform.RotateAround(Tank.position, Vector3.up, Speed * Time.deltaTime * Input.GetAxis("Mouse X"));
        }
    }
}
```

## 🌲 分屏显示

基于之前的坦克, 使用两个摄像机实现分屏效果, 一个摄像机俯视坦克跟随移动, 一个摄像机跟随炮口移动

首先分屏用到了我们的`Camera`模块中的`Viewport`知识, 我们首先把摄像机的`Viewport`调整到一半

![](images/Pasted%20image%2020250928135203.png)

然后可以看到坦克的可显示屏幕就占一半了

![](images/Pasted%20image%2020250928135324.png)

然后我们再创建一个摄像机, 让它在炮筒旁边

![](images/Pasted%20image%2020250928135820.png)

![](images/Pasted%20image%2020250928135833.png)

然后可以看看一下游戏画面

![](images/Pasted%20image%2020250928135901.png)

我们第二个相机仅仅是把它放在了坦克头的旁边, 没有写任何代码, 它会随着我们的坦克头进行转动


