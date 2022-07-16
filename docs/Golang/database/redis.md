## Redis的使用

使用第三方开源的redis库：`github.com/garyburd/redigo/redis`

命令行输入：`go get github.com/garyburd/redigo/redis`

## 链接Redis

```go
func main() {
   conn, err := redis.Dial("tcp", "47.103.77.106:6379")
   if err != nil {
      fmt.Println("conn redis failed", err)
      return
   }
   fmt.Println("redis conn success")
   defer conn.Close()
    ...
}
```

## String类型Get、Set

### Set

```go
_, err = conn.Do("Set", "name", "jack")
if err != nil {
   fmt.Println("set error", err)
   return
}
```

### Get

```go
reply, err := redis.String(conn.Do("Get", "name"))
if err != nil {
   fmt.Println("get error")
}
fmt.Println(reply)
```

## String批量操作

### MSet

```go
_, err := conn.Do("MSet", "abc", 100, "efg", 200)
if err != nil {
   fmt.Println(err)
   return
}
```

###MGet

```go
reply, err := redis.Ints(conn.Do("MGet", "abc", "efg"))
if err != nil {
    fmt.Println(err)
}
for _, r := range reply {
    fmt.Println(r)
}
```

## 设置过期时间

```go
_, err := conn.Do("expire", "abc", 18)
if err != nil {
   fmt.Println("expire failed")
   return
}
```

## List队列操作

### lpush

```go
_, err := conn.Do("lpush", "book_list", "time", "age", "year")
if err != nil {
   fmt.Println(err)
   return
}
```

### lpop

```go
res, err := redis.String(conn.Do("lpop", "book_list"))
if err != nil {
   fmt.Println(err)
}
fmt.Println(res)
```

## Hash表

### HSet

```go
_, err := conn.Do("HSet", "books", "number", 10)
if err != nil {
   fmt.Println(err)
   return
}
```

### HGet

```go
reply, err := redis.Int(conn.Do("HGet", "books", "number"))
if err != nil {
   fmt.Println(err)
}
fmt.Println(reply)
```

## Redis连接池

```go
// 初始化连接池
var pool *redis.Pool

func init() {
   pool = &redis.Pool{
      MaxIdle:     16,  // 初始的连接储量
      MaxActive:   0,   // 最大连接数量，0是按需分配
      IdleTimeout: 300, // 连接关闭时间，300秒
      Dial: func() (redis.Conn, error) {  // 连接的数据库
         return redis.Dial("tcp", "47.103.77.106:6379")
      },
   }
}

func main() {
	c := pool.Get()    // 从连接池取一个连接
	defer pool.Close() // 运行结束关闭连接池
	defer c.Close()    // 运行结束放回连接池
	_, err := c.Do("Set", "gender", "男")
	if err != nil {
		fmt.Println(err)
	}
	reply, err := redis.String(c.Do("Get", "gender"))
	if err != nil {
		fmt.Println(err)
	}
	fmt.Println(reply)
}
```