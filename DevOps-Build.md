---
title: DevOps 漫谈:全生命周期构建管理
date: 2018-12-10 10:39:51
tags: [DevOps,Engineering,Golang]
categories: 工程技术
---
## 摘要

- 从源代码到产品
- 构建全生命周期管理：以 Golang 为例

<!--more-->

## 问题

## 构建生命周期

![](http://riboseyim-qiniu.riboseyim.com/build_process_go_v1.png)

- Pre Step: clean、variables(project,module,version,timestamp)
- Build: compile、package
- Install: install Deps
- Test:
- Release: Tag、build archive


## 基于 Mage 构建管理

```go
$ cd $project/test
$ mage build
$ mage -compile ./static-output
```

```go
//magefile.go
// +build mage

package main

import (
	"errors"
	"fmt"
	"os"
	"os/exec"
	"time"

	"github.com/magefile/mage/mg" // mg contains helpful utility functions, like Deps
	"github.com/magefile/mage/sh"
)

func init() {
	// ProjectName
	os.Setenv("MATTEL_REPO", "go-chilopod")
	os.Setenv("TAG", "v0.1.2")
}

// Default target to run when none is specified
// If not set, running mage will list available targets
// var Default = Build

// A build step that requires additional params, or platform specific steps for example
func Build_v1() error {
	mg.Deps(InstallDeps)
	fmt.Println("Building...")
	cmd := exec.Command("go", "build", "-o", "MyApp", ".")
	return cmd.Run()
}

func Build() error {
	mg.Deps(Dep)
	return sh.RunV("go", "build", "-o", "$MATTEL_REPO", "-ldflags="+ldflags(), "github.com/riboseyim/$MATTEL_REPO")
}

func Dep() error {
	fmt.Println("Installing Deps...")
	cmd := exec.Command("dep", "ensure")
	return cmd.Run()
}

// A custom install step if you need your bin someplace other than go/bin
func Install() error {
	mg.Deps(Build)
	fmt.Println("Installing...")
	return os.Rename("./MyApp", "/usr/bin/MyApp")
}

// Manage your deps, or running package managers.
func InstallDeps() error {
	fmt.Println("Installing Deps...")
	cmd := exec.Command("go", "get", "github.com/stretchr/piglatin")
	return cmd.Run()
}

// Clean up after yourself
func Clean() {
	fmt.Println("Cleaning...")
	os.RemoveAll("MyApp")
}

func Tools() error {
	//mg.Deps(Protoc)

	update := envBool("UPDATE")

	retool := "github.com/twitchtv/retool"

	args := []string{"get", retool}
	if update {
		args = []string{"get", "-u", retool}
	}

	if err := sh.Run("go", args...); err != nil {
		return err
	}

	return sh.Run("retool", "sync")
}

// retool runs a command using a retool-cached binary.
func retool(cmd string, args ...string) error {
	return sh.Run("retool", append([]string{"do", cmd}, args...)...)
}

func Release() (err error) {
	if os.Getenv("TAG") == "" {
		return errors.New("TAG environment variable is required")
	}
	if err := sh.RunV("git", "tag", "-a", "$TAG", "-m", "add tag $TAG by mage tools"); err != nil {
		return err
	}
	if err := sh.RunV("git", "push", "origin", "$TAG"); err != nil {
		return err
	}

	defer func() {
		if err != nil {
			sh.RunV("git", "tag", "--delete", "$TAG")
			sh.RunV("git", "push", "--delete", "origin", "$TAG")
		}
	}()
	return sh.RunV("goreleaser", "release", "--skip-publish", "--rm-dist")
	//return retool("goreleaser")
}

func ldflags() string {
	timestamp := time.Now().Format(time.RFC3339)
	hash := hash()
	tag := tag()
	if tag == "" {
		tag = "dev"
	}

	return fmt.Sprintf(`-X "github.com/riboseyim/project/proj.timestamp=%s" `+
		`-X "github.com/riboseyim/project/proj.commitHash=%s" `+
		`-X "github.com/riboseyim/project/proj.gitTag=%s"`, timestamp, hash, tag)
}

// tag returns the git tag for the current branch or "" if none.
func tag() string {
	s, _ := sh.Output("git", "describe", "--tags")
	return s
}

// hash returns the git hash for the current repo or "" if none.
func hash() string {
	hash, _ := sh.Output("git", "rev-parse", "--short", "HEAD")
	return hash
}

func envBool(key string) bool {
	value := os.Getenv(key)
	if value != "" {
		return true
	} else {
		return false
	}
}
```

```go
$ mage build
Installing Deps...
# github.com/riboseyim/go-chilopod/vendor/go.opencensus.io/trace
vendor/go.opencensus.io/trace/trace_go11.go:30:9: undefined: trace.NewContext
Error: running "go build -o go-chilopod -ldflags=-X "github.com/riboseyim/project/proj.timestamp=2018-12-30T21:54:20+08:00" -X "github.com/riboseyim/project/proj.commitHash=042e38b" -X "github.com/riboseyim/project/proj.gitTag=v0.1.1" github.com/riboseyim/go-chilopod" failed with exit code 2

$ dep ensure -update
```

## 构建工具集

- 集成管理：mage
- 依赖管理：godep、retool
- 发布管理：goreleaser


#### 依赖管理工具定位

- go get：管理 $GOPATH 代码
- 依赖管理工具定位为管理应用代码

#### 依赖分析工具：godag

```
godag --pkg_name=github.com/magefile/mage --pkg_path=/Users/yanrui/go/src/github.com/magefile/mage --depth=1 --dot_file_path=mage.dot
```

#### go dep

>Dep is a tool for managing dependencies for Go projects

- dep init: Gopkg.toml、vendor
- dep ensure: Gopkg.lock

```go
$ dep
Commands:
  init     Set up a new Go project, or migrate an existing one
  status   Report the status of the projects dependencies
  ensure   Ensure a dependency is safely vendored in the project
  version  Show the dep version information
  check    Check if imports, Gopkg.toml, and Gopkg.lock are in sync

Examples:
  dep init                               set up a new project
  dep ensure                             install the projects dependencies
  dep ensure -update                     update the locked versions of all dependencies
  dep ensure -add github.com/pkg/errors  add a dependency to the project

// Error

```
```go
$ dep init
Importing configuration from govendor.
These are only initial constraints, and are further refined during the solve process.
Detected govendor configuration file...
Converting from vendor.json...
  Trying * (b1433c2) as initial lock for imported dep github.com/Shopify/sarama
  Trying * (ecdeabc) as initial lock for imported dep github.com/davecgh/go-spew
  Trying * (b1fe83b) as initial lock for imported dep github.com/eapache/go-resiliency
  Trying * (bb955e0) as initial lock for imported dep github.com/eapache/go-xerial-snappy
  Using ^1.1.0 as initial constraint for imported dep github.com/eapache/queue
	..........
  Locking in  (e19ae14) for direct dep golang.org/x/text
  Locking in v1.17.0 (df01485) for transitive dep google.golang.org/grpc
  Locking in  (7560714) for direct dep github.com/google/goexpect
  Locking in  (fe724d1) for direct dep github.com/magefile/mage
Old vendor backed up to /Users/yanrui/go/src/github.com/riboseyim/go-chilopod/_vendor-20181230213719


aca80164:go-chilopod kurui$ dep status
PROJECT                              CONSTRAINT     VERSION        REVISION  LATEST   PKGS USED
github.com/Shopify/sarama            *                             b1433c2            1   
go.opencensus.io                     *                             9f6adc5            3  
github.com/eapache/queue             v1.1.0         v1.1.0         44cc805   v1.1.0   1     
github.com/pierrec/lz4               v1.1           v1.1           2fcda4c   v1.1     1  
github.com/soniah/gosnmp             branch master  branch master  cd9a07a   cd9a07a  1  
github.com/sparrc/go-ping            branch master  branch master  ef3ab45   ef3ab45  1  
google.golang.org/grpc               v1.17.0        v1.17.0        df01485   v1.17.0  1  
```

#### godep

>Godep is a tool for managing Go package dependencies.

```go
godep go run
godep go build
```

注意：
- 1) 如果使用 godep 包管理工具则不能在使用 go run 和 go build，否则还是会到 $GOPATH 目录匹配第三方库。
- 2) 目录结构 ./Godeps 、./vendor
- 3) 安装问题：墙内用户可能会遇到 can not found golang.org/x/...

