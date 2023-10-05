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

#### 🌵 新建商品系统目录

点击新增

![](images/Pasted%20image%2020231005144247.png)

然后我们选择`目录`, 取名叫`商品系统`

![](images/Pasted%20image%2020231005144227.png)

然后点确定保存, 然后刷新页面

![](images/Pasted%20image%2020231005144425.png)

我们可以看到商品系统就创建出来了 















