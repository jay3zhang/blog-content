# python操作MySQL数据库
--对照MySQLdb学习pymysql

参考资料：
[慕课网-Python操作MySQL数据库][1]
[pymysql文档](http://pymysql.readthedocs.io/en/latest/)
[PyMySQL on github](https://github.com/PyMySQL/PyMySQL)


[1]:http://www.imooc.com/learn/475

MySQL-python（即MySQLdb）只支持python2，在py3中用pymysql，pymysql完美兼容MySQLdb。

## 数据库对象connection&&cursor对象

### connection对象建立与数据库的连接
* 创建方法：

`MySQLdb.Connect(host,port,user,passwd,db,charset)`

`pymysql.connect(host,port,user,password,db,charset)`

tips: **charset=utf8 而不是utf-8**

* connection对象的方法：

1. cursor()	 创建并返回游标
2. commit()   提交当前事务
3. rollback()   回滚
4. close()	关闭连接

* example:	（py2的MySQLdb版本的代码[看这里][1]）

首先利用SQLyog建立一个名为imooc的数据库，再建立一个user表并插入几条数据。（也可以在sqlyog中点击创建）
```
create table imooc.user(
      userid INT(11) NOT NULL AUTO_INCREMENT,
	username VARCHAR(100) DEFAULT NULL,
	PRIMARY KEY (userid)
)engine=innodb  default charset=utf8
```
插入语句示例：
`insert into imooc.user(userid,username) values(10,'name10');`

```
import pymysql
conn = pymysql.connect(host = '127.0.0.1',
                       port = 3306,
                       user = 'root',
                       password = '123456',
                       db = 'imooc',
                       charset = 'utf8'
                       )
cursor = conn.cursor()
print(conn)
print(cursor)
　　
conn.close()
cursor.close()
```

### cursor对象用于查询和获取结果
* 支持的方法：
1. execute()   执行一个数据库命令
2. fetchone()   取结果集的下一行
3. fetchmany(size)   取结果集的下n（n=size）行
4. fetchall()    取结果集的剩下所有行
5. rowcount()   最后一次执行execute()影响的行数
6. close()    关闭cursor对象

```
import pymysql
conn = pymysql.connect(...)  #与之前相同
cursor = conn.cursor()
　　
sql = "select * from user"
cursor.execute(sql)
print(cursor.rowcount)
　　
rs = cursor.fetchone()
print(rs)
rs = cursor.fetchmany(3)
print(rs)
rs = cursor.fetchall()
for row in rs:
    print('id=%s, name=%s' % row)
　
conn.close()
cursor.close()
```

## select/update/insert/delete

与在mysql语法相同，靠execute()执行

* example：
```
sql = "mysql查询语句"
cursor.execute(sql)
```

## 银行转账实例

账户A给账户B转账100元：
1. 检测两个账户是否存在
2. 检测账户A余额是否足够
3. A余额-100，B余额+100
4. 提交更改事务

在数据库imooc中创建表account
```
create table imooc.account(
 	acctid INT(11) NOT NULL comment '账户ID',
 	money int(11) DEFAULT NULL comment '余额',
 	PRIMARY KEY (acctid)
)engine=innodb  default charset=utf8
```

代码实例：
```
# coding:utf-8
import sys
import pymysql

class TransferMoney(object):
    def __init__(self,conn):
        self.conn = conn

    def transfer(self,from_acctid,to_acctid,money):
        try:
            self.check_acct_available(from_acctid)
            self.check_acct_available(to_acctid)
            self.has_enough_money(from_acctid,money)
            self.reduce_money(from_acctid,money)
            self.add_money(to_acctid,money)
            conn.commit()
        except Exception as e:
            conn.rollback()
            raise e

    def check_acct_available(self, acctid):
        cursor = self.conn.cursor()
        try:
            sql = "select * from account where acctid=%s" % acctid
            cursor.execute(sql)
            print('check_acct_available:'+sql)
            rs = cursor.fetchall()
            if len(rs)!=1:
                raise Exception('账号%s不存在' % acctid)
        finally:
            cursor.close()

    def has_enough_money(self, acctid, money):
        cursor = self.conn.cursor()
        try:
            sql = "select * from account where acctid=%s and money>%s" % (acctid,money)
            cursor.execute(sql)
            print('has_enough_money:' + sql)
            rs = cursor.fetchall()
            if len(rs)!=1:
                raise Exception('账号%s余额不足' % acctid)
        finally:
            cursor.close()

    def reduce_money(self, acctid, money):
        cursor = self.conn.cursor()
        try:
            sql = "update account set money=money-%s where acctid=%s" % (money,acctid)
            cursor.execute(sql)
            print('reduce_money:' + sql)
            rs = cursor.fetchall()
            if cursor.rowcount != 1:
                raise Exception('账号%s减款失败' % acctid)
        finally:
            cursor.close()

    def add_money(self,acctid,money):
        cursor = self.conn.cursor()
        try:
            sql = "update account set money=money+%s where acctid=%s" % (money,acctid)
            cursor.execute(sql)
            print('add_money:' + sql)
            rs = cursor.fetchall()
            if cursor.rowcount != 1:
                raise Exception('账号%s加款失败' % acctid)
        finally:
            cursor.close()

if __name__=='__main__':
    from_acctid = sys.argv[1]   # 转账用户
    to_acctid = sys.argv[2]     #收钱用户
    money = sys.argv[3]   # 转账金额

    conn = pymysql.connect(host = '127.0.0.1',
                       port = 3306,
                       user = 'root',
                       password = '123456',
                       db = 'imooc',
                       charset = 'utf8')

    tr_money = TransferMoney(conn)
    try:
        tr_money.transfer(from_acctid,to_acctid,money)
    except Exception as e:
        print(e)
    finally:
        conn.close()
```