```go
$ godep
Usage:
	godep command [arguments]

The commands are:
    save     list and copy dependencies into Godeps
    go       run the go tool with saved dependencies
    get      download and install packages with specified dependencies
    path     print GOPATH for dependency code
    restore  check out listed dependency versions in GOPATH
    update   update selected packages or the go version
    diff     shows the diff between current and previously saved set of dependencies
    version  show version info

//官方:GFW Error can not found golang.org/x/.......
go get github.com/tools/godep

//镜像:https://github.com/golang/tools

go get github.com/golang/tools

mkdir $GOPATH/src/golang.org/x

// 安装 godep -> $GOPATH\bin
go get github.com/tools/godep
```

```go
$ godep save
drwxr-xr-x   4 kurui  staff   128 12 24 11:18 Godeps
$ ls -rlt Godeps
-rw-r--r--  1 kurui  staff   136 12 24 11:18 Readme
-rw-r--r--  1 kurui  staff  4074 12 24 11:18 Godeps.json

$ more Godeps/Godeps.json
{
        "ImportPath": "github.com/riboseyim/go-chilopod",
        "GoVersion": "go1.11",
        "GodepVersion": "v80",
        "Deps": [
                {
                        "ImportPath": "github.com/Shopify/sarama",
                        "Comment": "v1.15.0-13-gb1433c2",
                        "Rev": "b1433c29241f23cec4704daf4d25358b7b3fbf9e"
                },
                {
                        "ImportPath": "github.com/davecgh/go-spew/spew",
                        "Comment": "v1.1.0-9-gecdeabc",
                        "Rev": "ecdeabc65495df2dec95d7c4a4c3e021903035e5"
                },
                {
                        "ImportPath": "github.com/eapache/go-resiliency/breaker",
                        "Comment": "v1.0.0-6-gb1fe83b",
                        "Rev": "b1fe83b5b03f624450823b751b662259ffc6af70"
                },
				.......
				]
}

$ ls -rlt vendor/
drwxr-xr-x  14 kurui  staff   448 12 23 21:12 github.com
drwxr-xr-x   3 kurui  staff    96 12 23 21:12 golang.org
-rw-r--r--   1 kurui  staff  8279 12 23 21:12 vendor.json
drwxr-xr-x   6 kurui  staff   192 12 24 11:18 go.opencensus.io

// 更新
$ go get a/b/xxx
$ godep update a/b/..
```

