# go 数据库相关

[TOC]







## go的官方接口解读

[参考链接](https://astaxie.gitbooks.io/build-web-application-with-golang/content/zh/05.1.html)

面向接口编程

### sql.Register

### driver.Driver

```
type Driver interface {
    Open(name string) (Conn, error)  返回一个链接，返回的Conn只能用来进行一次goroutine的操作
}
```



### driver.Conn

```
type Conn interface {
	//Prepare函数返回与当前连接相关的执行Sql语句的准备状态，可以进行查询、删除等操作
    Prepare(query string) (Stmt, error) 
    //关闭链接
    Close() error
    //返回一个代表事务处理的TX句柄，通过它你可以进行查询,更新等操作，或者对事务进行回滚、递交
    Begin() (Tx, error)
}
```

```
	//更新数据
	//可以看到这里的prepare做好了准备，然后调用exec执行就可
	stmt, err = db.Prepare("update userinfo set username=? where uid=?")
	checkErr(err)

	res, err = stmt.Exec("add yaoyue", id)
	checkErr(err)
```



### driver.Stmt

> 一种准备好的状态，和conn相关联

```Go
type Stmt interface {
    Close() error
    NumInput() int               //返回当前预留参数的个数
    Exec(args []Value) (Result, error)   执行更新，插入等操作
    Query(args []Value) (Rows, error)    执行查询操作
}
```

### driver.Tx

```
type Tx interface {
    Commit() error
    Rollback() error
}
事务处理一般就2个过程，递交或回滚
```



### driver.Execer

```Go
type Execer interface {
    Exec(query string, args []Value) (Result, error)
}
//一个可以选择是否实现的接口
```

### driver.Result

```
type Result interface {
    LastInsertId() (int64, error)  //函数返回由数据库执行插入操作得到的自增ID号。
    RowsAffected() (int64, error)   //函数返回query操作影响的数据条目数。
} 
```



### driver.Rows

### driver.RowsAffected

RowsAffected其实就是一个int64的别名，但是他实现了Result接口，用来底层实现Result的表示方式

### driver.Value

### driver.ValueConverter

ValueConverter接口定义了如何把一个普通的值转化成driver.Value的接口

### driver.Valuer

### database/sql

database/sql在database/sql/driver提供的接口基础上定义了一些更高阶的方法，用以简化数据库操作




