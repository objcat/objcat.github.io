# 🍎 product模块(商品系统)

终于到了我们后台模块的编写环节了, 我们从第45节课开始

# 🍎 三级分类(45集)

## 🌲 介绍

我们来看一张京东的主页, 可以看到上面的商品有三级分类

![](images2/Pasted%20image%2020231004134415.jpg)

## 🌲 导入数据

视频中让我们把数据导入到表, 我们就不用导入了, 因为使用的数据库脚本里面包含了数据了, 我们可以打开表看一看

![](images2/Pasted%20image%2020231004170006.png)

在`pms_category`表中我们可以看到已经有相关的分类数据了

![](images2/Pasted%20image%2020231004170124.png)

鼠标悬停在上面可以看到对应的类型和注释

## 🌲 实现接口

数据都有了, 我们就可以跟随视频一步一步的来实现接口了, 我们首先找到`CategoryService`, 新增一个方法叫`listCategoryTree`, 注意我没有遵循视频中的命名规范, 因为我是不赞同`restful`风格的, 你大可以和视频起一个名字

```java
public interface CategoryService extends IService<CategoryEntity> {  
    PageUtils queryPage(Map<String, Object> params);  
    List<CategoryEntity> listCategoryTree();  
}
```

然后我们在`CategoryServiceImpl`中进行实现

```java
@Override  
public List<CategoryEntity> listCategoryTree() {  
    // 查询所有分类  
    List<CategoryEntity> categoryEntities = baseMapper.selectList(Wrappers.emptyWrapper());  
    // 找到所有一级分类  
    List<CategoryEntity> level1Menues = categoryEntities.stream().filter((categoryEntity) -> {  
        return categoryEntity.getParentCid() == 0;  
    }).map((menu1) -> {  
        // 给一级分类设置子分类  
        menu1.setChildren(getChildren(menu1, categoryEntities));  
        return menu1;  
    }).sorted((a, b) -> {  
        Integer sort1 = Optional.ofNullable(a).map(CategoryEntity::getSort).orElse(0);  
        Integer sort2 = Optional.ofNullable(a).map(CategoryEntity::getSort).orElse(0);  
        return sort1 - sort2;  
    }).collect(Collectors.toList());  
    // 组装成父子树形结构  
    return level1Menues;  
}  
  
private List<CategoryEntity> getChildren(CategoryEntity menu1, List<CategoryEntity> all) {  
    List<CategoryEntity> categoryEntities = all.stream().filter(entity -> {  
        // 获取1级分类的子分类, 也就是二级分类  
        // return entity.getParentCid() == menu1.getCatId();
        return Objects.equals(entity.getParentCid(), menu1.getCatId());  
    }).map((menu2) -> {  
        // 给二级分类设置子分类, 原理很简单, 就是在总表中找二级分类  
        menu2.setChildren(getChildren(menu2, all));  
        return menu2;  
    }).sorted((a, b) -> {  
        Integer sort1 = Optional.ofNullable(a).map(CategoryEntity::getSort).orElse(0);  
        Integer sort2 = Optional.ofNullable(a).map(CategoryEntity::getSort).orElse(0);  
        return sort1 - sort2;  
    }).collect(Collectors.toList());  
    return categoryEntities;  
}
```

代码是经过我修改的, 我使用了`Optional`来解决空指针异常问题, 因为我发现有一个sort为空, `SELECT * FROM pms_category WHERE cat_id=1431;`, 还有另外一个问题比较`Long`类型必须用`equals`, 因为是对象类型, ``==``只是用于比较地址, 当`Long`数值大于一定值即使相同的`值`的`地址`也会不一样, 之所以注释上是为了让你打开看看有啥bug, 「现在就告诉你, 可能会发生数据不全的问题」

然后我们在`controller`中调用它

```java
/**  
 * 列表  
 */  
@RequestMapping("/listCategoryTree")  
public R listCategory(@RequestParam Map<String, Object> params){  
    return R.ok().put("data", categoryService.listCategoryTree());  
}
```

然后我们访问一下试试吧

http://localhost:8081/product/category/listCategoryTree

可以访问通就对上了

> 白猫有话要说, 虽然有经验的同学都能写出来, 但是这对于新手来说可能是「毁灭性打击」, 所以白猫建议如果实在跟不下来直接把代码粘贴到你的项目中, 先把程序运行出来, 然后再去分析结构反推代码, 这样更有利于学习

# 🍎 菜单管理(46集)

实现三级分类后我们就可以调用接口试试了, 视频让我们去「菜单管理」首先我们要管理菜单

![](images2/Pasted%20image%2020231005130923.png)

## 🌲 删库

然后我们可以看到比视频中多了这么多东西, 我认为这些都是可以删除的, 然后自己再跟着视频慢慢创建, 这样我们自己也可以动手去做, 我们当然可以点击右侧的删除按钮一个一个的删除, 但是这会很麻烦, 所以我们可以在`datagrip`中直接写SQL删除

### 🌸 简单办法

我们可以取巧, 我们发现菜单系统菜单和用户自定义菜单的分界线是`30`

![](images2/Pasted%20image%2020231010102416.png)

所以我们只需要执行

```
DELETE FROM sys_menu WHERE menu_id > 30
```

### 🌸 复杂办法

首先我们确定数据库为`gulimall-admin -> sys_menu`, 然后我们来查询一下

```sql
# 查询一级分类
SELECT *  
FROM sys_menu  
WHERE name = '商品系统';

31,0,商品系统,"","",0,editor,0

# 查询二级分类
SELECT *  
FROM sys_menu  
WHERE parent_id = (SELECT menu_id FROM sys_menu WHERE name = '商品系统');

32,31,分类维护,product/category,"",1,menu,0
34,31,品牌管理,product/brand,"",1,editor,0
37,31,平台属性,"","",0,system,0
41,31,商品维护,product/spu,"",0,zonghe,0

# 查询三级分类
SELECT *  
FROM sys_menu  
WHERE parent_id IN  
      (SELECT menu_id FROM sys_menu WHERE parent_id = (SELECT menu_id FROM sys_menu WHERE name = '商品系统'));

38,37,属性分组,product/attrgroup,"",1,tubiao,0
39,37,规格参数,product/baseattr,"",1,log,0
40,37,销售属性,product/saleattr,"",1,zonghe,0
```

虽然查询到了, 查询完毕后我们把他们合成起来

```sql
SELECT * FROM sys_menu WHERE parent_id IN (SELECT menu_id FROM sys_menu WHERE parent_id=(SELECT menu_id FROM sys_menu WHERE name='商品系统')) OR parent_id=(SELECT menu_id FROM sys_menu WHERE name='商品系统') OR name='商品系统';

31,0,商品系统,"","",0,editor,0
32,31,分类维护,product/category,"",1,menu,0
34,31,品牌管理,product/brand,"",1,editor,0
37,31,平台属性,"","",0,system,0
38,37,属性分组,product/attrgroup,"",1,tubiao,0
39,37,规格参数,product/baseattr,"",1,log,0
40,37,销售属性,product/saleattr,"",1,zonghe,0
41,31,商品维护,product/spu,"",0,zonghe,0
```

然后只查id

```sql
SELECT menu_id  
FROM sys_menu  
WHERE parent_id IN  
      (SELECT menu_id FROM sys_menu WHERE parent_id = (SELECT menu_id FROM sys_menu WHERE name = '商品系统'))  
   OR parent_id = (SELECT menu_id FROM sys_menu WHERE name = '商品系统')  
   OR name = '商品系统';  
SELECT menu_id  
FROM sys_menu  
WHERE parent_id IN  
      (SELECT menu_id FROM sys_menu WHERE parent_id = (SELECT menu_id FROM sys_menu WHERE name = '商品系统'))  
   OR parent_id = (SELECT menu_id FROM sys_menu WHERE name = '商品系统')  
   OR name = '商品系统';

31
32
34
37
38
39
40
41
```

到这一步也没问题, 但是当我要删除的时候出错了

```sql
DELETE  
FROM sys_menu  
WHERE menu_id IN (SELECT menu_id  
                  FROM sys_menu  
                  WHERE parent_id IN (SELECT menu_id  
                                      FROM sys_menu  
                                      WHERE parent_id = (SELECT menu_id FROM sys_menu WHERE name = '商品系统'))  
                     OR parent_id = (SELECT menu_id FROM sys_menu WHERE name = '商品系统')  
                     OR name = '商品系统');

# [HY000][1093] You can't specify target table 'sys_menu' for update in FROM clause
```

报错原因是由于在删除操作中对同一张表进行了查询, 这是数据库中严令禁止的, 这个时候我们一般会通过给表起别名的方式来更新

我们把`sys_menu`换成`(SELECT * FROM sys_menu) AS s`, 然后写出了下面的语句

```sql
DELETE  
FROM sys_menu  
WHERE menu_id IN (SELECT menu_id  
                  FROM (SELECT * FROM sys_menu) AS s  
                  WHERE parent_id IN (SELECT menu_id  
                                      FROM (SELECT * FROM sys_menu) AS s  
                                      WHERE parent_id =  
                                            (SELECT menu_id FROM (SELECT * FROM sys_menu) AS s WHERE name = '商品系统'))  
                     OR parent_id = (SELECT menu_id FROM (SELECT * FROM sys_menu) AS s WHERE name = '商品系统')  
                     OR name = '商品系统');
```

然后执行我们发现删除干净了, 接下来只要把`商品系统`换成别的系统就可以删除栏目和子栏目了

![](images2/Pasted%20image%2020231005143337.png)

删除干净后我们就可以跟随视频去做了

## 🌲 跑路

开个玩笑当然不是跑路, 我们继续跟着视频做哈

## 🌲 新建「商品系统」目录

点击新增

![](images2/Pasted%20image%2020231005144247.png)

然后我们选择`目录`, 取名叫`商品系统`

![](images2/Pasted%20image%2020231005144227.png)

然后点确定保存, 然后刷新页面

![](images2/Pasted%20image%2020231005144425.png)

我们可以看到商品系统就创建出来了 

## 🌲 新建「分类维护」菜单

然后我们点击新建

![](images2/Pasted%20image%2020231005175245.png)

给商品系统下面新建一个分类维护, 用于维护分类

## 🌲 完善「分类维护」页面

我们首先点击菜单中的分类维护, 发现它跳转的页面是

http://localhost:8001/#/product-category

那我们就可以得出在vue项目中需要创建一个页面和它对应

![](images2/Pasted%20image%2020231005195004.png)

然后写一段测试代码

```vue
<template lang="">

<h1>hello world</h1>

</template>

<style lang=""></style>
```

可以看到效果是这样的, 证明我们设置的页面生效了

![](images2/Pasted%20image%2020231005194917.png)

页面确定了我们就可以来完善项目了, 首先去`element`把树形控件的代码拷贝下来

https://element.eleme.cn/#/zh-CN/component/tree

然后直接放入页面

```vue
<template lang="">
  <el-tree
    :data="data"
    :props="defaultProps"
    @node-click="handleNodeClick"
  ></el-tree>
</template>
<script>
export default {
  data() {
    return {
      data: [
        {
          label: "一级 1",
          children: [
            {
              label: "二级 1-1",
              children: [
                {
                  label: "三级 1-1-1",
                },
              ],
            },
          ],
        },
        {
          label: "一级 2",
          children: [
            {
              label: "二级 2-1",
              children: [
                {
                  label: "三级 2-1-1",
                },
              ],
            },
            {
              label: "二级 2-2",
              children: [
                {
                  label: "三级 2-2-1",
                },
              ],
            },
          ],
        },
        {
          label: "一级 3",
          children: [
            {
              label: "二级 3-1",
              children: [
                {
                  label: "三级 3-1-1",
                },
              ],
            },
            {
              label: "二级 3-2",
              children: [
                {
                  label: "三级 3-2-1",
                },
              ],
            },
          ],
        },
      ],
      defaultProps: {
        children: "children",
        label: "label",
      },
    };
  },
  methods: {
    handleNodeClick(data) {
      
    },
  },
};
</script>
<style lang=""></style>
```

然后看一下效果

![](images2/Pasted%20image%2020231006123443.png)

可以看到树形结构已经展示出来了

## 🌲 配置网关

页面结构出来后老师并没有带我们去写网络请求, 而是遇到跨域问题就直接去配置网关, 而且中途灌输了一大堆的复杂配置比如分布式配置中心, 新手容易听的云里雾里的, 所以我这里化繁为简, 先把网络请求写完, 因为还不需要远程调用, 学完后回过头再去配置网关以及网关跨域

# 🍎 分类维护(48集)

## 🌲 展示三级分类

我们直接从`48集的8分30秒`开始

我们接下来就要向后台发送网络请求获取「三级分类」然后加载到树形结构中, 首先我们要在`category.vue`中写一个网络请求

```vue
<template lang="">
  <el-tree
    :data="menus"
    :props="defaultProps"
    @node-click="handleNodeClick"
  ></el-tree>
</template>
<script>
import axios from 'axios'
export default {
  data() {
    return {
      menus: [],
      defaultProps: {
        children: "children",
        label: "label",
      },
    };
  },
  methods: {
    handleNodeClick(data) {
      
    },
    requestMenus() {
      axios.get("http://localhost:8081/product/category/listCategoryTree").then((res) => {
          
      })
    }
  },
  created() {
    this.requestMenus();
  },
};
</script>
<style lang=""></style>
```

注意我这里没有使用`人人`封装好的`httpRequest.js`而是直接使用了`axios`库, `httpRequest.js`也是基于这个库封装的, 然后我们在页面中请求

