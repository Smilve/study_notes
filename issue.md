# 日常问题

## Java

### mybatis的order by语句

在order by 子句中，如果有传入得变量需要使用 `${}`才能读取数据，因为`#{}`会预处理传入的字符串，会给字符串加上双引号，使用这个放在order by后面会导致排序失效。

### ThreadLocal

[弱引用](https://www.jianshu.com/p/964fbc30151a)

[强引用、软引用、弱引用、虚引用](http://www.cnblogs.com/gudi/p/6403953.html)

[ThreadLocal](https://www.jianshu.com/p/3c5d7f09dfbd)

### logger

logger.debug()源码就是logger.isDebugEnabled()进行判断，如果日志输出级别高于debug，使用下面的代码会提高效率；因为直接debug会执行getTotalMoney()这个方法，会消耗时间，然后再判断发现不会打印，相当于白耗时间

```java
if(logger.isDebugEnabled()){
    logger.debug("The money is " + getTotalMoney());
}
```

### Java中i++是否是原子操作

不是；有取值，++，再赋值三个操作  (不是原子操作会被其他线程打断)

i=0;两个线程分别对i进行++100次，值得范围 (2~200)

最坏的情况：首先b取值0的时候，被a线程打断加到99，然后被b线程打断++赋值i=1，然后被a线程打断取值1，然后被b线程打断直接加到100，然后a线程++赋值i=2；结束

<img src="images/issue.assets/image-20220126175310186.png" alt="image-20220126175310186" style="zoom: 80%;" />

## 前端





## 数据库





## 工具

maven、git、idea等



### maven 导入build下的编译包导入失败

将依赖放到dependency下进行下载，下载成功就能导入了

```xml
 <dependency>
     <groupId>org.apache.maven.plugins</groupId>
     <artifactId>maven-antrun-plugin</artifactId>
     <version>1.8</version>
</dependency>
```



## Linux

### 基础命令

```bash
# sudo su 切换成root的存储权限 不会读取环境配置 如java命令失效
su - root 

# 使用rz上次文件乱码时使用
rz -be
```



### docker安装minio

```bash
docker run -d -p 9000:9000 -p 9001:9001 --name minio1 --restart=always -e "MINIO_ROOT_USER=root" -e "MINIO_ROOT_PASSWORD=lvbo1104" -v /home/minio/data:/data -v /home/minio/config:/root/.minio minio/minio server /data --console-address ":9001"
```



> 注意

linux在关机或者重启前先运行`sync`同步命令，将内存的数据同步到磁盘中。

还有docker使用`-v`挂载目录时，如果目录里面有文件夹，容器会启动失败，需要先把容器内的文件拷贝出来，再新运行一个容器挂载目录

nginx在转发的时候，会丢掉host信息，如果需使用gateway中的host断言，就需要在location中加上host

```bash
location / {
	proxy_set_header Host $host;   # 因为nginx转发请求的时候会丢掉host，在gateway就匹配不到了，在这需要加上
    proxy_pass http://gulimall;
}
    
```

