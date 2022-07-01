### intellij 中无法下载maven包
尝试切换下载服务器镜像
- 在$user_home$/.m2/settings.xml中添加
```xml
<mirrors>
   <mirror>
    <!--This sends everything else to /public -->
    <id>nexus-aliyun</id>
    <mirrorOf>*</mirrorOf>
    <name>Nexus aliyun</name>
    <url>http://maven.aliyun.com/nexus/content/groups/public</url>
    </mirror>
</mirrors>
```
- 在pom.xml中添加
```xml
<repositories>
        <repository>
            <id>nexus-aliyun</id>
            <name>nexus-aliyun</name>
            <url>http://maven.aliyun.com/nexus/content/groups/public/</url>
            <releases>
                <enabled>true</enabled>
            </releases>
            <snapshots>
                <enabled>false</enabled>
            </snapshots>
        </repository>
    </repositories>
```