#### Retool

- [Project Retool](https://github.com/twitchtv/retool)

>It’s like dep but for go tools, pinning revisions so that all your developers run the exact same version.

#### Goreleaser

- [Project Goreleaser](https://goreleaser.com/)

>GoReleaser will build the binaries for your app for Windows, Linux and macOS, both amd64 and i386 architectures

```bash
$ brew install goreleaser
$ cp /usr/local/Cellar/goreleaser/0.94.0/bin/goreleaser /user/local/bin/
$
$ goreleaser init
• Generating .goreleaser.yml file
• config created; please edit accordingly to your needs file=.goreleaser.yml
$
```

- goreleaser.yml

```bash
# This is an example goreleaser.yaml file with some sane defaults.
# Make sure to check the documentation at http://goreleaser.com
before:
  hooks:
    # you may remove this if you don't use vgo
    - go mod download
    # you may remove this if you don't need go generate
    - go generate ./...
builds:
- env:
  - CGO_ENABLED=0
archive:
  replacements:
    darwin: Darwin
    linux: Linux
    windows: Windows
    386: i386
    amd64: x86_64
checksum:
  name_template: 'checksums.txt'
snapshot:
  name_template: "{{ .Tag }}-next"
changelog:
  sort: asc
  filters:
    exclude:
    - '^docs:'
    - '^test:'
```

```go
$ git tag -a v0.1.0 -m "First release"
$ git push --tags origin master
$ goreleaser release --skip-publish

   • releasing using goreleaser 0.94.0...
   • loading config file       file=.goreleaser.yml
   • RUNNING BEFORE HOOKS
      • running go generate ./...
   • GETTING AND VALIDATING GIT STATE
      • releasing v0.1.0, commit 1f47832dffd1c84f65a16ed01868bb29f1419e0e
   • SETTING DEFAULTS
      • Invalid value %!s(MISSING) for prerelease. Should be auto, true or false
   • CHECKING ./DIST  
   • WRITING EFFECTIVE CONFIG FILE
      • writing                   config=dist/config.yaml
   • GENERATING CHANGELOG
      • writing                   changelog=dist/CHANGELOG.md
   • LOADING ENVIRONMENT VARIABLES
      • skipped                   reason=publishing is disabled
   • BUILDING BINARIES
      • building                  binary=dist/linux_386/go-chilopod
      • building                  binary=dist/darwin_amd64/go-chilopod
      • building                  binary=dist/darwin_386/go-chilopod
      • building                  binary=dist/linux_amd64/go-chilopod
   • ARCHIVES         
      • creating                  archive=dist/go-chilopod_0.1.0_Linux_i386.tar.gz
      • creating                  archive=dist/go-chilopod_0.1.0_Darwin_i386.tar.gz
      • creating                  archive=dist/go-chilopod_0.1.0_Darwin_x86_64.tar.gz
      • creating                  archive=dist/go-chilopod_0.1.0_Linux_x86_64.tar.gz
   • LINUX PACKAGES WITH NFPM
      • skipped                   reason=no output formats configured
   • SNAPCRAFT PACKAGES
      • skipped                   reason=no summary nor description were provided
   • CALCULATING CHECKSUMS
      • checksumming              file=go-chilopod_0.1.0_Linux_x86_64.tar.gz
      • checksumming              file=go-chilopod_0.1.0_Darwin_i386.tar.gz
      • checksumming              file=go-chilopod_0.1.0_Linux_i386.tar.gz
      • checksumming              file=go-chilopod_0.1.0_Darwin_x86_64.tar.gz
   • SIGNING ARTIFACTS
      • skipped                   reason=artifact signing is disabled
   • DOCKER IMAGES    
      • skipped                   reason=docker section is not configured
   • PUBLISHING       
      • skipped                   reason=publishing is disabled
   • release succeeded after 13.70s
```



#### Mage

- [Project Mage](https://magefile.org)

```go
$ go get -u -d github.com/magefile/mage
$ cd $GOPATH/src/github.com/magefile/mage
$ go run bootstrap.go
Running target: Install
exec: go env GOPATH
exec: git rev-parse --short HEAD
exec: git describe --tags
exec: go build -o /Users/yanrui/go/bin/mage -ldflags=-X "github.com/magefile/mage/mage.timestamp=2018-12-10T10:36:22+08:00" -X "github.com/magefile/mage/mage.commitHash=fe724d1" -X "github.com/magefile/mage/mage.gitTag=v1.7.1-2-gfe724d1" github.com/magefile/mage

$ cd $project/test
$ mage build
$ mage -compile ./static-output
```

- default magefile.go

```go
// mage -init
// +build mage

package main

import (
	"fmt"
	"os"
	"os/exec"

	"github.com/magefile/mage/mg" // mg contains helpful utility functions, like Deps
)

// Default target to run when none is specified
// If not set, running mage will list available targets
// var Default = Build

// A build step that requires additional params, or platform specific steps for example
func Build() error {
//
	mg.Deps(Dep)
	return sh.RunV("go", "build", "-o", "project", "-ldflags="+ldflags(), "github.com/riboseyim/chilopod")
//
		//mg.Deps(Dep)
    //return sh.RunV("go", "build", "-o", "$MATTEL_REPO", "-ldflags="+ldflags(), "github.com/riboseyim/$MATTEL_REPO")
}

// A custom install step if you need your bin someplace other than go/bin
func Install() error {
	mg.Deps(Build)
	fmt.Println("Installing...")
	return os.Rename("./MyApp", "/usr/bin/MyApp")
}

// Manage your deps, or running package managers.
func InstallDeps() error {
	fmt.Println("Installing Deps...")
	cmd := exec.Command("go", "get", "github.com/stretchr/piglatin")
	return cmd.Run()
}

// Clean up after yourself
func Clean() {
	fmt.Println("Cleaning...")
	os.RemoveAll("MyApp")
}

```


- mage options

```c
mage [options] [target]

Mage is a make-like command runner.  See https://magefile.org for full docs.

Commands:
  -clean    clean out old generated binaries from CACHE_DIR
  -compile <string>
            output a static binary to the given path
  -init     create a starting template if no mage files exist
  -l        list mage targets in this directory
  -h        show this help
  -version  show version info for the mage binary

Options:
  -d <string>
          run magefiles in the given directory (default ".")
  -debug  turn on debug messages
  -h      show description of a target
  -f      force recreation of compiled magefile
  -keep   keep intermediate mage files around after running
  -gocmd <string>
          use the given go binary to compile the output (default: "go")
  -t <string>
          timeout in duration parsable format (e.g. 5m30s)
  -v      show verbose output when running mage targets
```



## 扩展阅读

#### [DevOps 漫谈系列](https://riboseyim.github.io/2016/07/28/DevOps/)
- [《凤凰项目》：从作坊到工厂的寓言故事](https://riboseyim.github.io/2018/04/10/DevOps-Phoenix/)
- [Kanban 看板管理实践](https://riboseyim.github.io/2017/08/06/TeamWork-Kanban/)
- [DevOps 漫谈：基础设施部署和配置管理](https://riboseyim.github.io/2018/03/26/DevOps-Deployment/)
- [Linux 容器安全的十重境界](https://riboseyim.github.io/2017/11/12/DevOps-Container-Security/)
- [工程师的自我修养：全英文技术学习实践](https://riboseyim.github.io/2017/06/27/Technology-English/)

#### [DevOps 实践的本质是文化](https://riboseyim.github.io/2018/03/29/DevOps-Culture/)
- 学习力－团队生命之根
- 带领团队翻译书籍
- Don’t make me think
- 凡是被很多人不断重复的好习惯，要将其自动化整合到工具

## 参考文献
- [Best Practices for Using Mage To Build Your Project  ](https://blog.gopheracademy.com/advent-2018/mage-best-practices/)
- [Golang官方依赖管理工具：dep](https://studygolang.com/articles/10589)
- [Building a CI/CD Bot with Slack and Kubernetes.](https://blog.gopheracademy.com/advent-2018/building-ci-cd-slack-bot/)
- [官方依赖管理工具：dep](https://gocn.vip/article/414)
- [初探 Go 的编译命令执行过程](https://halfrost.com/go_command/)
