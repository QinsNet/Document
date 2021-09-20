## Token理念

还是简单分析一下场景需求：

一个用户发起登录请求，服务器执行登录逻辑之后，有时会需要将用户信息暂存内存，需要时再查询该用户信息。

比如用户登录，服务器生成一个User类的实体，内部包涵了该用户的临时信息，比如登录时间、用户等级、用户凭证.....当用户断开连接时，再将这个实体销毁。

并且服务时，往往是多个客户端对标一个服务端，如果遇到聊天系统的功能需求，客户端之间也往往会通过服务端进行通讯，这样服务端也需要区别不同的客户端。

基于上述需求，Ethereal开放了Token类别，Token相当于一个客户端连接体，用户控制BaseToken即控制客户端连接体。

BaseToken内含有唯一Key值属性，Ethereal通过用户给予的Key值属性，对Token进行生命周期处理。

```c#
[Service]
public bool Login(BaseToken token, string username,string password)
{
    token.Key = username;//为该token设置键值属性
    BaseToken.Register();//将token注册，受Ethereal管理其生命周期
}
```

通过上面的函数，我们似乎发现了一个特殊之处，token放在了服务类的**首参**，其实刚刚的加法函数也可以改写为：

```c#
public class ServerService
{
    [Service]
    public int Add(BaseToken token,int a,int b)
    {
        return a + b;
    }
}
```

Ethereal会根据用户的首参情况，来决定是否为首参注入token实体。

在Request中，不必提供token参数的定义，对于Request，仍保持基本接口规范即可。

`    public Integer Add(Integer a,Integer b);`



```c#
[Service]
public bool Login(User user, string username,string password)
{
    user.Key = username;//为该token设置键值属性
    user.Register();//将token注册，受Ethereal管理其生命周期
}
```

BaseToken是可继承的，那就代表了用户可以通过自定义一个User类，并继承BaseToken，这进一步的转换了设计理念，从面向连接体，变为了面向用户。