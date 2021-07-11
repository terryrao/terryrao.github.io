# mysql 添加 root@localhost 失败
;date: 2019-07-05 15:40:03
;tags:

mysql 8.0.16版本中为了让 mysql 的 root 账户 能够远程登录，我在 mysql 数据库 中 直接将 root 用户的 host 从 localhost 改为 % 。然后再添加 'localhost' 记录时报错了。

```mysql
mysql > user mysql;
mysql > update user set host='%' where user = 'root';
Query OK, 0 rows affected (0.00 sec)
mysql > flush privileges;
Query OK, 0 rows affected (0.00 sec)
mysql > create user 'root'@'localhost' identified by 'password';
ERROR 1396 (HY000): Operation CREATE USER failed for 'root'@'localhost'
```

然后再试着添加新的用户时，host=% 的用户可以添加，但是 host=localhost  却一直建立不了，这就不好办了，因为本地登录的时候如果不添加 host 的时候，默认就是 localhost 。后来我试着删除 user=root host=loalhost ，然后再添加，看看能不能行：

```mysql
mysql > drop user 'root'@'localhost';
Query OK, 0 rows affected (0.01 sec)
mysql > create user 'root'@'localhost' identified by 'password';
Query OK, 0 rows affected (0.01 sec)
mysql > grant all privileges on *.* to 'root'@'localhost' with grant option;
Query OK, 0 rows affected (0.01 sec)
mysql > flush privileges;
Query OK, 0 rows affected (0.01 sec)
```

没想到居然成功了。这样就把  user=root , host=localhost 重新找回来了。overstackflow 上也有类似这样的问题，有答主说这是一个bug。解决方法跟上面的一样。

参考 ： https://stackoverflow.com/questions/5555328/error-1396-hy000-operation-create-user-failed-for-jacklocalhost
