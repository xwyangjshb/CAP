**1、有CAP学习QQ群吗？**

CAP没有群。
原因是我希望培养大家独立思考和遇到问题时候的解决能力。
使用时候遇到问题先尝试看文档和独立解决，如果解决不了，可以提ISSUE或者发邮件。
QQ群有效沟通太低，浪费时间。

**2、CAP要求发送者与接收者必须使用不同的数据库吗？**

没有要求，要求不同的实例使用不同的数据库。不同实例的意思为，不同代码的两套程序。

但是如果你真的需要在不同的实例使用相同的数据库，那么可以参考下面3的答案。


**3、CAP如何在不同的实例中使用相同的数据库？**

如果想在不同的实例（程序）中连接相同的数据库，那么你可以在配置CAP的时候通过指定不同的数据库表名前缀来实现。

你可以通过以下方式来指定数据库表名前缀：

```cs
public void ConfigureServices(IServiceCollection services)
{
    services.AddCap(x =>
    {
        x.UseKafka("");
        x.UseMySql(opt =>
        {
            opt.ConnectionString = "connection string";
            opt.TableNamePrefix = "appone"; // 在这里配置不同的实例使用的表名前缀
        });
    });
}

```
注意：相同的实例不需要指定不同的表名称前缀，他们在接收消息的时候会进行负载均衡。

**4、CAP可以不使用数据库吗? 我仅仅是想通过她来传递消息，我可以接受消息丢失的情况** 

目前是不可以的。 

CAP 的设计目标即为在不同的微服务或者SOA系统中来保持数据一致性的一种解决方案，保证这种数据一致性方案的前提是利用了传统数据库的 ACID 特性，如果离开了数据库，那么CAP仅仅是一个消息队列的客户端封装，这没有任何意义。


**5、使用CAP时候，业务出现错误怎么回滚？** 

不能回滚，CAP是最终一致性的方案。

你可以想象你的场景为在调用第三方支付，假如你在进行一项第三方支付的操作，调用支付宝的接口成功后，而你自己的代码出现错误了，支付宝会回滚吗？ 如果不回滚那么是又应该怎么处理呢？ 这里也是同理。


