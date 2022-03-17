# Shiro安全控制

### 1、介绍

Apache Shiro是Java的一个安全框架。

Shiro可以帮助我们完成：认证、授权、加密、会话管理、与Web集成、缓存等。其不仅可以用在 JavaSE环境，也可以用在 JavaEE 环境。

### 2、优点

- 易于理解的 Java Security API
- 简单的身份认证，支持多种数据源
- 对角色的简单的授权，支持细粒度的授权
- 不跟任何的框架或者容器捆绑，可以独立运行

### 3、特性

`Authentication`身份认证/登录，验证用户是不是拥有相应的身份
`Authorization`授权，即验证权限，验证某个已认证的用户是否拥有某个权限，即判断用户是否能做事情 `SessionManagement`会话管理，即用户登录后就是一次会话，在没有退出之前，它的所有信息都在会话中
`Cryptography`加密，保护数据的安全性，如密码加密存储到数据库，而不是明文存储
`Caching`缓存，比如用户登录后，其用户信息，拥有的角色/权限不必每次去查，提高效率
`Concurrency`Shiro支持多线程应用的并发验证，即如在一个线程中开启另一个线程，能把权限自动传播过去
`Testing`提供测试支持
`RunAs`允许一个用户假装为另一个用户（如果他们允许）的身份进行访问
`RememberMe`记住我，这是非常常见的功能，即一次登录后，下次再来的话不用登录了

### 4、架构

`Subject`主体，代表了当前的“用户”，这个用户不一定是一个具体的人，与当前应用交互的任何东西都是Subject，如网络爬虫， 机器人等；即一个抽象概念；所有Subject都绑定到SercurityManager，与Subject的所有交互都会委托给SecurityManager；可以把Subject认为是一个门面；SecurityManager才是实际的执行者
`SecurityManage`安全管理器；即所有与安全有关的操作都会与SecurityManager交互；且它管理着所有Subject； 可以看出它是Shiro的核心，它负责与后边介绍的其他组件进行交互
`Realm`域，Shiro从Realm获取安全数据（如用户，角色，权限），就是说SecurityManager要验证用户身份， 那么它需要从Realm获取相应的用户进行比较以确定用户身份是否合法；也需要从Realm得到用户相应的角色/权限进行验证用户是否能进行操作；可以有1个或多个Realm，我们一般在应用中都需要实现自己的Realm
`SessionManager`如果写过Servlet就应该知道Session的概念，Session需要有人去管理它的生命周期，这个组件就是SessionManager
`SessionDAO`DAO大家都用过，数据库访问对象，用于会话的CRUD，比如我们想把Session保存到数据库，那么可以实现自己的SessionDAO，也可以写入缓存，以提高性能
`CacheManager`缓存控制器，来管理如用户，角色，权限等的缓存的；因为这些数据基本上很少去改变，放到缓存中后可以提高访问的性能

应用代码通过Subject来进行认证和授权，而Subject又委托给SecurityManager； 我们需要给Shrio的SecurityManager注入Realm，从而让SecurityManager能得到合法的用户及其权限进行判断，Shiro不提供维护用户/权限，而是通过Realm让开发人员自己注入。

Shiro不会去维护用户，维护权限；这些需要自己去设计/提供；然后通过响应的接口注入给Shiro即可

