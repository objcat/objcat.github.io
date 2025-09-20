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

### 🌸 解法1(不推荐)

#### 🌼 带转弯

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

但是我们发现这个转弯并不是很正常, 因为它是直来直去的, 可以看看解法2

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

我们都知道cs中点击前后左右其实是平移的, 转动鼠标才是转角

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