```
Access to XMLHttpRequest at 'http://localhost:8081/product/category/listCategoryTree' from origin 'http://localhost:8001' has been blocked by CORS policy: No 'Access-Control-Allow-Origin' header is present on the requested resource.
xhr.js:178     GET http://localhost:8081/product/category/listCategoryTree 
```

发现浏览器中报了如下错误, 意思就是访问跨域被禁止了, `协议, 域名, 端口`只要浏览器和服务器其中一个不同, 那就无法访问, 那我们要如何做呢, 最简单的方式是在后台加上一个注解

```java
@CrossOrigin(origins = "*")
@RequestMapping("/listCategoryTree")
public R listCategory(@RequestParam Map<String, Object> params){
	return R.ok().put("data", categoryService.listCategoryTree());
}
```

然后我们发现这次可以看到数据了, 然后我们就可以按照格式来把后台得到的数据用树形加载出来

![](images2/Pasted%20image%2020231006141456.png)

然后我们给菜单赋值, `this.menus`是数据源, 我们赋值为`data`, 然后把`label`的值改为`name`

```vue
<template lang="">
  <el-tree
    :data="menus"
    :props="defaultProps"
    @node-click="handleNodeClick"
  ></el-tree>
</template>
<script>
import axios from 'axios'
export default {
  data() {
    return {
      menus: [],
      defaultProps: {
        children: "children",
        label: "name",
      },
    };
  },
  methods: {
    handleNodeClick(data) {
      
    },
    requestMenus() {
      axios.get("http://localhost:8081/product/category/listCategoryTree").then((res) => {
          this.menus = res.data.datas;
      })
    }
  },
  created() {
    this.requestMenus();
  },
};
</script>
<style lang=""></style>
```

然后刷新

![](images2/Pasted%20image%2020231006161741.png)

我们可以看到分类菜单都能显示了, 那么怎么改变分类顺序呢, 也很简单, 我们只需要改`sort`就可以了, 因为我们是从小到大排序, 所以把`sort`改小菜单自然就上来了

比如我们想把手机放在前面, 那就在数据库把手机的`sort`改成`-1`

![](images2/Pasted%20image%2020231007134209.png)

我们再刷新页面发现手机到第一个了

![](images2/Pasted%20image%2020231007134235.png)

这就是排序原理了

## 🌲 删除分类

想要添加删除分类, 我们的UI上必须要有添加和删除按钮, 因此我们需要查询`element`文档, 找到添加按钮的方法, 这个栏目讲删除, 我们从添加按钮开始

### 🌸 添加按钮

```vue
<template>
  <el-tree :data="menus" :props="defaultProps" @node-click="handleNodeClick">
    <span class="custom-tree-node" slot-scope="{ node, data }">
      <span>{{ node.label }}</span>
      <span>
        <el-button type="text" size="mini" @click="append(data)">
          Append
        </el-button>
        <el-button type="text" size="mini" @click="remove(node, data)">
          Delete
        </el-button>
      </span>
    </span>
  </el-tree>
</template>

<script>
import axios from 'axios'

export default {
  data() {
    return {
      menus: [],
      defaultProps: {
        children: "children",
        label: "name",
      },
    };
  },
  methods: {
    handleNodeClick(data) {

    },
    requestMenus() {
      axios.get("http://localhost:8081/product/category/listCategoryTree").then((res) => {
        this.menus = res.data.datas;
      })
    },
    append(data) {
      console.log(data);
    },
    remove(node, data) {
      console.log(node);
      console.log(data);
    }
  },
  created() {
    this.requestMenus();
  },
};
</script>
```

然后我们可以看到按钮都显示出来了

![](images2/Pasted%20image%2020231007154353.png)

然后我们可以点击`Append`和`Delete`来打印当中的元素, 其中`data`就是我们的数据, `node`是节点

### 🌸 阻止事件传递

我们会发现一个问题, 就是点击`Append`和`Delete`的时候列表会打开和关闭, 这很不协调, 视频中是让设置一个属性`:expand-on-click-node="false"`, 但是我觉得这样点击范围就变小了, 也不方便, 所以我使用阻止事件传递到父视图, vue替我们封装好了, 我们使用`@click.stop`就可以阻止事件传递到父视图, 设置后点击非按钮地方都可以打开菜单了

```vue
<el-button type="text" size="mini" @click.stop="append(data)">
  Append
</el-button>
<el-button type="text" size="mini" @click.stop="remove(node, data)">
  Delete
</el-button>
```

### 🌸 隐藏逻辑

然后根据视频要求, 我们只有三级分类所以分类等于`三级`就隐藏`Append`, 如果有子节点就隐藏删除按钮, 我们一起来实现一下, 我们用`v-if`来做判断, 如果满足条件就显示, 不满足就隐藏

```vue
<el-button v-if="node.level <= 2" type="text" size="mini" @click.stop="append(data)">
  Append
</el-button>
<el-button v-if="node.childNodes.length == 0" type="text" size="mini" @click.stop="remove(node, data)">
  Delete
</el-button>
```

然后视频中让加`选择框`和`node-key`我都没添加, 用到的时候自然会加, 所以先不加, 问就是麻烦

### 🌸 接口跨域配置

之后我们就可以调用网络请求来删除数据了, 我们确定一下接口

```java
@RequestMapping("/delete")
public R delete(@RequestBody Long[] catIds){
	categoryService.removeByIds(Arrays.asList(catIds));

	return R.ok();
}
```

### 🌸 接口测试工具

这个时候老师让我们下载一个`postman`, 我不是很想使用这个, 所以我使用`REST Client`, 首先在`vscode`中下载插件

![](images2/Pasted%20image%2020231007175329.png)

然后创建一个`test.http`文件, 写入下面的内容

```
### 删除
POST http://localhost:8081/product/category/delete
Content-Type: application/json

[1]
```

### 🌸 备份数据

然后我还备份一下`category`的数据到`csv`

![](images2/Pasted%20image%2020231007175455.png)

备份的目的是可以随时还原数据的状态方便学习

### 🌸 测试接口

然后发送请求, 发现成功了, 对应的数据也没了

```json
{
  "msg": "success",
  "code": 0
}
```

说明`mybatis-plus`的`delete`接口做的是物理删除, 到那时我们开发中一般不使用这么暴力的删除方式, 因为数据删除后不好恢复, 所以我们要使用逻辑删除, 那么逻辑删除是怎么实现的呢? 我们一起来看看吧

### 🌸 配置逻辑删除

所谓逻辑删除其实就是一个标识位, 我们去找`pms_category`表中可以看到一个`show_status`

![](images2/Pasted%20image%2020231007162906.png)

这个就是区分逻辑删除的字段, 可以看注释, 1是不删除, 0是删除, 所以我们就可以通过配置删除位来达到删除的目的, 我们在配置文件中进行配置, 根据我们的规律, 没有删除是1, 删除了是0

```yml
mybatis-plus:
  # 配置mapper存放位置
  mapper-locations: classpath:mapper/**/*.xml
  # 配置日志打印策略
  configuration:
    log-impl: org.apache.ibatis.logging.nologging.NoLoggingImpl
  # 配置主键生成策略
  global-config:
    db-config:
      id-type: auto
      # 删除了
      logic-delete-value: 0
      # 没有删除
      logic-not-delete-value: 1
```

然后我们要在`CategoryEntity.java`中给`mybatis`指定逻辑删除字段

```java
@TableLogic
private Integer showStatus;
```

指定完毕后我们再测试一下, 我们发现图书已经改成了0, 说明逻辑删除配置成功了

![](images2/Pasted%20image%2020231007181151.png)

### 🌸 配置日志打印级别

新手可能会一头雾水, 这是什么原理呢? 那我们就一起来看看SQL吧, 首先我们配置一下日志打印策略

```yml
configuration:
  log-impl: org.apache.ibatis.logging.stdout.StdOutImpl
```

然后重启程序再次执行接口就能看到日志输出

```shell
Creating a new SqlSession
Registering transaction synchronization for SqlSession [org.apache.ibatis.session.defaults.DefaultSqlSession@3a4915b8]
JDBC Connection [HikariProxyConnection@658814226 wrapping com.mysql.cj.jdbc.ConnectionImpl@d54fd3] will be managed by Spring
==>  Preparing: UPDATE pms_category SET show_status=0 WHERE cat_id IN ( ? ) AND show_status=1
==> Parameters: 1(Long)
<==    Updates: 0
Releasing transactional SqlSession [org.apache.ibatis.session.defaults.DefaultSqlSession@3a4915b8]
Transaction synchronization committing SqlSession [org.apache.ibatis.session.defaults.DefaultSqlSession@3a4915b8]
Transaction synchronization deregistering SqlSession [org.apache.ibatis.session.defaults.DefaultSqlSession@3a4915b8]
Transaction synchronization closing SqlSession [org.apache.ibatis.session.defaults.DefaultSqlSession@3a4915b8]
```

我们可以看到逻辑删除只是`update`, 并不是`delete`, 所以推理可以得到加上`@TableLogic`注解后进行删除就会变成逻辑删除

### 🌸 实现删除功能

接口调通了, 我们接下来就可以在前端实现了

```js
remove(node, data) {
  axios.post("http://localhost:8081/product/category/delete", [data.catId]).then((res) => {
	console.log(res.data);
	if (res.data.code == 0) {
	  console.log("删除成功");
	  this.requestMenus();
	}
  })
}
```

首先我们调用`delete`接口, 要注意跨域注解, 否则访问不通, 当删除成功后我们可以调用`requestMenus`进行刷新, 这样列表就可以刷新了

### 🌸 发现问题

但是我们会发现一些问题, 在删除之前没有弹框提示, 删除之后没有message提示删除成功, 还有删除后菜单合起来了, 并没有展开

### 🌸 添加提示框

https://element.eleme.cn/#/zh-CN/component/dialog
https://element.eleme.cn/#/zh-CN/component/message-box

我们可以看到有两个提示框组件, 那么使用哪个呢, element告诉我们了

> 从场景上说，MessageBox 的作用是美化系统自带的 alert、confirm 和 prompt，因此适合展示较为简单的内容。如果需要弹出较为复杂的内容，请使用 Dialog。

所以我们果断选择简单的`message-box`, 可以看到代码是这样的

```js
this.$confirm('确认删除？')
	.then(_ => {
	  console.log("确定");
	})
	.catch(_ => {
	  console.log("取消");
	});
```

### 🌸 添加消息框

https://element.eleme.cn/#/zh-CN/component/message

```js
this.$message({
  message: '恭喜你，这是一条成功消息',
  type: 'success'
});
```

### 🌸 让菜单刷新后打开

我们可以换个思路, 当我删除菜单成功后我可以直接删除当前节点, 不刷新数据菜单就不会关上了, 删除节点我们使用`tree`自带的`remove`方法, 我们可以使用`ref`来引用一个组件, 然后调用组件本身的方法

### 🌸 实现删除分类

综合下来就是这个样子的

```vue
<template>
  <el-tree :data="menus" :props="defaultProps" ref="tree">
    <span class="custom-tree-node" slot-scope="{ node, data }">
      <span>{{ node.label }}</span>
      <span>
        <el-button v-if="node.level <= 2" type="text" size="mini" @click.stop="append(data)">
          添加
        </el-button>
        <el-button v-if="node.childNodes.length == 0" type="text" size="mini" @click.stop="remove(node, data)">
          删除
        </el-button>
      </span>
    </span>
  </el-tree>
</template>

<script>
import axios from 'axios'

export default {
  data() {
    return {
      menus: [],
      defaultProps: {
        children: "children",
        label: "name"
      },
    };
  },
  methods: {
    requestMenus() {
      axios.get("http://localhost:8081/product/category/listCategoryTree").then((res) => {
        this.menus = res.data.datas;
      })
    },
    append(data) {
      console.log(data);
    },
    remove(node, data) {
      this.$confirm(`确认删除「${data.name}」菜单?`)
        .then(_ => {
          // 调用删除接口
          this.delete(data, node);
        })
        .catch(e => {
          console.log(e);
          console.log("取消");
        });
    },
    delete(data, node) {
      axios.post("http://localhost:8081/product/category/delete", [data.catId]).then((res) => {
        console.log(res.data);
        // 判断是否删除成功
        if (res.data.code == 0) {
          this.$message({
            message: '删除成功!',
            type: 'success'
          });
          // 数据删除成功后再删除网页中的节点
          this.$refs.tree.remove(node);
        } else {
          this.$message({
            message: '删除失败!',
            type: 'error'
          });
        }
      })
    }
  },
  created() {
    this.requestMenus();
  },
};
</script>
```

### 🌸 实现默认展开

我们可以看到视频中没有去直接删除节点, 而是用了另外一个方法让tree默认展开, 我们在文档中可以查看到一个叫`default-expanded-keys`的属性, 值是一个数组, 那么这个数组里装什么呢, 装的就是你的`node-key`把哪个`node`的`node-key`装进去就会默认打开哪个, 所以这个时候视频前面设置的`node-key`就派上用场了, 我当时是没设置的, 因为他当时没说明这个值有什么用, 我们现在就把他配置上

```vue
<el-tree :data="menus" :props="defaultProps" ref="tree" node-key="catId" :default-expanded-keys="[4]">
```

我们可以看到视频中配置的`node-key`并不是动态的, 而是静态的, 我看当时配置这个`key`的时候弹幕上有人问不应该使用`:node-key`来配置动态的`key`吗, 这里统一解答一下, 这么配置是没错的, `tree`会根据这个字段去数据源中找到`catId`然后取出这个值作为我们的`node-key`

接下来我们配置成`4`来默认打开`catId`为`4`的菜单, 我们看一看效果吧

![](images2/Pasted%20image%2020231008153221.png)

