# Sql语句

## 基础语法

### 增

### 删

### 改

### 查

## 用户与授权

### 用户

#### 创建用户

```mysql
CREATE USER 'username'@'host' IDENTIFIED BY 'password';
```

#### 删除用户

```mysql
DROP USER 'username'@'host';
```

#### 修改用户密码

```mysql
SET PASSWORD FOR 'username'@'host' = PASSWORD('newpassword');
```

### 授权

#### 给用户授权

```mysql
GRANT privileges ON databasename.tablename TO 'username'@'host'
```

[^]: privileges：用户的操作权限，如SELECT，INSERT，UPDATE等，如果要授予所的权限则使用ALL databasename：数据库名 tablename：表名，如果要授予该用户对所有数据库和表的相应操作权限则可用*表示，如*.*

#### 查看用户拥有的权限

```mysql
SHOW GRANTS FOR 'pig'@'%'; 
```

#### 取消用户授权

```mysql
REVOKE privilege ON databasename.tablename FROM 'username'@'host';
```

