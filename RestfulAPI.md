---
title: 开源技术架构漫谈：基于Go语言快速构建RESTful API服务
date: 2017-05-23 14:14:12
tags: [Developer,DevOps,Golang]
categories: 工程技术
---
## 摘要
- What is a JSON API?
- 启动一个RESTful服务
- 抽象数据模型
- 增加路径分发功能
- 重构：Handlers & Router

<!--more-->

In this post, we will not only cover how to use Go to create a RESTful JSON API, but we will also talk about good RESTful design.

部分内容删减调整，原文请查看： [Making a RESTful JSON API in Go,2014Nov](https://thenewstack.io/make-a-restful-json-api-go/)

[Author:CORY LANOU](http://www.linkedin.com/in/corylanou/):a full stack technologist who has specialized in start-ups for the last 17 years. I'm currently working at InfluxDB on their core data team. I also help lead and organizer several community technology meetups and do Go training.

## 一、What is a JSON API?
JSON API 是数据交互规范，用以定义客户端如何获取与修改资源，以及服务器如何响应对应请求。JSON API设计用来最小化请求的数量，以及客户端与服务器间传输的数据量。通过遵循共同的约定，可以提高开发效率，利用更普遍的工具,基于 JSON API 的客户端还能够充分利用缓存，以提升性能。(更多：http://jsonapi.org.cn/format/)。

示例：
```js
{
  "links": {
    "posts.author": {
      "href": "http://example.com/people/{posts.author}",
      "type": "people"
    },
    "posts.comments": {
      "href": "http://example.com/comments/{posts.comments}",
      "type": "comments"
    }
  },
  "posts": [{
    "id": "1",
    "title": "Rails is Omakase",
    "links": {
      "author": "9",
      "comments": [ "5", "12", "17", "20" ]
    }
  }]
}
```

#### 启动一个RESTful服务

```bash
$ go run main.go

$ curl http://localhost:8080
Hello,"/"

```

```go
package main

import (
    "fmt"
    "html"
    "log"
    "net/http"
)

func main() {
    http.HandleFunc("/", func(w http.ResponseWriter, r *http.Request) {
        fmt.Fprintf(w, "Hello, %q", html.EscapeString(r.URL.Path))
    })

    log.Fatal(http.ListenAndServe(":8080", nil))

}
```

#### 增加路径分发功能
路径又称"终点"（endpoint），表示API的具体网址。在RESTful架构中，每个网址代表一种资源（resource）。
第三方组件（Gorilla Mux package）： “github.com/gorilla/mux”

```go
package main

import (
    "fmt"
    "log"
    "net/http"
    "github.com/gorilla/mux"
)

func main() {
    router := mux.NewRouter().StrictSlash(true)
    router.HandleFunc("/", Index)
    router.HandleFunc("/todos", TodoIndex)
    router.HandleFunc("/todos/{todoId}", TodoShow)

    log.Fatal(http.ListenAndServe(":8080", router))
}

func Index(w http.ResponseWriter, r *http.Request) {
    fmt.Fprintln(w, "Welcome!")
}

func TodoIndex(w http.ResponseWriter, r *http.Request) {
    fmt.Fprintln(w, "Todo Index!")
}

func TodoShow(w http.ResponseWriter, r *http.Request) {
    vars := mux.Vars(r)
    todoId := vars["todoId"]
    fmt.Fprintln(w, "Todo show:", todoId)
}
```

访问测试：

```bash
$ curl http://localhost:8080/todo
404 page not found
$ curl http://localhost:8080/todos
Todo Index! ,"/todos"
$ curl http://localhost:8080/todos/{123}
TodoShow: ,"123"
```

#### 抽象数据模型

创建一个数据模型“Todo”、“Routes”。在其它语言中，使用类（class）实现。
在Go语言中，没有class，必须使用结构(struct)。

**Todo.go**
```go
package main

import "time"

type Todo struct {
      Id        int       `json:"id"`
      Name      string    `json:"name"`
      Completed bool      `json:"completed"`
      Due       time.Time `json:"due"`
}

type Todos []Todo
```

**Routes.go**
```go
package main

import (
    "net/http"
    "github.com/gorilla/mux"
)

type Route struct {
    Name        string
    Method      string
    Pattern     string
    HandlerFunc http.HandlerFunc
}

type Routes []Route
```
#### 重构：Handlers & Router

**Handlers.go**
```go
package main

import (
    "encoding/json"
    "fmt"
    "net/http"
    "github.com/gorilla/mux"
)

func Index(w http.ResponseWriter, r *http.Request) {
    fmt.Fprintln(w, "Welcome!")
}

func TodoIndex(w http.ResponseWriter, r *http.Request) {
    todos := Todos{
        Todo{Name: "Write presentation"},
        Todo{Name: "Host meetup"},
    }

    if err := json.NewEncoder(w).Encode(todos); err != nil {
        panic(err)
    }
}

func TodoShow(w http.ResponseWriter, r *http.Request) {
    vars := mux.Vars(r)
    todoId := vars["todoId"]
    fmt.Fprintln(w, "Todo show:", todoId)
}
```

**Router.go**
```go
package main

import (
    "net/http"
    "github.com/gorilla/mux"
)

func NewRouter() *mux.Router {
    router := mux.NewRouter().StrictSlash(true)
    for _, route := range routes {
        var handler http.Handler
        handler = route.HandlerFunc
        handler = Logger(handler, route.Name)

        router.
            Methods(route.Method).
            Path(route.Pattern).
            Name(route.Name).
            Handler(handler)

    }
    return router
}
```

#### 启动入口是不是清爽很多！

**Main.go**
```go
Main.go
package main

import (
    "log"
    "net/http"
)

func main() {
    router := NewRouter()
    log.Fatal(http.ListenAndServe(":8080", router))
}
```

web access:http://localhost:8080/todos

>Todo Index! ,"/todos"
[
  {
      "id":0,
      "name":"Write sth ....",
      "completed":false,
      "due":"0001-01-01T00:00:00
  },
  {
      "id":1,
      "name":"Host meetup ....",
      "completed":false,
      "due":"0001-01-01T00:00:00Z"
  }
]

#### 增强功能：持久化

```go
func TodoCreate(w http.ResponseWriter, r *http.Request) {
    var todo Todo
    //add Todo instance
}
```

#### 增强功能：日志

```js
2017/05/23 15:57:23 http: multiple response.WriteHeader calls
2017/05/23 15:57:23 GET	/todos	TodoIndex	6.945807ms
2017/05/23 16:18:40 http: multiple response.WriteHeader calls
2017/05/23 16:18:40 GET	/todos	TodoIndex	2.127435ms
```

## Things We Didn’t Do

1. 版本控制
  API版本迭代 & 跨版本资源访问。常用做法是将版本号放在URL，较为简洁，例如：https://localhost:8080/v1/
  另一种做法是将版本号放在HTTP头信息中。

2. 授权验证：涉及到OAuth和JWT。
（1）[OAuth 2.0，OAuth2 is an authentication framework](http://www.rfcreader.com/#rfc6749),[RFC 6749](http://www.rfcreader.com/#rfc6749)
  OAuth2是一种授权框架，提供了一套详细的、可供实践的指导性解决方案。OAuth 2.0定义了四种授权方式。授权码模式（authorization code）、简化模式（implicit）、密码模式（resource owner password credentials）、客户端模式（client credentials）。

- [Getting Started with OAuth2 in Go | Youtube](https://t.co/HXbBiVorVO)
- [Extensible security first OAuth 2.0 and OpenID Connect SDK for Go ](https://github.com/ory/fosite)

(2)[JSON web tokens,JWT is an authentication protocol](https://jwt.io),[RFC 7519](https://tools.ietf.org/html/rfc7519)
JWT是一种安全协议。基本思路就是用户提供用户名和密码给认证服务器，服务器验证用户提交信息信息的合法性；如果验证成功，会产生并返回一个Token（令牌），用户可以使用这个token访问服务器上受保护的资源。

```js
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiYWRtaW4iOnRydWV9.TJVA95OrM7E2cBab30RMHrHDcEfxjoYZgeFONFh7HgQ
```

header:定义算法(alg:ALGORITHM)和TOKEN TYPE（typ）

```js
{
  "alg": "HS256",
  "typ": "JWT"
}
```

Data:
```js
{
  "sub": "1234567890",
  "name": "John Doe",
  "admin": true
}
```
- [AuthN | Modern, open source, web app authentication ](https://keratin.tech/)
- [API Authentication With Node.js](https://code.tutsplus.com/tutorials/api-authentication-with-nodejs--cms-29536?utm_source=twitter&utm_medium=social&utm_campaign=social_twtr_tuts_code)
3. eTags：关于缓存、性能和用户标识和追踪。


## 参考文献
1. [阮一峰:RESTful API 设计指南](http://www.ruanyifeng.com/blog/2014/05/restful_api.html)
2. [CORY LANOU:Making a RESTful JSON API in Go,2014Nov](https://thenewstack.io/make-a-restful-json-api-go/)
3. [InfoQ:使用ETags减少Web应用带宽和负载](http://www.infoq.com/cn/articles/etags)
4. [Stackoverflow:jwt vs oauth authentication](https://stackoverflow.com/questions/39909419/jwt-vs-oauth-authentication)
5. [OAuth 2 VS JSON Web Tokens:How to secure an API,20160605](http://www.seedbox.com/en/blog/2015/06/05/oauth-2-vs-json-web-tokens-comment-securiser-un-api/)
6. [阮一峰:理解OAuth 2.0,201405](http://www.ruanyifeng.com/blog/2014/05/oauth_2_0.html)
7. [RESTful routing in Go | Karl Seguin](http://openmymind.net/RESTful-routing-in-Go/)
