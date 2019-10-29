# 使用Spring Social开发第三方登录

从本章开始进入了oauth协议；这里的第三方登录也是oauth协议；

## OAuth协议简介

* OAuth要解决的问题
* OAuth协议中的各个角色
* OAuth运行流程

## OAuth要解决的问题

![](/assets/image/imooc/spring_secunity/snipaste_20180805_233924.png)

考虑这样一个场景：假如开发一个APP----微信翻译助手，需要用户存储在微信上的数据；

传统的做法是：拿到用户的用户名密码
问题：
* 应用可以访问用户在微信上的所有数据；（不能做到读授权一部分功能权限）
* 只能修改密码才能收回授权
* 密码泄露可能性提高

OAuth要使用Token令牌来解决以上的问题
* 令牌绑定权限
* 令牌有过期时间
* 不需要用户的原始密码

## OAuth协议中的各个角色
![OAuth协议中的各种角色](https://upload-images.jianshu.io/upload_images/15200008-2ac1edbeba28adbd.png?imageMogr2/auto-orient/strip|imageView2/2/format/webp)

* 服务提供商 Provider  
  谁提供这个令牌，谁就是这个提供商;微信提供令牌，所以微信就是提供商
* 资源所有者 Resource Owner  
  资源是指什么？资源就是用户需要翻译的聊天数据。这些聊天数据所有者是用户，而不是微信，用户只是把这些资源放在了微信上。那么这里的资源所有者就是用户
* 第三方应用 Client  
  第三方应用也就是我们例子中的翻译助手，它的目的就是将微信用户转化为第三方的用户。然后向微信提供服务。
* 认证服务器 Authorization Server  
  认证用户的身份并且派发令牌
* 资源服务器 Resource Server  
  这个角色有两个作用：
  - 保存用户的资源，即将用户的聊天数据保存起来。
  - 验证令牌。最终我们第三方应用发送请求的时候是发送到资源服务器上边的，发请求的时候会带着认证服务器发给我的Token，由资源服务器去验证这个Token，如果验证过了就会把用户的聊天数据给第三方应用。


## OAuth运行流程
![](/assets/image/imooc/spring_secunity/snipaste_20180805_233937.png)

![](https://upload-images.jianshu.io/upload_images/15200008-f8fe90183ced6a91.png?imageMogr2/auto-orient/strip|imageView2/2/format/webp)

- 1. 首先用户要访问第三方应用。
- 2. 然后第三方应用就会请求用户去授权，第三方应用必须要让用户去访问聊天数据才能对其进行服务。
- 3. 如果用户同意授权，那么第三方应用就会去访问服务提供商的认证服务器，告诉认证服务器用户同意我访问微信聊天数据了，你给我一个申请的令牌。
- 4. 其次认证服务器会去验证第三方应用“说的是不是实话”，即用户是不是真的同意授权了。如果验证通过了就会给第三方应用发送一个令牌。
- 5. 第三方应用拿到了这个令牌之后，它就可以使用这个令牌去资源服务器中申请访问资源。
- 6. 资源服务器根据第三方应用给出的这个令牌，确认令牌无误之后，它会同意把用户的聊天数据开放给第三方应用。

在这几个步骤中，第二步（用户同意授权）这个是关键，因为有了这个授权之后，Client才能去进行下面的一系列操作。

OAuth提供了4中授权方式

* 授权码模式 Authorization code
* 简化模式 implicit
* 密码模式 resource owner Password credentials
* 客户端模式 client credentials

### 授权码模式流程

![](/assets/image/imooc/spring_secunity/snipaste_20180805_234652.png)

该模式与其他三种模式不同，最重要的区别就是：同意授权这个动作，是在认证服务器上完成的，而其他的三种都是在第三方应用上完成的。

该模式是4中模式中最严格最完整的一种协议

### 简化模式
在授权后直接带回令牌
