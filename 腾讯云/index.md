# index
## 查看端口
```
netstat -pn | grep 9000
```
## 查看nginx状态
```
nginx -t
```

ps aux |grep mysqld

kill -9 14621

GRANT ALL PRIVILEGES ON *.* TO 'username'@'%' IDENTIFIED BY 'password' WITH GRANT OPTION;

mysql -u root -p

skip-grant-tables

FLUSH PRIVILEGES;

## 先装mysql root更新密码，更新权限
1.  ```mysqld_safe --skip-grant-tables &``` （提示进程已存在的话，先```ps aux |grep mysqld```, 再 ```kill```）
2. mysql
3. FLUSH PRIVILEGES;
4. GRANT ALL PRIVILEGES ON *.* TO 'username'@'%' IDENTIFIED BY 'password' WITH GRANT OPTION; (注意usename, domain, password)
> vi /etc/my.cnf 可以修改这个文件，添加skip-grant-tables, 然后重复3，4
## 更新密码
```
UPDATE user SET Password=PASSWORD("yourPassword") WHERE User='root';
```