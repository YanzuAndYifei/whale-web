## 彦祖&亦菲队过程记录 - Task2
### 1. 【用户管理】后端
#### A) 更新数据库关系映射文件
> `whale-web\backend\core\models.py`，这样我们可以在数据库直观看到扩展的用户属性

**步骤：**
- 明确缺失的属性
  - 通过查看数据库：bluewhale.core_user的column
  - 也可通过查看数据库/后端交互文件：`whale-web\backend\core\migrations\0001_initial.py`文件
  - 也可通过查看后端代码：`whale-web\backend\core\models.py`文件，以及其所继承的`AbstractBaseUser`和`PermissionsMixin`
- 更新文件`whale-web\backend\core\models.py`，添加所缺失的`user_image`等属性
- 运行命令`python manage.py migrate`

**问题1**：运行命令`python manage.py migrate`报错如下
```
Your models in app(s): 'core' have changes that are not yet reflected in a migration, and so won't be applied.
Run 'manage.py makemigrations' to make new migrations, and then re-run 'manage.py migrate' to apply them.
```
解决：有错误提示，先运行`python manage.py makemigrations`即可
```
python manage.py makemigrations
python manage.py migrate
```
**问题2**：再执行完上述操作后，再次执行了`python manage.py migrate`报错如下
```
File "...\.virtualenvs\backend-taan1NRp\lib\site-packages\pymysql\err.py", line 143, in raise_mysql_exception
raise errorclass(errno, errval)
django.db.utils.OperationalError: (1060, "Duplicate column name 'user_image'")
```
解决：错误表明`user_image`列存在，但仍尝试migrate它，故失败了
原因：后端代码这边和数据库不匹配

StackOverFlow解释如下：[Duplicate column name](https://stackoverflow.com/questions/36016485/duplicate-column-name)
```
python manage.py makemigrations
python manage.py migrate --fake
```
#### B) 新增用户增删改查接口
- 更新【后端的关联配置文件】`whale-web\backend\bluewhale\urls.py`
>   - 增：注册即为增加用户信息：`path(f'{api_prefix}/register', register, name='register')`
>   - 查：Get即为查看用户信息：`path(f'{api_prefix}/me', get_user_info, name='user profile')`
>   - 增加：删除，更改
- 更新【具体实现】 `whale-web\backend\core\views_auth.py`
>   利用`UserModel`和`UserSerializer`

### 2. 【用户管理】前端
参考：openapi 3.0 - https://swagger.io/docs/specification/describing-parameters/

注意：运行完`npm run mock`，再重新打开VSCode的Swagger即可预览