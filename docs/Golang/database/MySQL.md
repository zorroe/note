## 创建数据库

新建`test`数据库，`person`、`place`表

```mysql
CREATE TABLE `person` (
    `user_id` int(11) NOT NULL AUTO_INCREMENT,
    `username` varchar(260) DEFAULT NULL,
    `sex` varchar(260) DEFAULT NULL,
    `email` varchar(260) DEFAULT NULL,
    PRIMARY KEY (`user_id`)
  ) ENGINE=InnoDB AUTO_INCREMENT=2 DEFAULT CHARSET=utf8;

CREATE TABLE place (
    country varchar(200),
    city varchar(200),
    telcode int
)ENGINE=InnoDB AUTO_INCREMENT=2 DEFAULT CHARSET=utf8;
```

## mysql使用

使用第三方开源的`mysql`库：

1. `mysql`驱动：`github.com/go-sql-driver/mysql`
2. 基于`mysql`的封装：`github.com/jmoiron/sqlx`

命令行输入：

```bash
go get github.com/go-sql-driver/mysql 
go get github.com/jmoiron/sqlx
```

结构体与数据库匹配

```go
type Person struct {
	UserId   int    `db:"user_id"`
	Username string `db:"username"`
	Sex      string `db:"sex"`
	Email    string `db:"email"`
}

type Place struct {
	Country string `db:"country"`
	City    string `db:"city"`
	TelCode int    `db:"telcode"`
}
```

数据源配置

```go
var Db *sqlx.DB

func init() {
	database, err := sqlx.Open("mysql", "root:123456@tcp(47.103.77.106:3306)/test")
	if err != nil {
		fmt.Println("open mysql failed,", err)
		return
	}
	Db = database
}
```

## 增

```go
func insert(name string, sex string, email string) bool {
   r, err := Db.Exec("insert into person(username, sex, email)values(?, ?, ?)", name, sex, email)
   if err != nil {
      fmt.Println("exec failed, ", err)
      return false
   }
   id, err := r.LastInsertId()
   if err != nil {
      fmt.Println("exec failed, ", err)
      return false
   }

   fmt.Println("insert succ:", id)
   return true
}
```

## 删

```go
func delete(id int) {
   exec, err := Db.Exec("delete from person where user_id = ?", id)
   if err != nil {
      fmt.Println("exec failed", err)
      return
   }
   affected, err := exec.RowsAffected()
   if err != nil {
      fmt.Println("exec failed", err)
   }
   fmt.Println("affected:", affected)
}
```

## 改

```go
func update(person Person) {
   result, err := Db.Exec("update person set username=? where user_id = ?", person.Username, person.UserId)
   if err != nil {
      fmt.Println("exec failed", err)
      return
   }
   affected, err := result.RowsAffected()
   if err != nil {
      fmt.Println("exec failed", err)
   }
   fmt.Println("affected:", affected)
}
```

## 查

```go
func selectPerson(name string) []Person {
   var person []Person
   err := Db.Select(&person, "select * from person where username = ?", name)
   if err != nil {
      fmt.Println("exec failed, ", err)
      return person
   }
   return person
}
```

## 主函数

```go
func main() {
	defer Db.Close()

	insert("Jack", "man", "111@131.com")

	person := selectPerson("Jack")

	person := Person{
		UserId:   2,
		Username: "Lucy",
		Sex:      "Women",
		Email:    "123@312.com",
	}
	update(person)

	delete(3)
}
```

## 事务

```go
Db.Begin()        开始事务
Db.Commit()       提交事务
Db.Rollback()     回滚事务
```

```go
func insert(name string, sex string, email string) bool {
	conn, err := Db.Beginx()
	if err != nil {
		fmt.Println("begin failed", err)
		return false
	}
	r, err := Db.Exec("insert into person(username, sex, email)values(?, ?, ?)", name, sex, email)
	if err != nil {
		fmt.Println("exec failed, ", err)
		return false
	}
	id, err := r.LastInsertId()
	if err != nil {
		fmt.Println("exec failed, ", err)
		conn.Rollback()
		return false
	}
	fmt.Println("insert succ:", id)
	conn.Commit()
	return true
}
```

