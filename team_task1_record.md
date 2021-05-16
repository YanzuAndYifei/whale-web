## 彦祖&亦菲队过程记录 - Task1

### 1. 重配环境中踩到的坑（Windows环境）
#### A) MySQL

**问题1**：打开DBeaver连接MySQL时提示：`Public Key Retrieval is not allowed`

解决：

https://blog.csdn.net/u012593888/article/details/113245545

**问题2**：启动Mysql时，运行`net start mysql` - 报错
```
The MySQL service is starting.
The MySQL service could not be started.
```

解决：

暂停mysql，删除`Data`文件，重新初始化。命令如下：
```
net stop mysql
del C:\Program Files\MySQL\MySQL Server 8.0\Data
mysqld  --initialize
```
> 提示：可到`C:\Program Files\MySQL\MySQL Server 8.0\bin`文件夹下运行`mysqld`命令

**问题3**：运行名`mysql -u root -p`报错:

`MySQL Error: : 'Access denied for user 'root'@'localhost'`

解决：
初始化mysql后，可到本地Data文件夹下看到临时密码
- `mysqld  --initialize`
- 找到文件"C:\Program Files\MySQL\MySQL Server 8.0\Data\<fileName>.err"里有temporary的密码
- 输入密码，成功进入mysql
- 改密码：`ALTER USER 'root'@'localhost' IDENTIFIED BY '123456';`

#### B) Backend

**问题**：运行命令`python manage.py migrate`时报错如下：
`RuntimeError: 'cryptography' package is required for sha256_password or caching_sha2_password auth methods`

解决：

记得切换账号登录到mysql
`mysql -ubluewhale -pbluewhale`

后端 http://127.0.0.1:8000/

### 2.MAC环境遇到的坑及解决方法

#### 1） 开启了数据库，但后端服务无法开启的问题

**解决方法**： 尝试在`backend/bluewhale/setting.py` 中`DATABASES`内容中的`HOST`的值更改为`127.0.0.1`

#### 2) 在swagger的页面中，进行文档的编写，运行的时候404的问题

**解决过程**：（可能）将编写的文档保留替换掉原先的文档，重启**mock**的内容

#### 3）登陆界面401未授权的情况

**解决过程**：在task00的时候注册的超级用户，将超级用户的账号密码登陆，即可显示对应的用户信息。

#### 4）swagger启动不成功？

**解决过程**： 全程保持前端，后端，数据库的开启，在开启的状态再启动`swagger`



### 3. 笔记（Windows环境）
#### A) MySQL
```
use bluewhale;
desc core_user;
show tables;
select * from core_user;
```
#### B) Docker启动Swagger
如果文件在`C:\UserRepo\whale-web\openapi.yaml`，端口用81，命令应为
`docker run -d -p 81:8080 -v C:\UserRepo\whale-web:/mnt -e SWAGGER_FILE=/mnt/openapi.yaml  swaggerapi/swagger-editor`
`docker stop <CcontainerId>`

### 4. Useful link
- Swagger语法及学习 https://huangwenchao.gitbooks.io/swagger/content/
- Docker command https://docs.docker.com/engine/reference/commandline/run/
- Swagger Docker doc https://github.com/swagger-api/swagger-editor#docker