我们可以看到数码打开了, 我们现在就实现了默认打开栏目了, 但是我不推荐用这种方式来做删除后默认打开的功能, 我觉得我的删除节点的方式就很好

## 🌲 添加分类

删除分类已经做完了, 那我们来看看添加分类, 这个就稍微复杂一点了, 因为我们要写一个添加页面

### 🌸 配置接口

我们找到`save`接口, 先配置上跨域`@CrossOrigin("*")`

```java
@CrossOrigin("*")
@RequestMapping("/save")
public R save(@RequestBody CategoryEntity category){
	categoryService.save(category);

	return R.ok();
}
```

### 🌸 全局跨域

如果你不想每个接口都配置一个跨域, 我们也可以配置一个全局跨域

```java
@Configuration
public class CorsConfig implements WebMvcConfigurer {
    @Override
    public void addCorsMappings(CorsRegistry registry) {
        registry.addMapping("/**")
                // 是否发送Cookie
                .allowCredentials(true)
                // 放行哪些原始域
//                .allowedOrigins("*")
                // 放行哪些原始域
                .allowedOriginPatterns("*")
                // 放行哪些请求方式
                .allowedMethods("GET", "POST", "PUT", "DELETE")
                // 放行哪些原始请求头部信息
                .allowedHeaders("*")
                // 暴露哪些头部信息
                .exposedHeaders("*");
    }
}
```

配置全局跨域后我们访问菜单发现出不来了

```
java.lang.IllegalArgumentException: When allowCredentials is true, allowedOrigins cannot contain the special value "*" since that cannot be set on the "Access-Control-Allow-Origin" response header. To allow credentials to a set of origins, list them explicitly or consider using "allowedOriginPatterns" instead.
	at 
```

后台报这个错误, 那是因为我们全局跨域冲突导致的, 我们需要把配置过的`@CrossOrigin("*")`删除掉

往细了说其实是`allowCredentials(true)`和`allowedOrigins("*")`冲突导致的, 必须使用`.allowedOriginPatterns("*")`, 否则你的`allowCredentials()`就要配置成`false`

### 🌸 测试接口

我们测试接口还是用`Http Client`

```
### 添加
POST http://localhost:8081/product/category/save
Content-Type: application/json

{"name": "测试1级栏目", "parentCid": 0, "catLevel": 1, "sort": 0, "showStatus": 1}
```

有了接口我们就能去实现页面了

### 🌸 实现dialog

代码如下

https://element.eleme.cn/#/zh-CN/component/dialog

```vue
<el-dialog
  title="提示"
  :visible.sync="dialogVisible"
  width="30%"
  :before-close="handleClose">
  <span>这是一段信息</span>
  <span slot="footer" class="dialog-footer">
    <el-button @click="dialogVisible = false">取 消</el-button>
    <el-button type="primary" @click="dialogVisible = false">确 定</el-button>
  </span>
</el-dialog>
```

当然不是拿来就可以用, 默认是这样的

![](images2/Pasted%20image%2020231009133143.png)

我们需要在里面加入表单

### 🌸 加入表单

我把`handleClose`去掉了, 因为没用, 把确定绑定了一个方法`determin`, 把表单填进去了

```vue
<el-dialog title="请填写信息" :visible.sync="dialogVisible" width="30%">
  <el-form ref="form" :model="form" label-width="80px">
	<el-form-item label="分类名称">
	  <el-input v-model="form.name"></el-input>
	</el-form-item>
  </el-form>
  <span slot="footer" class="dialog-footer">
	<el-button @click="dialogVisible = false">取 消</el-button>
	<el-button type="primary" @click="determin()">确 定</el-button>
  </span>
</el-dialog>
```

然后给表单构造个数据

```json
form: {
  name: ""
}
```

### 🌸 刷新列表

我们添加分类后需要在次刷新列表, 有两种实现方式, 一种是调用`append`来拼接一个节点, 但是因为我们列表中有排序不好弄, 所以我们选择和视频中一样先刷新再默认打开

### 🌸 整合上面两个组件

我们来看完整实现和视频不同的是我并没有定义`category`对象, 而是使用`category_add`计算属性`computed`, 计算属性可封装零碎的代码到方法中

```vue
<template>
  <div>
    <el-tree :data="menus" :props="defaultProps" ref="tree" node-key="catId" :default-expanded-keys="expandedKeys">
      <span class="custom-tree-node" slot-scope="{ node, data }">
        <span>{{ node.label }}</span>
        <span>
          <el-button v-if="node.level <= 2" type="text" size="mini" @click.stop="append(data)">
            添加
          </el-button>
          <el-button v-if="node.childNodes.length == 0" type="text" size="mini" @click.stop="remove(node, data)">
            删除
          </el-button>
        </span>
      </span>
    </el-tree>

    <el-dialog title="新增" :visible.sync="dialogVisible" width="30%">
      <el-form ref="form_add" :model="form_add" label-width="80px">
        <el-form-item label="分类名称">
          <el-input v-model="form_add.name"></el-input>
        </el-form-item>
      </el-form>
      <span slot="footer" class="dialog-footer">
        <el-button @click="dialogVisible = false">取 消</el-button>
        <el-button type="primary" @click="determin_add()">确 定</el-button>
      </span>
    </el-dialog>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  data() {
    return {
      menus: [],
      dialogVisible: false,
      dialogVisible_update: false,
      expandedKeys: [],
      temp_data: {},
      form_add: {
        name: ""
      },
      form_update: {
        name: ""
      },
      defaultProps: {
        children: "children",
        label: "name"
      },
    };
  },
  methods: {
    requestMenus() {
      axios.get("http://localhost:8081/product/category/listCategoryTree").then((res) => {
        if (res.data.code == 0) {
          this.menus = res.data.datas;
        }
      })
    },
    append(data) {
      // 显示对话框
      this.dialogVisible = true;
      // 缓存data
      this.temp_data = data;
    },
    remove(node, data) {
      this.$confirm(`确认删除「${data.name}」菜单?`)
        .then(_ => {
          // 调用删除接口
          this.delete(data, node);
        })
        .catch(e => {
          console.log(e);
          console.log("取消");
        });
    },
    delete(data, node) {
      axios.post("http://localhost:8081/product/category/delete", [data.catId]).then((res) => {
        console.log(res.data);
        // 判断是否删除成功
        if (res.data.code == 0) {
          this.$message({
            message: '删除成功!',
            type: 'success'
          });
          // 数据删除成功后再删除网页中的节点
          this.$refs.tree.remove(node);
        } else {
          this.$message({
            message: '删除失败!',
            type: 'error'
          });
        }
      })
    },
    determin_add() {
      // 直接使用计算属性获取
      let category_add = this.category_add;
      // 判断分类名称是否为空
      if (category_add.name == "") {
        this.$message({
          message: '分类名称不能为空',
          type: 'error'
        });
        return;
      }
      // 关闭对话框
      this.dialogVisible = false
      // 发送网络请求
      axios.post("http://localhost:8081/product/category/save", category_add).then((res) => {
        if (res.data.code == 0) {
          this.$message({
            message: '添加成功!',
            type: 'success'
          });
          // 清空表单
          this.form_add = {
            name: ""
          };
          // 新增成功后默认打开父分类
          this.expandedKeys.push(category_add.parentCid);
          // 先刷新再打开
          this.requestMenus()
        }
      }
      )
    },
  },
  computed: {
    category_add() {
      console.log(this.temp_data.catId);
      console.log(this.temp_data);
      console.log(this.temp_data.catLevel + 1);
      return {
        // 分类名
        name: this.form_add.name,
        // 父id
        parentCid: this.temp_data.catId,
        // 层级
        catLevel: this.temp_data.catLevel + 1,
        // 排序 默认0
        sort: 0,
        // 删除标记 默认1
        showStatus: 1
      }
    }
  },
  created() {
    this.requestMenus();
  },
};
</script>
```

## 🌲 修改分类

修改也简单, 我们来看一下接口

### 🌸 测试接口

首先确定接口

```java
@RequestMapping("/update")
public R update(@RequestBody CategoryEntity category){
	categoryService.updateById(category);

	return R.ok();
}
```

然后测试

```
### 修改
POST http://localhost:8081/product/category/update
Content-Type: application/json

{"catId": "3", "name": "家用电器2"}
```

### 🌸 实现页面

直接写完整版, 这也没啥好说的, 就是加了个`dialog`

```vue
<template>
  <div>
    <el-tree :data="menus" :props="defaultProps" ref="tree" node-key="catId" :default-expanded-keys="expandedKeys">
      <span class="custom-tree-node" slot-scope="{ node, data }">
        <span>{{ node.label }}</span>
        <span>
          <el-button v-if="node.level <= 2" type="text" size="mini" @click.stop="append(data)">
            添加
          </el-button>
          <el-button type="text" size="mini" @click.stop="update(node, data)">
            修改
          </el-button>
          <el-button v-if="node.childNodes.length == 0" type="text" size="mini" @click.stop="remove(node, data)">
            删除
          </el-button>
        </span>
      </span>
    </el-tree>

    <el-dialog title="新增" :visible.sync="dialogVisible" width="30%">
      <el-form ref="form_add" :model="form_add" label-width="80px">
        <el-form-item label="分类名称">
          <el-input v-model="form_add.name"></el-input>
        </el-form-item>
      </el-form>
      <span slot="footer" class="dialog-footer">
        <el-button @click="dialogVisible = false">取 消</el-button>
        <el-button type="primary" @click="determin_add()">确 定</el-button>
      </span>
    </el-dialog>

    <el-dialog title="修改" :visible.sync="dialogVisible_update" width="30%">
      <el-form ref="form_update" :model="form_update" label-width="80px">
        <el-form-item label="分类名称">
          <el-input v-model="form_update.name"></el-input>
        </el-form-item>
      </el-form>
      <span slot="footer" class="dialog-footer">
        <el-button @click="dialogVisible_update = false">取 消</el-button>
        <el-button type="primary" @click="determin_update()">确 定</el-button>
      </span>
    </el-dialog>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  data() {
    return {
      menus: [],
      dialogVisible: false,
      dialogVisible_update: false,
      expandedKeys: [],
      temp_data: {},
      form_add: {
        name: ""
      },
      form_update: {
        name: ""
      },
      defaultProps: {
        children: "children",
        label: "name"
      },
    };
  },
  methods: {
    requestMenus() {
      axios.get("http://localhost:8081/product/category/listCategoryTree").then((res) => {
        if (res.data.code == 0) {
          this.menus = res.data.datas;
        }
      })
    },
    append(data) {
      // 显示对话框
      this.dialogVisible = true;
      // 缓存data
      this.temp_data = data;
    },
    remove(node, data) {
      this.$confirm(`确认删除「${data.name}」菜单?`)
        .then(_ => {
          // 调用删除接口
          this.delete(data, node);
        })
        .catch(e => {
          console.log(e);
          console.log("取消");
        });
    },
    update(node, data) {
      // 显示对话框
      this.dialogVisible_update = true;
      // 让对话框显示原名称
      this.form_update.name = data.name;
      // 缓存data
      this.temp_data = data;
    },
    delete(data, node) {
      axios.post("http://localhost:8081/product/category/delete", [data.catId]).then((res) => {
        console.log(res.data);
        // 判断是否删除成功
        if (res.data.code == 0) {
          this.$message({
            message: '删除成功!',
            type: 'success'
          });
          // 数据删除成功后再删除网页中的节点
          this.$refs.tree.remove(node);
        } else {
          this.$message({
            message: '删除失败!',
            type: 'error'
          });
        }
      })
    },
    determin_add() {
      // 直接使用计算属性获取
      let category_add = this.category_add;
      // 判断分类名称是否为空
      if (category_add.name == "") {
        this.$message({
          message: '分类名称不能为空',
          type: 'error'
        });
        return;
      }
      // 关闭对话框
      this.dialogVisible = false
      // 发送网络请求
      axios.post("http://localhost:8081/product/category/save", category_add).then((res) => {
        if (res.data.code == 0) {
          this.$message({
            message: '添加成功!',
            type: 'success'
          });
          // 清空表单
          this.form_add = {
            name: ""
          };
          // 新增成功后默认打开父分类
          this.expandedKeys.push(category_add.parentCid);
          // 先刷新再打开
          this.requestMenus()
        }
      }
      )
    },
    determin_update() {
      // 直接使用计算属性获取
      let category_update = this.category_update;
      // 判断分类名称是否为空
      if (category_update.name == "") {
        this.$message({
          message: '分类名称不能为空',
          type: 'error'
        });
        return;
      }
      // 判断catId是否为空
      if (category_update.catId == "") {
        this.$message({
          message: '分类名称不能为空',
          type: 'error'
        });
        return;
      }
      // 关闭对话框
      this.dialogVisible_update = false;
      // 发送修改请求
      axios.post("http://localhost:8081/product/category/update", category_update).then((res) => {
        if (res.data.code == 0) {
          this.$message({
            message: '修改成功!',
            type: 'success'
          });
          // 清空表单
          this.form_update = {
            name: ""
          };
          // 新增成功后打开当前分类
          this.expandedKeys.push(this.temp_data.parentCid);
          // 先刷新再打开
          this.requestMenus()
        }
      }
      )
    }
  },
  computed: {
    category_add() {
      console.log(this.temp_data.catId);
      console.log(this.temp_data);
      console.log(this.temp_data.catLevel + 1);
      return {
        // 分类名
        name: this.form_add.name,
        // 父id
        parentCid: this.temp_data.catId,
        // 层级
        catLevel: this.temp_data.catLevel + 1,
        // 排序 默认0
        sort: 0,
        // 删除标记 默认1
        showStatus: 1
      }
    },
    category_update() {
      return {
        // 分类id
        catId: this.temp_data.catId,
        // 分类名
        name: this.form_update.name
      }
    }
  },
  created() {
    this.requestMenus();
  },
};
</script>
```

