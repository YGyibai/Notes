# Springboot集成shiro

## shiro是什么

shiro是一个功能强大且易于使用的 Java 安全框架，可执行身份验证、授权、加密和会话管理。

## 主要功能

- Subject：主体，概念中的"用户"
- SecurityManager：安全管理器，管理所有Subject，shiro架构的核心，充当一种“伞形”对象,主要协调shiro内部的各种安全组件

- Realms：用于进行权限信息的验证，Realm 充当 Shiro 和应用程序安全数据之间的“桥梁”或“连接器”。在配置 Shiro 时，您必须至少指定一个 Realm 用于身份验证和/或授权。可以配置多个 Realm，但至少需要一个。

## 细分功能

1. Authentication：身份认证/登录(账号密码验证)。

2. Authorization：授权，即角色或者权限验证。

3. Session Manager：会话管理，用户登录后的session相关管理。

4. Cryptography：加密，密码加密等。

 5. Web Support： Web支持，集成Web环境。

 6. Caching：缓存，用户信息、角色、权限等缓存到如redis等缓存中。

 7. Concurrency：多线程并发验证，在一个线程中开启另一个线程，可以把权限自动传播过去。

 8. Remember Me：记住我，登录后，下次再来的话不用登录了。