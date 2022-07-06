## 下载与安装
下载nacos server.zip包，解压并运行（以单例模式）
```cmd
./startup.cmd -m standalone
```

## 问题与修复
1. 各种版本不兼容的问题，在maven repository中选择合适的版本（不一定是最新的版本）
2. feign不支持Page<T>，重写一个RestResponsePage<T> https://stackoverflow.com/questions/52490399/spring-boot-page-deserialization-pageimpl-no-constructor
3. 
  基本实现
  https://blog.csdn.net/CSDN_LAFF/article/details/109840928 https://blog.csdn.net/CSDN_LAFF/article/details/109824809