顺便提一下通过`expandedKeys`来操作默认打开分类的办法有bug, 因为视频中只实现了增加没有删除, 如果删除一个然后关上, 再删除另外一个分类中的菜单, 原来删除过分类的菜单就会被默认打开, 因为没有移除操作, 小问题以后再解决吧

## 🌲 拖拽分类

https://element.eleme.cn/#/zh-CN/component/tree

### 🌸 实现拖拽

实现拖拽只需要给`el-tree`加一个属性, 在文档中可以查看到

```
<el-tree :data="menus" :props="defaultProps" ref="tree" node-key="catId" :default-expanded-keys="expandedKeys" draggable>
```

加上`draggable`属性后我们发现页面可以被拖拽了

### 🌸 判断能否被放置

我们在拖拽的过程中会有一个操作就是把一个分类放置到另一个里面, 那我们怎么控制这种行为呢?有一个属性叫做`allow-drag`

```vue
<el-tree :data="menus" :props="defaultProps" ref="tree" node-key="catId" :default-expanded-keys="expandedKeys"
      :allow-drop="allowDrop" draggable>
```

然后我们实现方法

```js
allowDrop(draggingNode, dropNode, type) {
  if (dropNode.data.label === '二级 3-1') {
	return type !== 'inner';
  } else {
	return true;
  }
}
```

这个方法返回`false`是不能被放置, 返回`true`是可以被放置, 比如我们设置都不可以放置

```js
allowDrop(draggingNode, dropNode, type) {
  return false;
}
```

### 🌸 中断

学习到这点到为止不学了, 我认为拖拽没那么重要, 我们直接去学下一个章节, 这章的其他功能我以后再完善

# 🍎 品牌管理(59集)

品牌管理相比「分类维护」就简单多了, 我们打开`pms_band`表看一下我们的品牌

![](images2/Pasted%20image%2020231010100954.png)

## 🌲 新建「品牌管理」菜单

![](images2/Pasted%20image%2020231010103116.png)

## 🌲 前端页面

![](images2/Pasted%20image%2020231010103410.png)

我们直接去自动生成的代码中找, `resources`目录下面就有品牌管理的前端页面, 直接复制到`vscode`

## 🌲 调试页面

然后需要重新启动一下vue项目, 否则新增的vue文件不生效

![](images2/Pasted%20image%2020231010103752.png)

然后我们可以看到品牌管理页面显示出来了, 但是是一直「转圈」的状态, 我们打开控制台看一下

```
xhr.js:178     GET http://localhost:8080/renren-fast/product/brand/list?t=1696905419987&page=1&limit=10&key= 404 (Not Found)
```

我们可以清楚的看到请求的端口有些不对, 我们需要把它改正过来, 然而我认为他们的接口管理方式并不好, 所以我准备从这章开始自己写管理接口的方法

## 🌲 管理接口

由于项目中接口比较乱, 不好管理, 所以我进行了优化, 请看我是怎么管理的

首先我要找到项目中配置环境变量的地方, 我发现了`dev.env.js`, 然后我在里面配置一下

```js
'use strict'
const merge = require('webpack-merge')
const prodEnv = require('./prod.env')

module.exports = merge(prodEnv, {
  NODE_ENV: '"development"',
  OPEN_PROXY: false, // 是否开启代理, 重置后需重启vue-cli
  GULIMALL_HOST: '"http://localhost:8081"'
})
```

值得注释的是配置字符串必须是`'"字符串"'`, 否则就会报错

```shell
localhost/:158 Uncaught SyntaxError: Unexpected token ':'
    at ./node_modules/debug/src/browser.js (app.js:4642:1)
    at __webpack_require__ (app.js:678:30)
    at fn (app.js:88:20)
    at eval (url.js:7:11)
    at ./node_modules/sockjs-client/lib/utils/url.js (app.js:6944:1)
    at __webpack_require__ (app.js:678:30)
    at fn (app.js:88:20)
    at eval (websocket.js:4:16)
    at ./node_modules/sockjs-client/lib/transport/websocket.js (app.js:6832:1)
    at __webpack_require__ (app.js:678:30)
```

我不知道这是用的哪门子技术, 反正写的我唉声叹气的, 我只能给自己洗脑, 我要学的是后端, 先不管前端的破问题

然后我在`utils`下面新建一个配置文件叫做`apimap`, 然后在里面写一个`api`变量

```js
const api = {
    api_product_brand_list: process.env.GULIMALL_HOST + "/product/brand/list",
    api_product_brand_delete: process.env.GULIMALL_HOST + "/product/brand/delete",
    api_product_brand_info: process.env.GULIMALL_HOST + "/product/brand/info",
    api_product_brand_save: process.env.GULIMALL_HOST + "/product/brand/save",
}

export {
    api
}
```

## 🌲 替换接口

我们来替换一下接口, 打开`brand.vue`, 搜索`product/`, 替换如下

现在`script`中导入`api`

```js
<script>
import AddOrUpdate from './brand-add-or-update'
import { api } from  "@/utils/apimap.js"
```

然后替换掉url前面的串

```js
url: this.$http.adornUrl('/product/brand/list'),
url: api.api_product_brand_list,

url: this.$http.adornUrl('/product/brand/delete'),
url: api.api_product_brand_delete,
```

替换完成后我们发现页面出来了

![](images2/Pasted%20image%2020231010123904.png)

如果没出来就在控制台中看看有没有报错, 是不是端口号没有对上

然后我们再来替换`brand-add-or-update.vue`

```js
url: this.$http.adornUrl(`/product/brand/info/${this.dataForm.brandId}`)
url: `${api.api_product_brand_info}/${this.dataForm.brandId}`,

url: this.$http.adornUrl(`/product/brand/${!this.dataForm.brandId ? 'save' : 'update'}`),
url: `${!this.dataForm.brandId ? api.api_product_brand_save : api.api_product_brand_update}`,
```

## 🌲 开放权限

然后视频让我们给给页面开放权限, 可能是由于权限系统还不完善, 我们找到`utils/index.js`

然后在里面把`isAuth`返回值改为`true`

```js
/**
 * 是否有权限
 * @param {*} key
 */
export function isAuth (key) {
  return true;
  // return JSON.parse(sessionStorage.getItem('permissions') || '[]').indexOf(key) !== -1 || false
}
```

然后我们可以看到按钮的权限已经开放了

![](images3/Pasted%20image%2020231010130700.png)

## 🌲 修改显示状态

下面视频讲的是如何修改显示状态, 其实很简单, 就是用vue的插槽机制, 文档中可以查看到

https://element.eleme.cn/#/zh-CN/component/table

```vue
<el-table-column prop="showStatus" header-align="center" align="center" label="显示状态">
<template slot-scope="scope">
  <i class="el-icon-time"></i>
  <span style="margin-left: 10px">{{ scope.row.date }}</span>
</template>
</el-table-column>
```

然后我们发现状态已经修改过来了

![](images3/Pasted%20image%2020231010132307.png)

我们接下来就把开关替换到里面

```vue
<template slot-scope="scope">
  <el-switch v-model="value" active-color="#13ce66" inactive-color="#ff4949">
  </el-switch>
</template>
```

然后我们会发现开关点不动, 那是因为value是一个没有定义的值, 我们需要绑定一个值

```vue
<template slot-scope="scope">
  <el-switch v-model="scope.row.showStatus" active-color="#13ce66" inactive-color="#ff4949">
  </el-switch>
</template>
```

我们使用`scope`来获取数据`scope.row.showStatus`, 这里新手会一头雾水, 其实这个`showStatus`是后台返回给我们的数据, 我们请求接口看一下

```
### 品牌列表
GET http://localhost:8081/product/brand/list
Content-Type: application/json
```

```json
{
  "msg": "success",
  "code": 0,
  "page": {
    "totalCount": 0,
    "pageSize": 10,
    "totalPage": 0,
    "currPage": 1,
    "list": [
      {
        "brandId": 9,
        "name": "华为",
        "logo": "https://gulimall-hello.oss-cn-beijing.aliyuncs.com/2019-11-18/de2426bd-a689-41d0-865a-d45d1afa7cde_huawei.png",
        "descript": "华为",
        "showStatus": 1,
        "firstLetter": "H",
        "sort": 1
      },
...
```

可以看到`list`下面有一个`华为`, 上面有一个`showStatus`属性, 这下知道了吧, 我们只是通过`element`框架提供给我们获取数据的方式来获取`showStatus`属性, 然后把他绑定在`switch`上

知道了这个原理后给`brand-add-or-update.vue`做同样的修改, 注意这里获取数据需要使用`dataForm.showStatus`

```vue
<el-form-item label="显示状态" prop="showStatus">
	<el-switch v-model="dataForm.showStatus" active-color="#13ce66" inactive-color="#ff4949">
	</el-switch>
</el-form-item>
```

但是我们发现开关的状态不太对, 因为`"showStatus": 1`但是开关是关闭的

![](images3/Pasted%20image%2020231010161421.png)

这是因为`el-switch`默认只能接收`true`和`false`所以我们需要设置它的值格式`:active-value`是开, `:inactive-value`是关

```vue
<el-switch v-model="scope.row.showStatus" :active-value="1" :inactive-value="0"
active-color="#13ce66" inactive-color="#ff4949">
</el-switch>
```

## 🌲 开关修改状态

https://element.eleme.cn/#/zh-CN/component/switch#methods

### 🌸 获取开关状态

然后视频中提到了使用开关修改状态, 原理就是点击开关的时候向后台发送网络请求, 我们使用`@change`来监听开关的开启和关闭动作

```vue
<el-switch v-model="scope.row.showStatus" @change="switchChange" active-color="#13ce66" inactive-color="#ff4949">

<script>
switchChange(value) {
  console.log(value); // 1 0
}
```

可以看到开关返回的是`1`和`0`, 但是我们修改某个品牌的状态还需要这个品牌的id, 很明显使用默认的value不行, 所以我们需要改写一下方法, 把数据传过来

```vue
<el-switch v-model="scope.row.showStatus" @change="switchChange(scope.row)" active-color="#13ce66" inactive-color="#ff4949">

<script>
switchChange(row) {
  console.log(row);
}
```

这次我们打印`row`就能看到数据了

```json
showStatus: true
```

### 🌸 测试接口

我们接下来就准备接口

```
### 品牌修改
POST http://localhost:8081/product/brand/update
Content-Type: application/json

{"brandId": 9, "showStatus": true}
```

发送请求后我们发现报错了

```
{
  "timestamp": "2023-10-10T07:43:53.171+00:00",
  "status": 400,
  "error": "Bad Request",
  "path": "/product/brand/update"
}
```

去后台看一下

```
2023-10-10 15:43:53.167  WARN 21324 --- [nio-8081-exec-7] .w.s.m.s.DefaultHandlerExceptionResolver : Resolved [org.springframework.http.converter.HttpMessageNotReadableException: JSON parse error: Cannot deserialize value of type `java.lang.Integer` from Boolean value (token `JsonToken.VALUE_TRUE`); nested exception is com.fasterxml.jackson.databind.exc.MismatchedInputException: Cannot deserialize value of type `java.lang.Integer` from Boolean value (token `JsonToken.VALUE_TRUE`)<EOL> at [Source: (org.springframework.util.StreamUtils$NonClosingInputStream); line: 1, column: 30] (through reference chain: com.objcat.product.entity.BrandEntity["showStatus"])]
```

意思是`Boolean`不能转化成`Integer`, 所以是我们的传参有问题, `showStatus`需要的传参是0和1

```
### 品牌修改
POST http://localhost:8081/product/brand/update
Content-Type: application/json

{"brandId": 9, "showStatus": 1}
```

修改后发现调通了

### 🌸 实现功能

我们接下来就可以在接口中进行写了

```js
switchChange(row) {
  axios.post(api.api_product_brand_update, { "brandId": row.brandId, "showStatus": row.showStatus }).then((res) => {
	if (res.data.code == 0) {
	  this.$message({
		message: '状态修改成功!',
		type: 'success'
	  });
	}
  });
}
```

然后可以发现开关状态可以改变了

## 🌲 OSS

在微服务开发中, 由于我们可能有多个集群, 所以在保存图片的时候一般不会保存在当前服务器, 而是存放在「云服务商」的服务器中, 这样我们所有的服务器都能访问到资源, 比如我们常说的`阿里云OSS桶/七牛云/京东云/华为云`等都有类似产品

[跳转 gulimall_oss](../gulimall_oss/gulimall_oss.md)

## 🌲 第三方调用服务

从第`63集`开始, 学习完上面的章节我们已经可以向`OSS`中上传文件了, 但是如果使用`OSS`的微服务非常多, 那每个微服务中都要配置两个`key`, 这就会十分麻烦, 所以我们会把这些公用的接口抽出来, 封装成「一个微服务」, 然后通过`openFeign`的方式调用, 那我们接下来就开始创建吧

### 🌸 创建服务

我们创建`gulimall-third-party`, 我们查看一下创建好的样子

### 🌸 配置依赖

在配置依赖之前我们要先移除`product`中的`oss`依赖, 因为第三方调用被这个微服务接管了

