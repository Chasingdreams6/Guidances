## session
第一次登录之后，服务器保存一个session，返回给客户端session ID, 客户端存在cookie中。之后的请求，客户端带着cookie中的session ID重启对话。

- session存贮在服务器上，增加了服务器的压力
- cookie可以被伪造
- 难以使用微服务架构，不同的服务器之间难以共享session

