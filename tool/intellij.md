# intellij

## 目录

* [idea moudle没有蓝色的小方块](intellij.md#ideamoudle没有蓝色的小方块)

## 本着学习自用的态度

* 参考文章：[https://www.cnblogs.com/lubians/articles/12179654.html](https://www.cnblogs.com/lubians/articles/12179654.html)
* 激活工具：链接：[https://pan.baidu.com/s/1gzjNQe2bJ-wQeRTT\_mdFGg](https://pan.baidu.com/s/1gzjNQe2bJ-wQeRTT_mdFGg) 提取码：gf3j

## 常用设置

### 插件

* 插件 Lombook， Easy Code\([https://blog.csdn.net/qq\_38225558/article/details/84479653](https://blog.csdn.net/qq_38225558/article/details/84479653)\)
* RestfulToolkit 将url收集显示，点击url可转到代码
* Grep Console插件能让你的Console丰富多彩，并且还能够过滤控制台输出\(过滤的时候先选中要过滤的关键字，然后右键选择Grep\)
* Rainbow Brackets 让花括号用不同的颜色标记，别于代码阅读区分
* SequenceDiagram:一键生成时序图.一般用它来生成简单的方法时序图，方便我们阅读代码，特别是在代码的调用层级比较多的时候。

  使用方法很简单，选中方法名（注意不要选类名），然后点击鼠标右键，选择 Sequence Diagram 选项即可！

* EasyCode: Easycode 可以直接对数据的表生成entity、controller、service、dao、mapper无需任何编码，简单而强大。

### 配置

1. 开始没开启显示空格的选项（Setting-&gt;Editor-&gt;Appearance-&gt;show whitespaces）

![](../.gitbook/assets/idea-tab-whitespace.PNG)

1. properties文件显示中文。所有的都改成UTF-8

![](../.gitbook/assets/idea-encode-utf8.png)

1. 缩放字体

![](../.gitbook/assets/idea-font-size.png)

### 社区版IDEA如何添加tomcat服务器

1. 在pom.xml中plugins节点下添加tomcat-maven-plugin，目前比较稳定的是tomcat7-maven-plugin, tomcat8-maven-plugin支持不是很好

   ```markup
   <plugins>
    <plugin>
        <groupId>org.apache.tomcat.maven</groupId>
        <artifactId>tomcat7-maven-plugin</artifactId>
        <version>2.2</version>
        <configuration>
        <port>8080</port>
        <path>/</path>
        <uriEncoding>UTF-8</uriEncoding>
        <server>tomcat7</server>
        </configuration>
    </plugin>
   </plugins>
   ```

2. 安装smart tomcat插件 \`\`\`
3. Go to File -&gt; Settings
4. Click Plugins, search smart tomcat
5. Install and restart IDEA
6. Edit Configurations\(在IDEA右上边，Run左边的下拉框\)
7. Click +, select Smart Tomcat
8. Config Tomcat Server, Deployment, Context Path

   \`\`\`

## 快捷键

* idea常用快捷键设置（改为eclipse相似）: File-&gt; Settings -&gt; search eclipse -&gt; Keymap\(change to Eclipse\)

  ```text
  ctrl + alt +B 查看接口的实现类
  ctrl + e 查看Recent Files
  Alt + Ins 实现接口的方法，toString(), 构造函数，重写方法
  double shift调出Everywhere Search
  ```

  **FAQ**

  **requested without authorization,  you can copy URL and open it in browser to trust it**

  ```text
  Settings > Build, Execution.. > Debugger 勾选Allow unsigned requests
  ```

  **IDEA 无法找到jdk，只能找到jre解决方式**

  ```text
  在IDEA的菜单栏中选中File, 选中Project Structure, 然后，弹出来的对话框左边有一个SDK单击它, 在中间的一栏中有个+号单击它，一单击会出现Add New SDK然后选中第一个JDK；
  ```

  **IntelliJ IDEA：无法创建Java Class文件**

  ```text
  右键新建不了java class文件
  File -> Project Structure -> Project Settings -> Modules
  选中 mian folder，然后邮件设置为Sources
  保存
  ```

### idea moudle没有蓝色的小方块

* [https://blog.csdn.net/wt\_better/article/details/86380826](https://blog.csdn.net/wt_better/article/details/86380826)
* 右边Maven -&gt; Generate Source and Update Project
* 更新.idea中的modules.xml

