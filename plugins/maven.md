# maven

* 本地父子项目继承： 其实都是一个坐标，不过本地父项目你需要install到本地Maven仓库，不然坐标无法检索。安装到本地仓库即可让子项目继承，父项目有的依赖子项目都会有了。执行mvn clean install，具体示例参考springcloud-eureka。

