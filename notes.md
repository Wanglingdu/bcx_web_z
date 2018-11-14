# DeepBc
## 数据库使用
数据库选择使用Mysql,具体搭建流程
1. 获取mysql镜像
```
sudo docker pull mysql:5.7.20
```
2. 运行一个mysql容器
```
sudo docker run --name DeepBC_Mysql -p 3306:3306 -e MYSQL_ROOT_PASSWORD=123456 -d mysql:5.7.20
```
上述命令各个参数含义：
```
run            运行一个容器
--name         后面是这个镜像的名称
-p 3306:3306   表示在这个容器中使用3306端口(第二个)映射到本机的端口号也为3306(第一个)
-d             表示使用守护进程运行，即服务挂在后台
```

3. docker中只是一个mysql的server,想要访问这个server需要安装一个mysql-client:
```
sudo apt install mysql-client-5.7
```
4. 使用客户端连接到mysql服务器:
```
mysql -h127.0.0.1 -P3306 -uroot -p123456
```

5. 连接到mysql之后，设置mysql字符集
```
mysql> SHOW VARIABLES LIKE 'character%';
mysql> SET character_set_database = utf8;   
mysql> SET character_set_server = utf8;   
```
6. 建立用户表
```sql
drop database if exists deepbc;
create database if not exists deepbc;
use deepbc
create table users
(
	id int not null auto_increment,
	username varchar(20) not null, #设定默认值
	password_hash varchar(128) not null,
	primary key (id)
);
create index index_id on users(id);
```
也可以把上面的sql语句存成.sql文件然后在客户端里使用source schema.sql执行
