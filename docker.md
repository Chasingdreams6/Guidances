
## 起源
- docker可以打包好一整套环境，使开发、测试、运维人员不需要从头配一套环境
-  vs 虚拟机，本身虚拟机的os要占用很多磁盘空间，如果开很多虚拟机，本身的os就占用了大量的硬盘与内存
-  容器技术只隔离应用程序的运行时环境但容器之间可以共享同一个操作系统，运行时环境指的是各种依赖和配置
-  程序的行为只依赖于container, 不依赖于container所依赖的os，build once, run everywhere
-  容器部署速度很快，可以快速地部署、下线

## 核心概念
- Image 镜像，可以认为是可执行程序
- container 容器，可以理解成运行后的进程（Image运行后就是container; docker run）
- dockerfile 指定程序依赖的配置，环境，源码等
- docker  docker服务器，可以根据dockerfile将程序打包成image
- docker pull， 使用此命令从他人的repositry中clone好已经写好的image使用
