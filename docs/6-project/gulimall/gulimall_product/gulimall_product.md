# 🍎 product模块

终于到了我们后台模块的编写环节了, 我们从第45节课开始

## 🌲 三级分类

### 🌸 介绍

我们来看一张京东的主页, 可以看到上面的商品有三级分类

![](images/Pasted%20image%2020231004134415.jpg)

### 🌸 导入数据

视频中让我们把数据导入到表, 我们就不用导入了, 因为使用的数据库脚本里面包含了数据了, 我们可以打开表看一看

![](images/Pasted%20image%2020231004170006.png)

在`pms_category`表中我们可以看到已经有相关的分类数据了

![](images/Pasted%20image%2020231004170124.png)

鼠标悬停在上面可以看到对应的类型和注释

### 🌸 实现接口

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
        return entity.getParentCid() == menu1.getCatId();  
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

代码是经过我修改的, 我使用了`Optional`来解决空指针异常问题, 因为我发现有一个sort为空, `SELECT * FROM pms_category WHERE cat_id=1431;`

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

白猫有话要说, 虽然有经验的同学都能写出来, 但是这对于新手来说可能是「毁灭性打击」, 所以白猫建议如果实在跟不下来直接把代码粘贴到你的项目中, 先把程序运行出来, 然后再去分析结构反推代码, 这样更有利于学习

## 🌲 菜单管理

### 🌸 删库

实现三级分类后我们就可以调用接口试试了, 视频让我们去「菜单管理」

![](images/Pasted%20image%2020231005130923.png)

然后我们可以看到比视频中多了这么多东西, 我认为这些都是可以删除的, 然后自己再跟着视频慢慢创建

我们当然可以点击右侧的删除按钮一个一个的删除, 但是这会很麻烦, 所以我们可以在`datagrip`中直接写SQL删除, 首先我们确定数据库为`gulimall-admin -> sys_menu`, 然后我们来查询一下

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

![](images/Pasted%20image%2020231005143337.png)

删除干净后我们就可以跟随视频去做了

### 🌸 跑路

开个玩笑当然不是跑路, 我们继续跟着视频做哈

#### 🌵 新建「商品系统」目录

点击新增

![](images/Pasted%20image%2020231005144247.png)

然后我们选择`目录`, 取名叫`商品系统`

![](images/Pasted%20image%2020231005144227.png)

然后点确定保存, 然后刷新页面

![](images/Pasted%20image%2020231005144425.png)

我们可以看到商品系统就创建出来了 

#### 🌵 新建「分类维护」菜单

然后我们点击新建

![](images/Pasted%20image%2020231005175245.png)

给商品系统下面新建一个分类维护, 用于维护分类

#### 🌵 完善「分类维护」页面

我们首先点击菜单中的分类维护, 发现它跳转的页面是

http://localhost:8001/#/product-category

那我们就可以得出在vue项目中需要创建一个页面和它对应

![](images/Pasted%20image%2020231005195004.png)

然后写一段测试代码

```vue
<template lang="">

<h1>hello world</h1>

</template>

<style lang=""></style>
```

可以看到效果是这样的, 证明我们设置的页面生效了

![](images/Pasted%20image%2020231005194917.png)

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

![](images/Pasted%20image%2020231006123443.png)

可以看到树形结构已经展示出来了

#### 🌵 配置网关

页面结构出来后老师并没有带我们去写网络请求, 而是遇到跨域问题就直接去配置网关, 而且中途灌输了一大堆的复杂配置比如分布式配置中心, 新手容易听的云里雾里的, 所以我这里化繁为简, 先把网络请求写完, 回过头再去配置网关以及网关跨域

#### 🌵 展示三级分类

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

![](images/Pasted%20image%2020231006141456.png)

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

![](images/Pasted%20image%2020231006161741.png)

我们可以看到分类菜单都能显示了, 那么怎么改变分类顺序呢, 也很简单, 我们只需要改`sort`就可以了, 因为我们是从小到大排序, 所以把`sort`改小菜单自然就上来了

比如我们想把手机放在前面, 那就在数据库把手机的`sort`改成`-1`

![](images/Pasted%20image%2020231007134209.png)

我们再刷新页面发现手机到第一个了

![](images/Pasted%20image%2020231007134235.png)

这就是排序原理了

#### 🌵 添加删除分类

想要添加删除分类, 我们的UI上必须要有添加和删除按钮, 因此我们需要查询`element`文档, 找到添加按钮的方法, 这个栏目讲删除, 我们从添加按钮开始

##### 🐔 添加按钮

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

![](images/Pasted%20image%2020231007154353.png)

然后我们可以点击`Append`和`Delete`来打印当中的元素, 其中`data`就是我们的数据, `node`是节点

##### 🐔 阻止事件传递

我们会发现一个问题, 就是点击`Append`和`Delete`的时候列表会打开和关闭, 这很不协调, 视频中是让设置一个属性`:expand-on-click-node="false"`, 但是我觉得这样点击范围就变小了, 也不方便, 所以我使用阻止事件传递到父视图, vue替我们封装好了, 我们使用`@click.stop`就可以阻止事件传递到父视图, 设置后点击非按钮地方都可以打开菜单了

