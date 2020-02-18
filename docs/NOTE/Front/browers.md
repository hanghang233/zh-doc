---

hideFooter: true

---
# 浏览器与前端性能-- #

::: tip 契子
- webpack
:::

## &01.能不能说一说浏览器缓存 ##
浏览器缓存分为：强缓存、协商缓存、缓存位置

- 浏览器的缓存又分为两种情况，一种是需要发送HTTP请求，一种是不需要发送请求

强缓存：这个阶段不需要发送HTTP缓存。
    * Expires：过期时间，存在于服务器返回的响应头中，告诉浏览器在这个时间内可以直接从缓存里面取数据，无需请求
    * Cache-Control：采用过期时间长来控制缓存

协商缓存：强缓存失效之后，浏览器在http中携带相应的缓存tag，由服务器根据这个tag来判断是否使用缓存
    * Last-Modified：最后修改时间
    * ETag：服务器根据当前文件的内容，给文件生成的唯一标识，只要里面的内容有改动，ETag就会变

总结：
1、首先通过cache-control验证强缓存是否使用

2、如果强缓存可用，可以直接使用；否则进入协商缓存

3、判断http协议请求头中的If-Modified-Since和If-None-Match，检查资源是否更新；1）如果更新，返回资源和200状态码；2）否则，返回304，告诉资源直接从浏览器缓存中取

## &02.能不能说一说浏览器的本地存储？各自优劣如何 ##
浏览器的本地存储主要分为：cookie、webStorage和IndexedDB，其中webStorage又可以分为localStorage和sessionStorage
- cookie--用来做状态存储
缺点：1、容量缺陷：cookie的存储量只有4kb，只能用来存储少量的信息

2、性能缺陷：相同域名及其子域名的http请求，都会携带所有的cookie

3、安全缺陷：以文本形式存储在客户端，容易被非法人员截获

- sessionStorage和localStorage
sessionStorage指会话级别的存储，localStorage指持久化存储；

sessionStorage应用场景：表单维护、存储浏览记录

- IndexedDB：运行在浏览器上面的非关系型数据库

## &03.说一说从输入URL到页面呈现出什么--网络篇 ##
1、构建请求

2、查找强缓存：如果是强缓存，直接从缓存中读取

3、DNS解析：得到具体的IP地址。浏览器也提供了DNS缓存服务，如果一个域名已经解析过了，会把解析的结果直接缓存下来，下次处理直接走缓存。

4、建立TCP连接

5、发送HTTP请求

6、网络响应

## &04.forEach有return效果吗 ##
forEach没有return效果，如果需要终止循环有两种解决方案：

1.利用try-catch

2.官方推荐：用every和some替代forEach函数。every在碰到return false的时候，中止循环。some在碰到return true的时候，中止循环
