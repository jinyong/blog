# 摘要
* 现维护项目中使用的表单验证
>strtus1的xml验证插件 + ActionForm。
>>xml插件验证的问题
>>>得给每一个action的每一个表单属性做配置，现在超过2000多行。根据需求增改页面都要有输入->确认->完成页面。增和改其实是个重复的逻辑。因为这些原因导致配置文件里有两份同样的配置。

>>ActionForm的问题
>>>ActionForm里做的是xml难以实现的验证。比如验证是否同名（需要操作数据库），或者一个表单属性涉及到其他表单属性。经常维护发现，很多都是重复性的代码，而且，遇到几十个表单属性的情况，简直就是灾难。
* spring
>项目里已经导入spring 3.x
>>spring mvc很早就已经能代替struts了。

综上，一是想慢慢摆脱struts1，二是想简化验证过程。

# 表单验证的简化
>方式一： 通过实现spring提供的validator来实现
>>要在controller利用@InitBinder绑定实现validator的类。在该类的isValid方法里提供所有的验证。

>>优点
>>>感觉跟struts1的ActionForm差不多，没感觉到什么优点。

>>缺点
>>>一旦用@InitBinder绑定验证类，会导致Bean里的注解验证（方式二）失效。

>方式二: 用注解验证
>>直接上例子。
```
public class User {
  
  @NotEmpty
  private String name;
  
  public String getName(){
    return name;
  }
  public void setName(String name){
    this.name = name;
  }
}
```
>>>简单粗暴。一个注释就验证了name不能为空。还可以指定自定义错误信息like @NotEmpty(message="登录帐号不能为空")，还可以这样 @NotEmpty(message="{login.account.not.empty}")。除了这个还有很多hibernate-validator提供的。不知道为什么spring只提供接口，没有去实现。所以要用到hibernate-validator。
>>>>问题
>>>>>但是在实际运用中出了点问题。很多message里提供的key的内容是可以格式化的。比如propertis文件里的login.account.not.empty={0}不能为空。而且存在另一个专门指定“登录账号”的key，login.accound=登录账号。所以最终的错误信息是由login.account.not.empty和login.account的内容组成的。但是hibernate-validator似乎不提供这样的结构。有可能我的了解不深。不管怎么说把朕急坏了。差点要放弃。还是度娘谷哥有招。居然可以自定义注解验证。

>方式三： 自定义注解验证
>>有两种（或以上）
>>>适用于类属性级别的
>>>>就像上面的例子，作用于类属性。虽说第一次有点代码量，但是只要定义好了，不管新增多少，都可以用一行注解解决。

>>>适用于整个类的
>>>>作用于整个类。在属性级别做不到的就在这里验证。这个验证的通用性就比较差了。估计只能也只有用在一个类上。

# 参考文章
* [https://docs.jboss.org/hibernate/validator/5.1/reference/en-US/html/validator-customconstraints.html#section-cross-parameter-constraints](https://docs.jboss.org/hibernate/validator/5.1/reference/en-US/html/validator-customconstraints.html#section-cross-parameter-constraints)
* [https://www.cnblogs.com/ASPNET2008/p/5831766.html](https://www.cnblogs.com/ASPNET2008/p/5831766.html)