```xml
<dependencies>
	<dependency>
		<groupId>com.objcat</groupId>
		<artifactId>gulimall-api-common</artifactId>
		<version>1.0</version>
		<exclusions>
			<exclusion>
				<groupId>com.baomidou</groupId>
				<artifactId>mybatis-plus-boot-starter</artifactId>
			</exclusion>

			<exclusion>
				<groupId>com.mysql</groupId>
				<artifactId>mysql-connector-j</artifactId>
			</exclusion>
		</exclusions>
	</dependency>

	<dependency>
		<groupId>com.alibaba.cloud</groupId>
		<artifactId>spring-cloud-starter-alicloud-oss</artifactId>
		<version>${spring-cloud-starter-alicloud-oss.version}</version>
	</dependency>
</dependencies>
```

### 🌸 配置yml

都是常规配置, 端口用的`30000`, 应用名是`gulimall-third-party`

- application.yml

```yml
server:
  # 服务端口号
  port: 30000
  servlet:
    encoding:
      # 返回数据使用utf-8编码
      charset: utf-8
      # 强制使用utf-8, 否则某些浏览器中查看会乱码
      force: true

spring:
  cloud:
    alicloud:
      access-key: xxx
      secret-key: xxx
      oss:
        endpoint: oss-cn-shanghai.aliyuncs.com
```

- bootstrap.yml

```yml
spring:
  application:
    # 配置服务的名称
    name: gulimall-third-party
  cloud:
    nacos:
      discovery:
        # 配置nacos服务端地址
        server-addr: localhost:8848
      config:
        server-addr: localhost:8848
        file-extension: yaml
```

### 🌸 启动nacos

```shell
sh startup.sh -m standalone
```

### 🌸 运行程序

我们启动程序发现报错了

```shell
***************************
APPLICATION FAILED TO START
***************************

Description:

Parameter 0 of method inetIPv6Utils in com.alibaba.cloud.nacos.util.UtilIPv6AutoConfiguration required a single bean, but 2 were found:
	- spring.cloud.inetutils-org.springframework.cloud.commons.util.InetUtilsProperties: defined in unknown location
	- inetUtilsProperties: defined by method 'inetUtilsProperties' in class path resource [org/springframework/cloud/commons/util/UtilAutoConfiguration.class]


Action:

Consider marking one of the beans as @Primary, updating the consumer to accept multiple beans, or using @Qualifier to identify the bean that should be consumed
```

别慌, 在网上我找到了解决方案设置`util.enable: false`

```
spring:
  cloud:
    alicloud:
      access-key: xxx
      secret-key: xxx
      oss:
        endpoint: oss-cn-shanghai.aliyuncs.com
    util:
      enabled: false
```

然后我们看到服务起来了

![](images3/Pasted%20image%2020231011120407.png)

### 🌸 测试oss

根据视频上要求我们要把`oss`相关的东西配置到`nacos`上去, 我们先不配置在本地测试一下, 测试好了再移动上去

```java
@SpringBootTest
public class ThirdApplicationTests {
    @Resource
    private OSSClient ossClient;

    @Test
    public void test() throws FileNotFoundException {
        // 填写Bucket名称，例如examplebucket。
        String bucketName = "gulimall2024";
        // 填写Object完整路径，完整路径中不能包含Bucket名称，例如exampledir/exampleobject.txt。
        String objectName = "exampledir/baimao.jpg";
        // 如果未指定本地路径，则默认从示例程序所属项目对应本地路径中上传文件流。
        String filePath = "/users/objcat/desktop/baimao.jpg";
        InputStream inputStream = new FileInputStream(filePath);
        // 创建PutObjectRequest对象。
        PutObjectRequest putObjectRequest = new PutObjectRequest(bucketName, objectName, inputStream);
        // 创建PutObject请求。
        PutObjectResult result = ossClient.putObject(putObjectRequest);
        System.out.println(result.getResponse());
    }
}
```

为了测试有效性我们把云上的图片删了, 重新测试上传也是可以上传成功的

### 🌸 配置中心

然后根据视频要求让我们把key配到配置中心, 视频使用命名空间区分服务, 而我不赞成这种方式, 所以我用服务名区分服务, 新建`gulimall-third-party.yaml`

![](images3/Pasted%20image%2020231011131005.png)

```
spring:
  cloud:
    alicloud:
      access-key: xxx
      secret-key: xxx
      oss:
        endpoint: oss-cn-shanghai.aliyuncs.com
    util:
      enabled: false
```

然后我们重新测试上传图片, 如果对了那么配置中心就没问题了

### 🌸 签名上传

签名上传就是web端请求后台接口获取签名, 然后由web端使用签名直接上传图片, 这样就不用占用服务器的带宽了, 我们可以参考文档, 课程中的最佳实践我已经找不到了, 新版文档是「服务端签名直传」

https://help.aliyun.com/zh/oss/use-cases/obtain-signature-information-from-the-server-and-upload-data-to-oss?spm=a2c4g.11186623.0.0.3165171dxAV8NZ

服务端签名直传是指在服务端生成签名，将签名返回给客户端，然后客户端使用签名上传文件到OSS。由于服务端签名直传无需将访问密钥暴露在前端页面，相比客户端签名直传具有更高的安全性。本文介绍如何进行服务端签名直传。

然后我们翻了一会并没有找到任何有关服务端的代码, 只找到一个客户端的代码, 所以我们要打开另一个文档

https://help.aliyun.com/zh/oss/use-cases/java-1?spm=a2c4g.11186623.0.0.43f2763fDgq3W9

核心代码如下

```java
protected void doGet(HttpServletRequest request, HttpServletResponse response)
		throws ServletException, IOException {

   // 从环境变量中获取访问凭证。运行本代码示例之前，请确保已设置环境变量OSS_ACCESS_KEY_ID和OSS_ACCESS_KEY_SECRET。
   EnvironmentVariableCredentialsProvider credentialsProvider = CredentialsProviderFactory.newEnvironmentVariableCredentialsProvider();
   // Endpoint以华东1（杭州）为例，其它Region请按实际情况填写。
   String endpoint = "oss-cn-hangzhou.aliyuncs.com"; 
   // 填写Bucket名称，例如examplebucket。
   String bucket = "examplebucket"; 
   // 填写Host名称，格式为https://bucketname.endpoint。                   
   String host = "https://examplebucket.oss-cn-hangzhou.aliyuncs.com
   // 设置上传回调URL，即回调服务器地址，用于处理应用服务器与OSS之间的通信。OSS会在文件上传完成后，把文件上传信息通过此回调URL发送给应用服务器。
   String callbackUrl = "https://192.168.0.0:8888";
   // 设置上传到OSS文件的前缀，可置空此项。置空后，文件将上传至Bucket的根目录下。
   String dir = "exampledir/"; 

	// 创建OSSClient实例。
	OSS ossClient = new OSSClientBuilder().build(endpoint, credentialsProvider);
	try {
		long expireTime = 30;
		long expireEndTime = System.currentTimeMillis() + expireTime * 1000;
		Date expiration = new Date(expireEndTime);
		// PostObject请求最大可支持的文件大小为5 GB，即CONTENT_LENGTH_RANGE为5*1024*1024*1024。
		PolicyConditions policyConds = new PolicyConditions();
		policyConds.addConditionItem(PolicyConditions.COND_CONTENT_LENGTH_RANGE, 0, 1048576000);
		policyConds.addConditionItem(MatchMode.StartWith, PolicyConditions.COND_KEY, dir);

		String postPolicy = ossClient.generatePostPolicy(expiration, policyConds);
		byte[] binaryData = postPolicy.getBytes("utf-8");
		String accessId = credentialsProvider.getCredentials().getAccessKeyId();
		String encodedPolicy = BinaryUtil.toBase64String(binaryData);
		String postSignature = ossClient.calculatePostSignature(postPolicy);

		Map<String, String> respMap = new LinkedHashMap<String, String>();
		respMap.put("accessid", accessId);
		respMap.put("policy", encodedPolicy);
		respMap.put("signature", postSignature);
		respMap.put("dir", dir);
		respMap.put("host", host);
		respMap.put("expire", String.valueOf(expireEndTime / 1000));
		// respMap.put("expire", formatISO8601Date(expiration));

		JSONObject jasonCallback = new JSONObject();
		jasonCallback.put("callbackUrl", callbackUrl);
		jasonCallback.put("callbackBody",
				"filename=${object}&size=${size}&mimeType=${mimeType}&height=${imageInfo.height}&width=${imageInfo.width}");
		jasonCallback.put("callbackBodyType", "application/x-www-form-urlencoded");
		String base64CallbackBody = BinaryUtil.toBase64String(jasonCallback.toString().getBytes());
		respMap.put("callback", base64CallbackBody);

		JSONObject ja1 = JSONObject.fromObject(respMap);
		// System.out.println(ja1.toString());
		response.setHeader("Access-Control-Allow-Origin", "*");
		response.setHeader("Access-Control-Allow-Methods", "GET, POST");
		response(request, response, ja1.toString());

	} catch (Exception e) {
		// Assert.fail(e.getMessage());
		System.out.println(e.getMessage());
	} finally { 
		ossClient.shutdown();
	}
}
```

我们可以看到代码很熟悉, 但是依然乱糟糟, 这就是阿里云文档的力量吧, 很培养程序员的能力, 我们根据视频修缮一下代码

```java
@RequestMapping("/thirdparty")
@RestController
public class OssController {

    @Resource
    private OSSClient ossClient;

    @Value("${spring.cloud.alicloud.oss.endpoint}")
    private String endpoint;

    @Value("${spring.cloud.alicloud.access-key}")
    private String accessId;

    @RequestMapping("/hello")
    public String hello() {
        return "hello world!";
    }

    @RequestMapping("/oss/getPolicy")
    public R getPolicy() {

        // 填写Host名称，格式为https://bucketname.endpoint。
        String host = "https://gulimall2024." + endpoint;
        // 我们一般用日期作为存放目录
        String date = new SimpleDateFormat("yyyy-MM-dd").format(new Date());
        String dir = date + "/";

        long expireTime = 30;
        long expireEndTime = System.currentTimeMillis() + expireTime * 1000;
        Date expiration = new Date(expireEndTime);
        // PostObject请求最大可支持的文件大小为5 GB，即CONTENT_LENGTH_RANGE为5*1024*1024*1024。
        PolicyConditions policyConds = new PolicyConditions();
        policyConds.addConditionItem(PolicyConditions.COND_CONTENT_LENGTH_RANGE, 0, 1048576000);
        policyConds.addConditionItem(MatchMode.StartWith, PolicyConditions.COND_KEY, dir);

        String postPolicy = ossClient.generatePostPolicy(expiration, policyConds);
        byte[] binaryData = new byte[0];
        try {
            binaryData = postPolicy.getBytes("utf-8");
        } catch (UnsupportedEncodingException e) {
            throw new RuntimeException(e);
        }
        String encodedPolicy = BinaryUtil.toBase64String(binaryData);
        String postSignature = ossClient.calculatePostSignature(postPolicy);

        Map<String, String> respMap = new LinkedHashMap<String, String>();
        respMap.put("accessid", accessId);
        respMap.put("policy", encodedPolicy);
        respMap.put("signature", postSignature);
        respMap.put("dir", dir);
        respMap.put("host", host);
        respMap.put("expire", String.valueOf(expireEndTime / 1000));

        ossClient.shutdown();
        return R.ok().put("data", respMap);
    }
}
```

然后我们启动服务访问一下接口

http://localhost:30000/thirdparty/oss/getPolicy

```json
{"msg":"success","code":0,"data":{"accessid":"LTAI5tMiAPE8b1cs6vGcoxya","policy":"eyJleHBpcmF0aW9uIjoiMjAyMy0xMC0xMlQwODoxMjozMC40MDlaIiwiY29uZGl0aW9ucyI6W1siY29udGVudC1sZW5ndGgtcmFuZ2UiLDAsMTA0ODU3NjAwMF0sWyJzdGFydHMtd2l0aCIsIiRrZXkiLCIyMDIzLTEwLTEyLyJdXX0=","signature":"stJwWUN6RpeAchdEYJQEaIZ8NC4=","dir":"2023-10-12/","host":"https://gulimall2024.oss-cn-shanghai.aliyuncs.com","expire":"1697098350"}}
```

发现已经可以获取了, 然后我们把接口加入到`apimap`中管理

```json
api_thirdpart_oss_policy: "http://localhost:30000" + "/thirdparty/oss/policy",
```

我们先临时写一下

### 🌸 导入上传组件

https://element.eleme.cn/#/zh-CN/component/upload

后台接口我们有了, 就可以使用`element`中的上传组件来上传文件了, 由于vue项目中已经有写好的上传文件的组件了, 我们就直接使用

![](images3/Pasted%20image%2020231012151945.png)

注意这个`upload`你可能没有, 不要着急, 从下载的资料同目录下就可以获取了, 然后我们直接去`brand-add-or-update.vue`使用

```vue
<script>
import SingleUpload from "@/components/upload/singleUpload.vue"

export default {
  components: {
    SingleUpload
  },
```

使用`import`导入并写入`components`注册, 然后我们就可以在`html`中使用了

```vue
<el-form-item label="品牌logo地址" prop="logo">
  <SingleUpload v-model="dataForm.logo" ></SingleUpload>
</el-form-item>
```

然后配置一下`html`中的地址, 我用的是上海的

```html
<el-upload action="http://gulimall2024-clouds.oss-cn-shanghai.aliyuncs.com"
```

### 🌸 配置policy地址

我们找到`upload/policy.js`, 然后修改里面的地址

```js
import http from '@/utils/httpRequest.js'
import { api } from '@/utils/apimap.js'
export function policy() {
   return  new Promise((resolve,reject)=>{
        http({
            url: api.api_thirdpart_oss_policy,
            method: "get",
            params: http.adornParams({})
        }).then(({ data }) => {
            resolve(data);
        })
    });
}
```

