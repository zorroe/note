## 安装

* 安装beego

```bash
go get -u github.com/beego/beego/v2
```

* 安装bee

```bash
go get -u github.com/beego/bee/v2
```

> 若安装之后`bee`命令无效
>
> `cd $GOPATH\pkg\mod\github.com\beego\bee\v2@v2.0.4` 
>
> 执行`go build`
>
> 然后将生成的`bee.exe`复制到`$GOPATH\bin`目录下（先确保`$GOPATH\bin`目录被添加到环境变量）

## Hello beego

```bash
cd $GOPATH\src
bee new hello
cd hello
bee run
```

一旦程序开始运行，您就可以在浏览器中打开 http://localhost:8080/ 进行访问。

> 若出现错误`missing go.sum entry for module providing package ＜package_name＞`
>
> 执行命令`go build -mod=mod`