假设，我们准备建立一个 developer 数据库帐户，用来管理数据库 mydb。

> 首先在 SQL Server 服务器级别，创建登陆帐户（create login）

		--创建登陆帐户（create login）
		create login developer with password='developer', default_database=HTRT
	
> 创建数据库用户（create user）：

		--为登陆账户创建数据库用户（create user）,在mydb数据库中的security中的user下可以找到新创建的developer
		create user developer for login developer with default_schema=dbo

		# 并指定数据库用户“developer” 的默认 schema 是“dbo”。这意味着 用户“developer” 在执行“select * from t”，实际上执行的是 “select * from dbo.t”。

> 通过加入数据库角色，赋予数据库用户“developer”权限：

		--通过加入数据库角色，赋予数据库用户“db_owner”权限
		exec sp_addrolemember 'db_owner', 'developer'
		此时，developer 就可以全权管理数据库 mydb 中的对象了。

		如果想让 SQL Server 登陆帐户“developer”访问多个数据库，比如 mydb2。 可以让 sa 执行下面的语句：

		--让 SQL Server 登陆帐户“developer”访问多个数据库
		use mydb2
		go create user developer for login developer with default_schema=dbo
		go exec sp_addrolemember 'db_owner', 'developer' go
		此时，developer 就可以有两个数据库 mydb, mydb2 的管理权限了！



create login test02 with password='Myjsy,Bjwqt_0', default_database=sms

create user test02 for login test02 with default_schema=dbo

exec sp_addrolemember 'db_owner', 'test02'