### 🌸 改完了测试一下吧

![](images3/Pasted%20image%2020231012162019.png)

我们发现点击上传文件, 选择一个图片上传, 控制台就报错了

```shell
Access to XMLHttpRequest at 'http://localhost:30000/thirdparty/oss/policy?t=1697098776530' from origin 'http://localhost:8001' has been blocked by CORS policy: Response to preflight request doesn't pass access control check: No 'Access-Control-Allow-Origin' header is present on the requested resource.
:30000/thirdparty/oss/policy?t=1697098776530:1     Failed to load resource: net::ERR_FAILED
createError.js:16 Uncaught (in promise) Error: Network Error
    at createError (createError.js:16:1)
    at XMLHttpRequest.handleError (xhr.js:87:1)
```

是跨域问题, 我们有两种解法

1. 在`gulimall-third-party`中配置跨域
2. 在`gulimall-gateway`中配置跨域并转发服务

很明显视频中选择了第二种, 那我们也配一配吧, 长痛不如短痛

### 🌸 配置网关(63集)

因为视频中非常重视网关, 而且网关上配置跨域挺方便, 因为配置完一次其他服务就是转发, 不用重新配置其他服务的跨域这个事情很方便, 所以我们也来配置一下吧, 网关就是转发请求用的, 本身很简单, 就是配置规则有点火星文, 不过无所谓, 跟我一起配

首先把`product`中的`nacos`打开, 让注册中心可以发现我们

```yml
spring:
  application:
    # 配置服务的名称
    name: gulimall-product
  cloud:
    nacos:
      discovery:
        # 配置nacos服务端地址
        server-addr: 127.0.0.1:8848
        enabled: true
```

然后把`product`中本身的跨域干掉, 否则会报下面的错误

```
Access to XMLHttpRequest at 'http://localhost:90/product/brand/list?t=1697101971697&page=1&limit=10&key=' from origin 'http://localhost:8001' has been blocked by CORS policy: The 'Access-Control-Allow-Origin' header contains multiple values 'http://localhost:8001, http://localhost:8001', but only one is allowed.
```

干掉的方法是不让`CorsConfig`注入容器即可

```
//@Configuration
public class CorsConfig implements WebMvcConfigurer
```

然后把服务都启动起来, 让配置中心可以读取到我们, 做这一步是为了确认都注册进来了

![](images3/Pasted%20image%2020231012164417.png)

注册进来我们开始配置, 注意`lb`是负载均衡的意思, 后面加的是我们的服务名, 有了注册中心通过服务名就能找到我们的服务

然后我们来写网关的配置文件, 与视频中不同的是我使用了`predicates`来开放`Path=/thirdparty/**`下面所有的接口, 我觉得这么配置清爽一些

```yml
server:
  # 服务端口号
  port: 90
  servlet:
    encoding:
      # 返回数据使用utf-8编码
      charset: utf-8
      # 强制使用utf-8, 否则某些浏览器中查看会乱码
      force: true

spring:
  main:
    web-application-type: reactive
  cloud:
    gateway:
      routes:
        - id: gulimall-third-party
          uri: lb://gulimall-third-party
          predicates:
            - Path=/thirdparty/**
        - id: gulimall-product
          uri: lb://gulimall-product
          predicates:
            - Path=/product/**
```

配置完需要重启网关, 然后我们调用一个接口试试能不能转发

http://localhost:90/thirdparty/oss/getPolicy

然后在网关中配置跨域, 就不用每个微服务配一遍了

```java
package com.objcat.gateway.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.cors.CorsConfiguration;
import org.springframework.web.cors.reactive.CorsWebFilter;
import org.springframework.web.cors.reactive.UrlBasedCorsConfigurationSource;

@Configuration
public class MyCorsConfiguration {

    @Bean
    CorsWebFilter corsWebFilter() {
        UrlBasedCorsConfigurationSource source = new UrlBasedCorsConfigurationSource();
        CorsConfiguration corsConfiguration = new CorsConfiguration();
        // 允许哪些请求头跨域
        corsConfiguration.addAllowedHeader("*");
        // 允许哪些请求方法跨域 get post
        corsConfiguration.addAllowedMethod("*");
        // 允许哪些地址跨域
//        corsConfiguration.addAllowedOrigin("*");
        // 高版本springboot配置跨域
        corsConfiguration.addAllowedOriginPattern("*");
        // 是否允许携带cookie 默认为false
        corsConfiguration.setAllowCredentials(true);
        source.registerCorsConfiguration("/**", corsConfiguration);
        return new CorsWebFilter(source);
    }
}
```

然后我们来配置一下前端, 因为我们统一使用网关进行转发, 所以我们首先要修改`dev.env.js`中的`GULIMALL_HOST`

```js
'use strict'
const merge = require('webpack-merge')
const prodEnv = require('./prod.env')

module.exports = merge(prodEnv, {
  NODE_ENV: '"development"',
  OPEN_PROXY: false, // 是否开启代理, 重置后需重启vue-cli
  GULIMALL_HOST: '"http://localhost:90"'
})
```

改成`90`重启服务器, 完活

然后我们修改请求工具, 这下就爽了, 有网关, 所有微服务统一, 我们把`category, brand`的接口都交给`apimap`管理

```js
const api = {
    api_product_category_listCategoryTree: process.env.GULIMALL_HOST + "/product/category/listCategoryTree",
    api_product_category_delete: process.env.GULIMALL_HOST + "/product/category/delete",
    api_product_category_save: process.env.GULIMALL_HOST + "/product/category/save",
    api_product_category_update: process.env.GULIMALL_HOST + "/product/category/update",

    api_product_brand_list: process.env.GULIMALL_HOST + "/product/brand/list",
    api_product_brand_delete: process.env.GULIMALL_HOST + "/product/brand/delete",
    api_product_brand_info: process.env.GULIMALL_HOST + "/product/brand/info",
    api_product_brand_save: process.env.GULIMALL_HOST + "/product/brand/save",
    api_product_brand_update: process.env.GULIMALL_HOST + "/product/brand/update",
    
    api_thirdpart_oss_policy: process.env.GULIMALL_HOST + "/thirdparty/oss/getPolicy",
}

export {
    api
}
```

然后`product`中写死的路径也换成变量, 这样我们就得到了一个「崭新的」接口列表了

### 🌸 配置阿里云跨域

然后我们上传试试吧, 我们发现又报错了

```shell
Access to XMLHttpRequest at 'http://gulimall2024-clouds.oss-cn-shanghai.aliyuncs.com/' from origin 'http://localhost:8001' has been blocked by CORS policy: Response to preflight request doesn't pass access control check: No 'Access-Control-Allow-Origin' header is present on the requested resource.
```

出现这个说明我们跨域配置对了, 我们接下来要配置oss的跨域, 我们登录阿里云后台, 找到跨域设置

![](images3/Pasted%20image%2020231012170807.png)

点击创建规则

![](images3/Pasted%20image%2020231012170851.png)

然后按图中添上, 点击确定

### 🌸 测试上传

然后我们测试一下上传

```
:8001/#/product-brand:1 Access to XMLHttpRequest at 'http://gulimall2024-clouds.oss-cn-shanghai.aliyuncs.com/' from origin 'http://localhost:8001' has been blocked by CORS policy: Response to preflight request doesn't pass access control check: No 'Access-Control-Allow-Origin' header is present on the requested resource.
upload.js:599     POST http://gulimall2024-clouds.oss-cn-shanghai.aliyuncs.com/ net::ERR_FAILED
```

还是这熊样, 气炸了

![](images3/Pasted%20image%2020231012173348.png)

不知道是时间没到还是参数配置错了, 暂时就更新到这里吧, 接下来是等待, 等了很长时间也没有用, 所以查看了一下配置发现了问题

```
// 老版本上传地址
https://gulimall2024-clouds.oss-cn-shanghai.aliyuncs.com
// 新版本上传地址
https://gulimall2024.oss-cn-shanghai.aliyuncs.com
```

我只是改了地区发现是不行的, 所以问题就在于上传地址的问题, 再次测试上传发现成功了

![](images3/Pasted%20image%2020231012175204.png)

我们发现图片可以出来了

![](images3/Pasted%20image%2020231015094657.png)

点击确定就可以保存了, 到此我们第三方服务也创建好了, 白猫提醒一下, 学习这里需要「得过且过」, 因为上传是非常复杂的, 对于新手来说要先以学习后台为重点, 实在搞不明白的就先把功能做出来, 然后往后学习, 而不是一直卡住, 否则很容易放弃

## 🌲 品牌logo显示图片

我们可以看到`品牌logo`是地址样式的, 这不太好看, 所以我们可以把他换成图片显示

![](images3/Pasted%20image%2020231015095549.png)

我们直接把图片组件贴上就行了

https://element.eleme.cn/#/zh-CN/component/image

```vue
<el-table-column prop="logo" header-align="center" align="center" label="品牌logo地址">
<template slot-scope="scope">
  <el-image style="width: 100px; height: 100px" :src="scope.row.logo" fit="fit"></el-image>
</template>
</el-table-column>
```

然后我们发现报错了, 我我们的`el-image`没有注册, 然后我们去`main.js`可以看到, 项目开启了部分引入, 引入模块的地方在`element-ui/index.js`中, 视频中是带我们把组件都添加上, 这样会很麻烦, 还要查缺补漏, 所以我们可以直接全部导入

```js
// import '@/element-ui'

import ElementUI from 'element-ui';
Vue.use(ElementUI)
```

完活, 回来看图片已经可以显示了

![](images3/Pasted%20image%2020231015110434.png)

但是我们会发现图片样式不对, 这是由于项目`main.js`使用的主题`css`样式有问题, 我们把他改成默认的

```js
// import '@/element-ui-theme'
import 'element-ui/lib/theme-chalk/index.css';
```

然后发现图片就可以正常显示了

![](images3/Pasted%20image%2020231015110700.png)

## 🌲 重构后台管理系统

由于老的客户端过于臃肿, 我用了两天时间使用`vite`重构客户端, 新客户端的名字叫`renren-fast-vite`, 可以直接在项目根目录中找到, 新老版本我都保留了, 但是以后我将全面替换成新的客户端, 我使用`yarn`进行包管理

```shell
# 安装yarn
sudo npm install -g yarn
# 拉取依赖
yarn
# 启动
yarn dev
```

## 🌲 完善表单校验(65集)

### 🌸 前端

https://element.eleme.cn/#/zh-CN/component/form

这集的后半段开始讲修改表单的校验规则了, 我觉得这并不是很重要, 但是为了以学习为目的还是简的做一做, 我们打开`brand-add-or-update.vue`先看看原来的规则

```js
dataRule: {
	name: [
	  { required: true, message: '品牌名不能为空', trigger: 'blur' }
	],
	logo: [
	  { required: true, message: '品牌logo地址不能为空', trigger: 'blur' }
	],
	descript: [
	  { required: true, message: '介绍不能为空', trigger: 'blur' }
	],
	showStatus: [
	  { required: true, message: '显示状态不能为空', trigger: 'blur' }
	],
	firstLetter: [
	  { required: true, message: '检索首字母不能为空', trigger: 'blur' }
	],
	sort: [
	  { required: true, message: '排序不能为空', trigger: 'blur' }
	]
  }
```

自带的规则比较简单, 就是不能为空, 视频中讲的是自定义校验规则

```js
dataRule: {
	name: [
	  { required: true, message: '品牌名不能为空', trigger: 'blur' }
	],
	logo: [
	  { required: true, message: '品牌logo地址不能为空', trigger: 'blur' }
	],
	descript: [
	  { required: true, message: '介绍不能为空', trigger: 'blur' }
	],
	showStatus: [
	  { required: true, message: '显示状态不能为空', trigger: 'blur' }
	],
	firstLetter: [
	  {
		validator: (rule, value, callback) => {
		  if (value == '') {
			callback(new Error("必须填写首字母"));
		  } else if (!/^[a-zA-Z]$/.test(value)) {
			callback(new Error("必须是英文字母, 并且只有一个"));
		  } else {
			callback();
		  }
		}, trigger: 'blur'
	  }
	],
	sort: [
	  { validator: (rule, value, callback) => {
		  if (value == '') {
			callback(new Error("排序字段必须填写"));
		  } else if (!Number.isInteger) {
			callback(new Error("排序字段必须是整数"));
		  } else {
			callback();
		  }
		}, trigger: 'blur' }
	]
  }
```

自定义规则也简单, 就是添加一个`validator`, 然后里面用一个箭头函数写校验规则就可以了

### 🌸 后端

不仅前端需要校验, 后端的校验更加重要, 因为有些情况下用户是直接调用接口去访问服务的, 而不是在前端页面点点点, 那我们就要进行后端校验了

#### 🌵 javax.validation校验

视频中讲的是使用`JSR303`进行校验`JSR303`是`Java`为`Bean`数据合法性校验提供给的标准框架

##### 🐔 校验注解

首先在接口的实体前使用校验注解, 把参数加入校验

```java
@RequestMapping("/save")
public R save(@Valid @RequestBody BrandEntity brand){
	brandService.save(brand);

	return R.ok();
}
```

然后我们去`BrandEntity`实体里面使用注解来校验名称

```java
/**
 * 品牌名
 */
@NotBlank
private String name;
```

意思是`不能为空`和`不能为空格`, 重启服务, 然后我们调用接口访问一下

##### 🐔 测试

```
### 品牌新增
POST http://localhost:90/product/brand/save
Content-Type: application/json

{"name": ""}
```

我们会发现访问不通, 错误如下

```json
{
  "timestamp": "2023-10-18T06:57:35.822+00:00",
  "status": 400,
  "error": "Bad Request",
  "path": "/product/brand/save"
}
```

