### Meta注解

- 类注解：网络元

  value:映射名

  nodes:默认节点

```java
@Meta(value = "User",nodes = "Shanghai")//默认上海节点
public abstract class User
```

- 属性注解：共享资源

  value:映射名

```java
@Meta(value = "User")
private String password;
```

- 方法注解：网络请求

  value:映射名

  nodes:节点映射，空表示默认节点

```java
@Meta(value = "Login",nodes = "Shanghai")
public abstract boolean login();
```

- 参数注解：共享资源

  value:映射名

```java
public abstract boolean addPack(@Meta Package aPackage);
```

## 