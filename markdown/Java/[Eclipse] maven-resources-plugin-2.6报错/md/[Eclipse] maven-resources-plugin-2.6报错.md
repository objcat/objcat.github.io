
错误如图所示。
![maven.png](https://upload-images.jianshu.io/upload_images/2194379-8b4b464651276539.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


解决方案 修改配置文件直接替换maven源为阿里云路径

```
G:\apache-maven-3.5.3\conf\settings.xml
```

编辑 在里面找到<mirrors></mirrors>标签 插入图中配置

![image.png](https://upload-images.jianshu.io/upload_images/2194379-f00f78ab0cb9d0f0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


```
<mirror>  
	<id>alimaven</id>  
	<mirrorOf>central</mirrorOf>    
	<name>aliyun maven</name>  
	<url>http://maven.aliyun.com/nexus/content/groups/public/</url>        
</mirror> 
```


然后在eclipse中更新maven

![image.png](https://upload-images.jianshu.io/upload_images/2194379-a967d11a4c11862a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![image.png](https://upload-images.jianshu.io/upload_images/2194379-9e750f1296b4a654.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


#finally enjoy it
#by objcat 2018.5.27