然后我们来看一下后台

```shell
2023-10-18 14:57:35.812  WARN 26788 --- [nio-8081-exec-1] .w.s.m.s.DefaultHandlerExceptionResolver : Resolved [org.springframework.web.bind.MethodArgumentNotValidException: Validation failed for argument [0] in public com.objcat.common.utils.R com.objcat.product.controller.BrandController.save(com.objcat.product.entity.BrandEntity): [Field error in object 'brandEntity' on field 'name': rejected value []; codes [NotBlank.brandEntity.name,NotBlank.name,NotBlank.java.lang.String,NotBlank]; arguments [org.springframework.context.support.DefaultMessageSourceResolvable: codes [brandEntity.name,name]; arguments []; default message [name]]; default message [不能为空]] ]
```

##### 🐔 优化json返回

我们可以看到, 是`Validation failed`的错误, 但是响应`json`的提示是不尽人意的, 里面甚至连错误信息都没有, 我们要怎么获取错误信息呢?

```java
@RequestMapping("/save")
public R save(@Valid @RequestBody BrandEntity brand, BindingResult result) {
	if (result.hasErrors()) {
		Map<String, Object> map = new HashMap<>();
		result.getFieldErrors().forEach((item) -> {
			map.put(item.getField(), item.getDefaultMessage());
		});
		return R.error(400, "提交的数据不合法").put("data", map);
	} brandService.save(brand);
	return R.ok();
}
```

我们可以写一个`BindingResult`来判断校验有没有出错, 然后我们访问一下

```json
{
  "msg": "提交的数据不合法",
  "code": 400,
  "data": {
    "name": "不能为空"
  }
}
```

我们可以看到提示语是`不能为空`, 这句话是怎么来的呢? 我们可以在程序里找一找

```java
public @interface NotBlank {
    String message() default "{javax.validation.constraints.NotBlank.message}";
```

我们点进去注解可以看到一段话, 它是读取这个变量, 我们在程序中再去搜索这个变量

![](images3/Pasted%20image%2020231018232926.png)

我们可以看到它在一个定义好的文件里, 是一个`properties`, 到这里提示消息也理解了

##### 🐔 修改提示语

我们可以修改校验出错后的提示语, 比如我们给`logo`也添加上校验不能为空, 并且这次我们写一个段提示语

```java
/**
 * 品牌logo地址
 */
@NotBlank(message = "logo不能为空啊大哥")
private String logo;
```

然后我们再来看一下请求数据

```json
{
  "msg": "提交的数据不合法",
  "code": 400,
  "data": {
    "name": "不能为空",
    "logo": "logo不能为空啊大哥"
  }
}
```

我们发现`logo`的校验信息也能提示了

##### 🐔 校验URL

我们使用URL注解来校验URL

```java
@NotBlank(message = "logo不能为空啊大哥")
@URL(message = "logo必须是URL")
private String logo;
```

然后我们测试一下

```
### 品牌新增
POST http://localhost:90/product/brand/save
Content-Type: application/json

{"logo": "1234"}
```

当`logo`不为空时回去校验`URL`

```json
{
  "msg": "提交的数据不合法",
  "code": 400,
  "data": {
    "name": "不能为空",
    "logo": "logo必须是URL"
  }
}
```

##### 🐔 自定义校验

我们可以使用正则表达式来实现自定义的校验, 我们一起来看看吧

```java
@NotBlank(message = "检索首字母不能为空")
@Pattern(regexp = "/*[a-zA-Z]$/", message = "首字母必须是字母并且只有一个")
private String firstLetter;
```

我们测试一下

```java
### 品牌新增
POST http://localhost:90/product/brand/save
Content-Type: application/json

{"logo": "1234", "firstLetter": "1"}
```

我们看一下返回

```json
"data": {
    "name": "不能为空",
    "logo": "logo必须是URL",
    "firstLetter": "首字母必须是字母并且只有一个"
}
```

这里需要注意一点`@NotBlank`是必须加上的, 否则没有`firstLetter`这个参数就会不校验, 那么程序就是`有bug`的

##### 🐔 全局异常处理

通过上面的练习, 我们可以捕获校验异常并返回一个正常的有用的信息了,  但是如果我们写的接口很多, 这种处理方式还是很麻烦的, 所以我们就在想有没有更方便的方法捕获异常并返回对应的`json`结构, 是有的, 我们可以使用`@ControllerAdvice`, 首先我们创建一个`exception`包然后新建文件`GulimallExceptionControllerAdvice.java`写入下面内容, 需要注意的是`BindingResult result`需要去掉, 否则异常会被它捕获, 就到不了全局异常处理了

```java
@Slf4j
@ControllerAdvice
public class GulimallExceptionControllerAdvice {

    @ResponseBody
    @ExceptionHandler(value = Exception.class)
    public R exceptionHandler(Exception e) {
        log.info("捕获到异常 {}", e.toString());
        return R.error(e.toString());
    }
}
// 也可以写成下面的样子
@ResponseBody
@ExceptionHandler(value = Throwable.class)
public R exceptionHandler(Throwable e) {
	log.info("捕获到异常 {}", e.toString());
	return R.error(e.toString());
}
```

这里再加一句, 看视频的时候有人说这个`GulimallExceptionControllerAdvice`应该定义到`common`中, 我同意你的看法, 但是当你定义到`common`中的时候你会发现它不起作用, 这是为什么呢? 其实很简单, product启动的之后`spring`默认扫自己启动文件所在的包, 但并不会去扫你`common`下的包, 问题出来了我们就要配置扫包

```java
@SpringBootApplication
@ComponentScan(basePackages = {"com.objcat.product", "com.objcat.common"})
public class ProductApplication {
    public static void main(String[] args) {
        SpringApplication.run(ProductApplication.class, args);
    }
}
```

我们把自己的包名和`com.objcat.common`的包名配置到`@ComponentScan`注解下就可以在启动时把`common`中的`bean`放入`spring容器`管理了, 这只是其中一种方法, 我们先用着

然后我们访问`save`接口返现错误被拦截到了

```json
{
  "msg": "org.springframework.web.bind.MethodArgumentNotValidException: Validation failed for argument [0] in public com.objcat.common.utils.R com.objcat.product.controller.BrandController.save(com.objcat.product.entity.BrandEntity) with 3 errors: [Field error in object 'brandEntity' on field 'logo': rejected value [1234]; codes [URL.brandEntity.logo,URL.logo,URL.java.lang.String,URL]; arguments [org.springframework.context.support.DefaultMessageSourceResolvable: codes [brandEntity.logo,logo]; arguments []; default message [logo],[Ljavax.validation.constraints.Pattern$Flag;@589007ce,,-1,,.*]; default message [logo必须是URL]] [Field error in object 'brandEntity' on field 'name': rejected value [null]; codes [NotBlank.brandEntity.name,NotBlank.name,NotBlank.java.lang.String,NotBlank]; arguments [org.springframework.context.support.DefaultMessageSourceResolvable: codes [brandEntity.name,name]; arguments []; default message [name]]; default message [不能为空]] [Field error in object 'brandEntity' on field 'firstLetter': rejected value [1]; codes [Pattern.brandEntity.firstLetter,Pattern.firstLetter,Pattern.java.lang.String,Pattern]; arguments [org.springframework.context.support.DefaultMessageSourceResolvable: codes [brandEntity.firstLetter,firstLetter]; arguments []; default message [firstLetter],[Ljavax.validation.constraints.Pattern$Flag;@5f54e3c4,/*[a-zA-Z]$/]; default message [首字母必须是字母并且只有一个]] ",
  "code": 500
}
```

我们可以在上面的报错提示中看到, 异常是一个`MethodArgumentNotValidException`, 翻译过来是参数没有通过校验的一个错误, 那么这个时候我们就应该把异常精确化一下, 添加一个`methodArgumentNotValidExceptionHandler`

```java
@ResponseBody
@ExceptionHandler(value = MethodArgumentNotValidException.class)
public R methodArgumentNotValidExceptionHandler(MethodArgumentNotValidException e) {
	log.info("methodArgumentNotValidExceptionHandler: 拦截到错误{}", e.toString());
	Map<String, Object> map = new HashMap<>();
	e.getBindingResult().getFieldErrors().forEach((fieldError) -> {
		map.put(fieldError.getField(), fieldError.getDefaultMessage());
	});
	return R.error(400, "参数格式校验失败").put("data", map);
}
```

然后我们测试一下发现可以正常提示错误了

```json
{
  "msg": "未知异常，请联系管理员",
  "code": 500,
  "data": {
    "name": "不能为空",
    "logo": "logo必须是URL",
    "firstLetter": "首字母必须是字母并且只有一个"
  }
}
```

而且我们发现拦截异常的时候会先拦截`MethodArgumentNotValidException`如果不是这个异常才会走`exceptionHandler`

##### 🐔 公共状态码

我们发现状态码都是我们手写的, 这样很不规范, 所以我们可以在`common`模块中去定义枚举来管理状态码, 我们在`common`中建立一个枚举`BizCodeEnum`

```java
public enum BizCodeEnum {
    UNKNOW_EXCEPTION(10000, "系统未知异常"),
    VAILD_EXCEPTION(10002, "参数格式校验失败");

    private int code;
    private String msg;

    BizCodeEnum(int code, String msg) {
        this.code = code;
        this.msg = msg;
    }

    public int getCode() {
        return code;
    }

    public String getMsg() {
        return msg;
    }
}
```

然后就能愉快的使用状态码了

```java
@ResponseBody
@ExceptionHandler(value = MethodArgumentNotValidException.class)
public R methodArgumentNotValidExceptionHandler(MethodArgumentNotValidException e) {
	log.info("methodArgumentNotValidExceptionHandler: 拦截到错误{}", e.toString());
	Map<String, Object> map = new HashMap<>();
	e.getBindingResult().getFieldErrors().forEach((fieldError) -> {
		map.put(fieldError.getField(), fieldError.getDefaultMessage());
	});
	return R.error(BizCodeEnum.VAILD_EXCEPTION.getCode(), BizCodeEnum.VAILD_EXCEPTION.getMsg()).put("data", map);
}
```

我们看一下返回的数据

```json
{
  "msg": "参数格式校验失败",
  "code": 10002,
  "data": {
    "name": "不能为空",
    "logo": "logo必须是URL",
    "firstLetter": "首字母必须是字母并且只有一个"
  }
}
```

##### 🐔 分组校验

有时候我们使用同一个`实体`来进行不同的`CRUD`操作的时候, 它们需要校验的属性可能是不同的, 比如新增的时候我们就不需要去校验`id`, 而修改的时候我们是需要校验`id`不为空的, 这就需要给校验添加分组, 主要依靠注解上面的`group`属性, 而为了标注分组, 我们需要建立两个接口类, 不用写内容

```java
public interface UpdateGroup {
}

public interface UpdateGroup {
}
```

然后我们在`实体`上标注分组, 意思是修改的时候id不能为空, 在新增的时候id一定要为空

```java
@TableId
@NotNull(message = "修改必须指定品牌id", groups = {UpdateGroup.class})
@Null(message = "新增不需要品牌id", groups = {AddGroup.class})
private Long brandId;
```

然后我们修改接口, 让它可以分组, 我们使用`@Validated`里面携带`group`来指定分组校验

```java
/**
 * 保存
 */
@RequestMapping("/save")
public R save(@Validated({AddGroup.class}) @RequestBody BrandEntity brand) {
	return R.ok();
}

/**
 * 修改
 */
@RequestMapping("/update")
public R update(@Validated({UpdateGroup.class}) @RequestBody BrandEntity brand) {
	brandService.updateById(brand);

	return R.ok();
}
```

然后我们发送请求试试

```json
### 品牌新增
POST http://localhost:8081/product/brand/save
Content-Type: application/json

{"brandId": "123"}
```

然后看看返回数据

```json
{
  "msg": "参数格式校验失败",
  "code": 10002,
  "data": {
    "brandId": "新增不需要品牌id"
  }
}
```

我们发现新增不需要`id`这个校验成功了, 但是你会发现一个问题, 就是它不校验品牌名是否为空了, 这就是分组的隔离性, 分组只检验分组里面的, 因为`name`没有在分组中, 所以不校验, 想要校验我们需要把它加入分组, 因为`groups`是个数组, 所以可以添加多个分组

```java
/**
 * 品牌名
 */
@NotBlank(groups = {AddGroup.class, UpdateGroup.class})
private String name;
```

配置好分组后, 我们发现它有可以校验了

```json
{
  "msg": "参数格式校验失败",
  "code": 10002,
  "data": {
    "brandId": "新增不需要品牌id",
    "name": "不能为空"
  }
}
```

##### 🐔 有点问题

但是如果这样设置, 那么我们修改的时候会有点问题, 因为修改都是增量更新, 不传值就不改, 如果我传的是`id`和`logo`, 那么意思就是只想更新`logo`, 但是你却不让我改, 说名字不能为空, 这逻辑是说不通的, 我们先不说这个问题埋一个伏笔

##### 🐔 自定义注解

有时候我们使用自带的注解已经不能达到校验的目的了, 那么这时候就要`自定义校验注解`

```java
@Documented
@Constraint(validatedBy = {ListValueConstraintValidator.class})
@Target({ElementType.METHOD, ElementType.FIELD, ElementType.ANNOTATION_TYPE, ElementType.CONSTRUCTOR, ElementType.PARAMETER, ElementType.TYPE_USE})
@Retention(RetentionPolicy.RUNTIME)
public @interface ListValue {
    String message() default "必须提交指定的值 0,1";

    Class<?>[] groups() default {};

    Class<? extends Payload>[] payload() default {};

    int[] vals() default {};
}
```

