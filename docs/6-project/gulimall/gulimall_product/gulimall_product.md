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
      console.log(data);
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

#### 🌵 请求数据

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
      console.log(data);
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
      console.log(data);
    },
    requestMenus() {
      axios.get("http://localhost:8081/product/category/listCategoryTree").then((res) => {
          this.menus = res.data.data;
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

我们可以看到分类菜单都能显示了

#### 🌵 回过头配置网关

请求数据已经可以正常显示了, 那么我们现在就来学习视频`46,47,48集`中配置的网关吧, 网关我们之前已经学过了, 就是用来转发请求的, 里面可以配置一些`跨域, 认证, 授权`之类的东西, 可以做到所有使用网关转发的微服务这种前置行为统一, 那我们接下来就开始配置吧

未完待续






















