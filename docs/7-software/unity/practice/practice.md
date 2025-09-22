# 🍎 简介

主要来记录练习题

# 🍎 坐标转换

## 🌲 练习1

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

## 🌲 练习2

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

## 🌲 练习1

首先我来解释一下`带转弯`和`直来直去`的区别, 根据游戏类型的不同, 如果是cs类的fps游戏, 控制方向一般是鼠标, 而方向键只控制行走, 所以是直来直去的, 如果是吃鸡里面的汽车, 那就是带转弯的, 自己好好想一下, 你很快就能get到了



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
            gameObject.AddX(-speed * Time.deltaTime);
        }

        if (Input.GetKey(KeyCode.D))
        {
            gameObject.AddX(speed * Time.deltaTime);
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
    public float movieSpeed = 10;
    public float rotateSpeed = 50;
    private void Update()
    {
        gameObject.transform.Translate(Vector3.forward * movieSpeed * Time.deltaTime * Input.GetAxis("Vertical"));
        gameObject.transform.Rotate(Vector3.up * rotateSpeed * Time.deltaTime * Input.GetAxis("Horizontal"));
    }
}
```

#### 🌼 直来直去

```cs
public class NewBehaviourScript : MonoBehaviour
{
    public float movieSpeed = 10;
    private void Update()
    {
        gameObject.transform.Translate(Vector3.forward * movieSpeed * Time.deltaTime * Input.GetAxis("Vertical"));
        gameObject.transform.Translate(Vector3.right * movieSpeed * Time.deltaTime * Input.GetAxis("Horizontal"));
    }
}
```

综合下来我们发现解法2更好, 更自然

## 🌲 练习2

在练习1的基础上让坦克头部根据鼠标来移动, 这个也是比较简单, 首先我们定义一个`headTransform`来接收坦克的头的变量, 这样就不用使用API去找, 省时省力

```cs
public class NewBehaviourScript : MonoBehaviour
{
    // 移动速度
    public float movieSpeed = 10;
    // 转弯速度
    public float rotateSpeed = 50;
    // 接收坦克的头
    public Transform headTransform;
    // 坦克头转动的速度
    public float headSpeed = 500;
    private void Update()
    {
        gameObject.transform.Translate(Vector3.forward * movieSpeed * Time.deltaTime * Input.GetAxis("Vertical"));
        gameObject.transform.Rotate(Vector3.up * rotateSpeed * Time.deltaTime * Input.GetAxis("Horizontal"));
        headTransform.Rotate(Vector3.up * headSpeed * Time.deltaTime * Input.GetAxis("Mouse X"));
    }
}
```

然后我们把坦克的头拖上去

![](images/Pasted%20image%2020250922195353.png)

然后我们运行代码试一下, 发现坦克的头可以控制了