`message`的值我们也可以写在一个文件中, 在`resources`目录下新建一个`ValidationMessages.properties`

```java
com.objcat.common.valid.listValue.message=必须提交指定的值 0,1
```

然后把属性改成这样

```java
String message() default "{com.objcat.common.valid.listValue.message}";
```

有时候你可能会发现`Payload`报红, 那么你就要跟视频中一样引入一个依赖库

```xml
<dependency>
    <groupId>javax.validation</groupId>
    <artifactId>validation-api</artifactId>
    <version>2.0.1.Final</version>
</dependency>
```

然后我们需要写一个校验器

```java
public class ListValueConstraintValidator implements ConstraintValidator<ListValue, Integer> {

    private Set<Integer> set = new HashSet<>();

    @Override
    public void initialize(ListValue constraintAnnotation) {
        ConstraintValidator.super.initialize(constraintAnnotation);

        int[] vals = constraintAnnotation.vals();
        for (int val : vals) {
            set.add(val);
        }

    }

    /***
     * description: 校验值 <br>
     * version: 1.0 <br>
     * date: 2023/10/19 13:09 <br>
     * author: objcat <br>
     *
     * @param integer 当前校验数字
     * @param constraintValidatorContext 校验上下文
     * @return boolean
     */
    @Override
    public boolean isValid(Integer integer, ConstraintValidatorContext constraintValidatorContext) {
        return set.contains(integer);
    }
}
```

可以看到还是非常简单的, 我们初始化的时候把`vals`放到集合, 然后调用`contains`方法来判断值是不是规定范围的, 我们访问一下接口

```java
### 品牌新增
POST http://localhost:8081/product/brand/save
Content-Type: application/json

{"showStatus": 2}
```

然后我们来看一下返回值

```json
{
  "msg": "参数格式校验失败",
  "code": 10002,
  "data": {
    "name": "不能为空",
    "showStatus": "必须提交指定的值 0,1"
  }
}
```

可以看到我们传`2`的时候校验是不通过的

##### 🐔 有话要说

这章学完之后, 如果你觉得`问题很大`, 那么你可能是高手, 如果你觉得没啥问题, 那么你就是个新手, 不过无论是`高手`还是`新手`我推荐你们把注解都注释掉, 否则可能会出现一大堆的问题, 比如想修改一个状态但是因为名字为空不能进行修改, 我们先学这种思想以后再优化

#### 🌵 spring-validation校验

##### 🐔 导入依赖

首先我们要在`common`中导入依赖

```xml
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-validation</artifactId>
</dependency>
```

因为老师并没有去讲这个, 所以我这个坑暂时挖着, 先学下面的以后可能把这个地方补上

# 🍎 属性分组(70集)

## 🌲 SPU/SKU

SPU：Standard Product Unit，标准产品单元，可以理解为一个产品型号，比如上面图片看到的iPhone 14 (A2884) 就是一个标准的产品单元，它属于生产制造过程的一个标准品，标准品在缺乏具体规格信息的时候是不能直接售卖的（除非这个产品系列只有一个规格）。

SKU：Stock Keeping Unit，最小库存单元，也就是对应仓库中的一件商品，这个商品的规格信息在入库的时候就已经确定了的，因此是可以直接售卖的。

## 🌲 规格参数与销售属性

- 属性是以三级分类组织起来的
- 规格参数中有些是可以提供检索的
- 规格参数也是基本属性, 他们具有自己的分组
- 属性的分组也是以三级分类组织起来的
- 属性名确定的, 但是值是根据每一个商品不同来决定的

## 🌲 建表

当然谷粒商城已经把表给我们建好了, 我们只需要拿来用, 但是我们也要理解表中是怎么建的

### 🌸 属性表(pms_attr)

```sql
CREATE TABLE pms_attr
(
    attr_id      bigint AUTO_INCREMENT COMMENT '属性id'
        PRIMARY KEY,
    attr_name    char(30)     NULL COMMENT '属性名',
    search_type  tinyint      NULL COMMENT '是否需要检索[0-不需要，1-需要]',
    value_type   tinyint      NULL COMMENT '值类型[0-为单个值，1-可以选择多个值]',
    icon         varchar(255) NULL COMMENT '属性图标',
    value_select char(255)    NULL COMMENT '可选值列表[用逗号分隔]',
    attr_type    tinyint      NULL COMMENT '属性类型[0-销售属性，1-基本属性',
    enable       bigint       NULL COMMENT '启用状态[0 - 禁用，1 - 启用]',
    catelog_id   bigint       NULL COMMENT '所属分类',
    show_desc    tinyint      NULL COMMENT '快速展示【是否展示在介绍上；0-否 1-是】，在sku中仍然可以调整'
) COMMENT '商品属性';
```

可以看到属性表中都是一些属性, 什么叫做属性呢, 我们来看一组数据

```
7,入网型号,0,0,xxx,A2217;C3J;以官网信息为准,1,1,225,0
8,上市年份,0,0,xxx,2018;2019,1,1,225,0
9,颜色,0,0,xxx,黑色;白色;蓝色,0,1,225,0
10,内存,0,0,xxx,4GB;6GB;8GB;12GB,0,1,225,0
11,机身颜色,0,0,xxx,黑色;白色,1,1,225,1
12,版本,0,0,xxx,"",0,1,225,0
13,机身长度（mm）,0,0,xx,158.3;135.9,1,1,225,0
14,机身材质工艺,0,1,xxx,以官网信息为准;陶瓷;玻璃,1,1,225,0
15,CPU品牌,1,0,xxx,高通(Qualcomm);海思（Hisilicon）;以官网信息为准,1,1,225,1
16,CPU型号,1,0,xxx,骁龙665;骁龙845;骁龙855;骁龙730;HUAWEI Kirin 980;HUAWEI Kirin 970,1,1,225,0
```

### 🌸 属性分组表(pms_attr_group)

```sql
CREATE TABLE pms_attr_group
(
    attr_group_id   bigint AUTO_INCREMENT COMMENT '分组id'
        PRIMARY KEY,
    attr_group_name char(20)     NULL COMMENT '组名',
    sort            int          NULL COMMENT '排序',
    descript        varchar(255) NULL COMMENT '描述',
    icon            varchar(255) NULL COMMENT '组图标',
    catelog_id      bigint       NULL COMMENT '所属分类id'
) COMMENT '属性分组';
```

用来描述属性在哪一组中, 我们来看一组数据

```
1,主体,0,主体,dd,225
2,基本信息,0,基本信息,xx,225
4,屏幕,0,屏幕,xx,233
7,主芯片,0,主芯片,xx,225
```

### 🌸 属性&属性分组关联表(pms_attr_attrgroup_relation)

```sql
CREATE TABLE pms_attr_attrgroup_relation
(
    id            bigint AUTO_INCREMENT COMMENT 'id'
        PRIMARY KEY,
    attr_id       bigint NULL COMMENT '属性id',
    attr_group_id bigint NULL COMMENT '属性分组id',
    attr_sort     int    NULL COMMENT '属性组内排序'
) COMMENT '属性&属性分组关联';
```

用来描述属性在哪一个分组中

```
23,7,1,
24,8,1,
26,11,2,
27,13,2,
28,14,2,
29,15,7,
30,16,7,
```

### 🌸 spu属性值表(pms_product_attr_value)

```sql
CREATE TABLE pms_product_attr_value
(
    id         bigint AUTO_INCREMENT COMMENT 'id'
        PRIMARY KEY,
    spu_id     bigint       NULL COMMENT '商品id',
    attr_id    bigint       NULL COMMENT '属性id',
    attr_name  varchar(200) NULL COMMENT '属性名',
    attr_value varchar(200) NULL COMMENT '属性值',
    attr_sort  int          NULL COMMENT '顺序',
    quick_show tinyint      NULL COMMENT '快速展示【是否展示在介绍上；0-否 1-是】'
) COMMENT 'spu属性值';
```

描述`spu`的属性值

```
55,13,7,入网型号,A2217,,0
56,13,8,上市年份,2018,,0
57,13,13,机身长度（mm）,158.3,,0
58,13,14,机身材质工艺,以官网信息为准,,0
59,13,15,CPU品牌,以官网信息为准,,1
60,13,16,CPU型号,A13仿生,,1
61,11,7,入网型号,LIO-AL00,,1
62,11,8,上市年份,2019,,0
63,11,11,机身颜色,黑色,,1
64,11,13,机身长度（mm）,158.3,,1
65,11,14,机身材质工艺,玻璃;陶瓷,,0
66,11,15,CPU品牌,海思（Hisilicon）,,1
67,11,16,CPU型号,HUAWEI Kirin 970,,1
```

未完待续

## 🌲 表关系

因为这个地方实在太多了, 我也很难听懂, 所以之后如果听懂了就把这些表补上

## 🌲 属性分组前端

我们现在就来写属性分组页面吧, 虽然前面听的不太明白, 但我们可以通过页面来反推逻辑, 最后再总结

### 🌸 重新导入菜单

我们可以看到老师这次又不手写分类了, 直接是用SQL导入了菜单, 那有人会问我们之前删除过的菜单要怎么回来呢, 这很简单从新导入一下就行了

我们删除数据库, 重新创建`gulimall_admin`, 然后直接导入`gulimall_admin.sql`

![](images3/Pasted%20image%2020231019173808.png)

导入后我们可以看到菜单都恢复过来了

### 🌸 新建属性分组页面

我们可以看到属性分组的链接是这样的

http://localhost:5173/product-attrgroup

从这里我们就能想到要创建一个`vue`, 这个文件叫做`attrgroup`

![](images3/Pasted%20image%2020231019174511.png)

然后给它先布个局

```vue
<template>
    <div>
        <el-row :gutter="20">
            <el-col :span="6">
                菜单
            </el-col>
            
	        <el-col :span="18">
                表格
            </el-col>
        </el-row>
    </div>
</template>
```

### 🌸 抽取三级分类

因为`三级分类`后续都会用到, 所以我们把它封装成`组件`, 这样就可以方便的进行引入达到复用的效果, 创建一个`common/categoryTree.vue`

然后我们把`category`中的代码直接拷贝进去, 修修补补之后是下面的代码了

```vue
<template>
    <div>
        <el-tree :data="menus" :props="defaultProps" ref="tree" node-key="catId">
            <span class="custom-tree-node" slot-scope="{ node, data }">
                <span>{{ node.label }}</span>
            </span>
        </el-tree>
    </div>
</template>
  
<script>
import axios from 'axios'
import { api } from '@/utils/apimap.js'
export default {
    data() {
        return {
            menus: [],
            defaultProps: {
                children: "children",
                label: "name"
            },
        };
    },
    methods: {
        requestMenus() {
            axios.get(api.api_product_category_listCategoryTree).then((res) => {
                if (res.data.code == 0) {
                    this.menus = res.data.datas;
                }
            })
        }
    },
    created() {
        this.requestMenus();
    }
};
</script>
```

然后用三级分类代替菜单

```vue
<el-col :span="6">
	<CategoryTree></CategoryTree>
</el-col>

<script>

import CategoryTree from "@/views/common/category-tree.vue"

export default {
    components: {
        CategoryTree
    },
```

### 🌸 完善表格

我们从自动生成的代码中把所有的东西直接对号入座拷贝过来, 其中`<template>`中的内容替换`表格`两个字, 变量什么的都配置进来, 否则报错

![](images/Pasted%20image%2020231020130837.png)

然后我们会看到右边一直转圈, 查看控制台我们发现如下错误, 所以是网络请求不通导致的一直转圈

```
GET http://localhost:8080/renren-fast/product/attrgroup/list?t=1697778375360&page=1&limit=10&key= 404 (Not Found)
```

一看就是接口没通, 我们把它替换成网关的就可以了, 向`apimap.js`中增加接口

```js
const host = import.meta.env.VITE_GULIMALL_HOST;

const api = {
    api_product_category_listCategoryTree: host + "/product/category/listCategoryTree",
    api_product_category_delete: host + "/product/category/delete",
    api_product_category_save: host + "/product/category/save",
    api_product_category_update: host + "/product/category/update",

    api_product_brand_list: host + "/product/brand/list",
    api_product_brand_delete: host + "/product/brand/delete",
    api_product_brand_info: host + "/product/brand/info",
    api_product_brand_save: host + "/product/brand/save",
    api_product_brand_update: host + "/product/brand/update",

    api_product_attrgroup_list: host + "/product/attrgroup/list",
    api_product_attrgroup_delete: host + "/product/attrgroup/delete",
    api_product_attrgroup_info: host + "/product/attrgroup/info",
    api_product_attrgroup_save: host + "/product/attrgroup/save",
    api_product_attrgroup_update: host + "/product/attrgroup/update",

    api_thirdpart_oss_policy: host + "/thirdparty/oss/getPolicy",
}

export {
    api
}
```

然后我们改写接口, 改写过程我不多说了, 然后可以看到不会一直转圈了

![](images/Pasted%20image%2020231020131601.png)

### 🌸 获取分类的属性分组功能(72集)

这一块老师讲的不好, 我非常迷惑, 所以决定自己做了

这一块说白了就是个筛选, 我们想实现点击左侧的三级分类后获取到`categoryId`带到右侧的列表做一个筛选, 值得注意的是我们能够看见页面上面也有一个搜索, 所以我们可以看出这个获取属性分组的接口需要很多的参数, 我们能想到的就是`categoryId`和`key(搜索的值)`, 我们首先需要一个接口, 我们先去找控制器`AttrGroupController.java`, 对应的功能应该写在对应的控制器中

```

```



未完待续...

















































