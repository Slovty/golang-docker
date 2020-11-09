## golang docker for windows
### Go-dev
#####  windows10下Dockerfile脚本命令执行报错，所以在ubuntu里面安装Golang

项目结构:
```
.
├── docker-compose.yml
├── Dockerfile
└── README.md
```

[_docker-compose.yaml_](docker-compose.yml)
```
version: "3"

services:
  godev:
    build: .
    tty: true
    stdin_open: true
    environment:
      TZ: Asia/Shanghai
    ports:
        - 8080:8080
        - 8081:8081
        - 8443:8443
        - 8000:8000
    volumes:
      - D:/workSpace/goworkspace/src:/usr/local/src
```
注意更改 volumes 的配置为自己本机的绝对路径.

## 通过 docker-compose 部署

```
$ docker-compose up -d

```

## 预期结果
 正常状态:
```
$ docker ps -a
CCONTAINER ID        IMAGE                           COMMAND                  CREATED             STATUS                   PORTS                                                                              NAMES
 5d618a726330        win_godev                       "/bin/bash"              6 hours ago         Up 6 hours               0.0.0.0:8000->8000/tcp, 0.0.0.0:8080-8081->8080-8081/tcp, 0.0.0.0:8443->8443/tcp   win_godev_1
```
 进入到容器里面的ubuntu,安装golang及相关必要依赖:
```
$ winpty docker exec -it d73631b8775f bash
root@d73631b8775f:/# apt-get update
安装
apt-get install curl make openssl libssl-dev  golang-1.14
```

进入容器后，进入到目标目录:
```
$ go env -w GOPROXY=https://goproxy.cn,direct
$ export GO111MODULE=on
$ go mod tidy
$ make run-api
$ curl localhost:8081/demo/hello

{"code":0,"msg":"hello","data":{"func":"hello api."}}

```
停止 
```
$ docker-compose stop
```

停止并且移除容器
```
$ docker-compose down
```