```vue
<el-button type="text" size="mini" @click.stop="append(data)">
  Append
</el-button>
<el-button type="text" size="mini" @click.stop="remove(node, data)">
  Delete
</el-button>
```

##### 🐔 隐藏逻辑

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

##### 🐔 接口跨域配置

之后我们就可以调用网络请求来删除数据了, 我们去找一下, 在`CategoryController`中有一个`delete`方法, 我们用这个方法应该是可以删除的, 我们把它前面也加上`@CrossOrigin("*")`

```java
@CrossOrigin("*")
@RequestMapping("/delete")
public R delete(@RequestBody Long[] catIds){
	categoryService.removeByIds(Arrays.asList(catIds));

	return R.ok();
}
```

##### 🐔 接口测试工具

这个时候老师让我们下载一个`postman`, 我不是很想使用这个, 所以我使用`REST Client`, 首先在`vscode`中下载插件

![](images/Pasted%20image%2020231007175329.png)

然后创建一个`test.http`文件, 写入下面的内容

```
### 删除
POST http://127.0.0.1:8081/product/category/delete
Content-Type: application/json

[1]
```

##### 🐔 备份数据

然后我还备份一下`category`的数据到`csv`

![](images/Pasted%20image%2020231007175455.png)

##### 🐔 测试接口

然后发送请求, 发现成功了, 对应的数据也没了

```
{
  "msg": "success",
  "code": 0
}
```

说明`mybatis-plus`的`delete`接口做的是物理删除, 到那时我们开发中一般不使用这么暴力的删除方式, 因为数据删除后不好恢复, 所以我们要使用逻辑删除, 那么逻辑删除是怎么实现的呢? 我们一起来看看吧

##### 🐔 配置逻辑删除

所谓逻辑删除其实就是一个标识位, 我们去找`pms_category`表中可以看到一个`show_status`

![](images/Pasted%20image%2020231007162906.png)

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

![](images/Pasted%20image%2020231007181151.png)

##### 🐔 配置日志打印级别

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

##### 🐔 实现删除功能

接口调通了, 我们接下来就可以在前端实现了

```js
remove(node, data) {
  axios.post("http://127.0.0.1:8081/product/category/delete", [data.catId]).then((res) => {
	console.log(res.data);
	if (res.data.code == 0) {
	  console.log("删除成功");
	  this.requestMenus();
	}
  })
}
```

首先我们调用`delete`接口, 要注意跨域注解, 否则访问不通, 当删除成功后我们可以调用`requestMenus`进行刷新, 这样列表就可以刷新了

##### 🐔 发现问题

但是我们会发现一些问题, 在删除之前没有弹框提示, 删除之后没有message提示删除成功, 还有删除后菜单合起来了, 并没有展开

##### 🐔 添加提示框

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

##### 🐔 添加消息框

https://element.eleme.cn/#/zh-CN/component/message

```js
this.$message({
  message: '恭喜你，这是一条成功消息',
  type: 'success'
});
```

##### 🐔 让菜单刷新后打开

我们可以换个思路, 当我删除菜单成功后我可以直接删除当前节点, 不刷新数据菜单就不会关上了, 删除节点我们使用`tree`自带的`remove`方法, 我们可以使用`ref`来引用一个组件, 然后调用组件本身的方法

##### 🐔 实现删除分类

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
      axios.post("http://127.0.0.1:8081/product/category/delete", [data.catId]).then((res) => {
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
            type: 'success'
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

##### 🐔 实现默认展开

我们可以看到视频中没有去直接删除节点, 而是用了另外一个方法让tree默认展开, 我们在文档中可以查看到一个叫`default-expanded-keys`的属性, 值是一个数组, 那么这个数组里装什么呢, 装的就是你的`node-key`把哪个`node`的`node-key`装进去就会默认打开哪个, 所以这个时候视频前面设置的`node-key`就派上用场了, 我当时是没设置的, 因为他当时没说明这个值有什么用, 我们现在就把他配置上

```vue
<el-tree :data="menus" :props="defaultProps" ref="tree" node-key="catId" :default-expanded-keys="[4]">
```

我们可以看到视频中配置的`node-key`并不是动态的, 而是静态的, 我看当时配置这个`key`的时候弹幕上有人问不应该使用`:node-key`来配置动态的`key`吗, 这里统一解答一下, 这么配置是没错的, `tree`会根据这个字段去数据源中找到`catId`然后取出这个值作为我们的`node-key`

接下来我们配置成`4`来默认打开`catId`为`4`的菜单, 我们看一看效果吧

![](images/Pasted%20image%2020231008153221.png)

我们可以看到数码打开了, 我们现在就实现了默认打开栏目了, 但是我不推荐用这种方式来做删除后默认打开的功能, 我觉得我的删除节点的方式就很好

#### 🌵 添加分类

删除分类已经做完了, 那我们来看看添加分类, 这个就稍微复杂一点了, 因为我们要写一个添加页面

##### 🐔 实现dialog





## 🌲 回过头配置网关

请求数据已经可以正常显示了, 那么我们现在就来学习视频`46,47,48集`中配置的网关吧, 网关我们之前已经学过了, 就是用来转发请求的, 里面可以配置一些`跨域, 认证, 授权`之类的东西, 可以做到所有使用网关转发的微服务这种前置行为统一, 那我们接下来就开始配置吧

未完待续






















