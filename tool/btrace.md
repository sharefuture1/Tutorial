# FAQ

1. btrace DEBUG: adding to boot classpath failed!

    常见是由于系统安装的btrace版本和代码中引入的jar的版本不匹配，建议加载jar（/btrace/../build/）

2. Port 2020 unavailable.

    2020端口不可用，那就换一个：btrace pid -p port xxx.java

3. 当参数为泛型的时候，使用Object替换
4. 在Windows中，使用cmd执行btrace命令，git bash执行不了

