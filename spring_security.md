## session
第一次登录之后，服务器保存一个session，返回给客户端session ID, 客户端存在cookie中。之后的请求，客户端带着cookie中的session ID重启对话。

ref : https://blog.csdn.net/bsegebr/article/details/125348334
- session存贮在服务器上，增加了服务器的压力
- cookie可以被伪造
- 难以使用微服务架构，不同的服务器之间难以共享session

## token
第一次登录之后，服务器根据算法生成一个token，返回给客户端。客户端存在Local stroage中。之后的请求，请求头上带着token，服务器解析token，验证是哪一个用户。

- token是计算出来的，不存在服务器上
- 多个服务器之间只要共享 加密解密token算法，就可以使用同一个token
- token可以被截获

## JWT （json web token）
由3部分组成，中间用.连接，第一部分为header，保存了令牌类型和加密算法；第二部分为payload，包含了用户的一些信息；第三部分为签名，是根据前两部分的一部分，加密计算而来。
