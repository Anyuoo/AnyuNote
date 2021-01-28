## Golang Project

### 使用gin创建web项目

> 引入gin

```bash
go env -w GO111MODULE=on
go env -w GOPROXY=https://goproxy.io,direct
#项目目录下
#go mod init gin
go mod edit -require github.com/gin-gonic/gin@latest
```

s