# [How to write Go code](https://golang.org/doc/code.html)

## introduction

```
This document demonstrates the development of a simple Go package inside a module and introduces the go tool, the standard way to fetch, build, and install Go modules, packages, and commands.
```

本文演示了在模块中开发一个简单的Go包，并介绍了Go工具，这是获取、构建和安装Go模块、包和命令的标准方法。

```
Note: This document assumes that you are using Go 1.13 or later and the GO111MODULE environment variable is not set. If you are looking for the older, pre-modules version of this document, it is archived here.
```

注意:本文档假设您使用的是Go 1.13或更高版本，并且没有设置GO111MODULE环境变量。如果您正在寻找该文档的较旧的预模块版本，它在这里存档。

## Code organization

```
Go programs are organized into packages. A package is a collection of source files in the same directory that are compiled together. Functions, types, variables, and constants defined in one source file are visible to all other source files within the same package.
```

Go程序被组织成包。包是同一目录中一起编译的源文件的集合。定义在一个源文件中的函数、类型、变量和常量对同一包中的所有其他源文件都可见。

```
A repository contains one or more modules. A module is a collection of related Go packages that are released together. A Go repository typically contains only one module, located at the root of the repository. A file named go.mod there declares the module path: the import path prefix for all packages within the module. The module contains the packages in the directory containing its go.mod file as well as subdirectories of that directory, up to the next subdirectory containing another go.mod file (if any).
```

仓库包含一个或多个模块。模块是一起发布的相关Go包的集合。Go仓库通常只包含一个模块，位于存储库的根目录。一个名为go.mod的文件在这里声明模块路径：模块中所有包的导入路径前缀。模块包含其go.mod的目录中的包文件以及该目录的子目录，直到下一个包含另一个go.mod的子目录文件(如果有的话)。

```
Note that you don't need to publish your code to a remote repository before you can build it. A module can be defined locally without belonging to a repository. However, it's a good habit to organize your code as if you will publish it someday.
```

请注意，在构建代码之前不需要将代码发布到远程存储库。模块可以在本地定义，而不属于仓库。但是，组织代码时要像某天要发布一样，这是一个好习惯。

```
Each module's path not only serves as an import path prefix for its packages, but also indicates where the go command should look to download it. For example, in order to download the module golang.org/x/tools, the go command would consult the repository indicated by https://golang.org/x/tools (described more here).
```

每个模块的路径不仅作为其包的导入路径前缀，而且还指示go命令应该在何处下载它。例如，为了下载模块`golang.org/x/tools`, go命令将咨询https://golang.org/x/tools指示的仓库。（注意：如果配制了GOPROXY=https://goproxy.cn,https://mirrors.aliyun.com/goproxy/,https://goproxy.io,direct，go命令将咨询https://goproxycn/x/tools指示的仓库）

```
An import path is a string used to import a package. A package's import path is its module path joined with its subdirectory within the module. For example, the module `github.com/google/go-cmp` contains a package in the directory `cmp/`. That package's import path is `github.com/google/go-cmp/cmp`. Packages in the standard library do not have a module path prefix.
```

导入路径是用于导入包的字符串。包的导入路径是包的模块路径与模块中包的子目录的连接。例如，模块' github.com/google/go-cmp '包含目录' cmp/ '中的一个包。这个包的导入路径是' github.com/google/go-cmp/cmp '。标准库中的包没有模块路径前缀。

## Your first program

```
To compile and run a simple program, first choose a module path (we'll use example.com/user/hello) and create a go.mod file that declares it:
```

要编译和运行一个简单的程序，首先选择一个模块路径(我们将使用example.com/user/hello)并创建一个go.mod文件声明:

```
$ mkdir hello # Alternatively, clone it if it already exists in version control.
$ cd hello
$ go mod init example.com/user/hello
go: creating new go.mod: module example.com/user/hello
$ cat go.mod
module example.com/user/hello

go 1.14
$
```

```
The first statement in a Go source file must be package name. Executable commands must always use package main.
Next, create a file named hello.go inside that directory containing the following Go code:
```

Go源文件中的第一条语句必须是包名。可执行命令必须始终使用package main。

接下来，创建一个名为hello的文件。进入包含以下go代码的目录:

```go
package main

import "fmt"

func main() {
	fmt.Println("Hello, world.")
}
```

```
Now you can build and install that program with the `go` tool:
```

现在你可以用go工具构建和安装该程序:

```
$ go install example.com/user/hello
$
```

```
This command builds the hello command, producing an executable binary. It then installs that binary as $HOME/go/bin/hello (or, under Windows, %USERPROFILE%\go\bin\hello.exe).
```

这个命令构建hello命令，生成一个可执行的二进制文件。然后将该二进制文件安装为$HOME/go/bin/hello(或者，在Windows下，%USERPROFILE%\go\bin\hello.exe)。

```
The install directory is controlled by the GOPATH and GOBIN environment variables. If GOBIN is set, binaries are installed to that directory. If GOPATH is set, binaries are installed to the bin subdirectory of the first directory in the GOPATH list. Otherwise, binaries are installed to the bin subdirectory of the default GOPATH ($HOME/go or %USERPROFILE%\go).
```

安装目录由GOPATH和GOBIN环境变量控制。如果设置了GOBIN，则将二进制文件安装到该目录。如果设置了GOPATH，则将二进制文件安装到GOPATH列表中第一个目录的bin子目录中。否则，二进制文件将安装到默认GOPATH的bin子目录($HOME/go或%USERPROFILE%\go)。

```
You can use the go env command to portably set the default value for an environment variable for future go commands:
```

您可以使用go env命令灵活地为将来的go命令设置环境变量的默认值:

```
$ go env -w GOBIN=/somewhere/else/bin
$
```

```
To unset a variable previously set by go env -w, use go env -u:
```

使用env -u取消之前设置的变量:

```
$ go env -u GOBIN
$
```

```
Commands like `go install` apply within the context of the module containing the current working directory. If the working directory is not within the `example.com/user/hello` module, `go install` may fail.
```

像“go install”这样的命令在包含当前工作目录的模块上下文中应用。如果工作目录不在' example.com/user/hello '模块中，' go install '可能会失败。

```
For convenience, go commands accept paths relative to the working directory, and default to the package in the current working directory if no other path is given. So in our working directory, the following commands are all equivalent:
```

为了方便，go命令接受相对于工作目录的路径，如果没有给出其他路径，则默认接受当前工作目录中的包。所以在我们的工作目录下，下面的命令都是等效的:

```
$ go install example.com/user/hello
```

```
$ go install .
```

```
$ go install
```

```
Next, let's run the program to ensure it works. For added convenience, we'll add the install directory to our PATH to make running binaries easy:
```

接下来，运行程序以确保它能工作。为了更加方便，我们将安装目录添加到我们的路径，使运行二进制文件更容易:

```
# Windows users should consult https://github.com/golang/go/wiki/SettingGOPATH
# for setting %PATH%.
$ export PATH=$PATH:$(dirname $(go list -f '{{.Target}}' .))
$ hello
Hello, world.
$
```

```
If you're using a source control system, now would be a good time to initialize a repository, add the files, and commit your first change. Again, this step is optional: you do not need to use source control to write Go code.
```

如果您使用的是源代码控制系统，那么现在是初始化仓库、添加文件并提交第一个更改的好时机。同样，这个步骤是可选的:您不需要使用源代码控制来编写Go代码。

```
$ git init
Initialized empty Git repository in /home/user/hello/.git/
$ git add go.mod hello.go
$ git commit -m "initial commit"
[master (root-commit) 0b4507d] initial commit
 1 file changed, 7 insertion(+)
 create mode 100644 go.mod hello.go
$
```

```
The go command locates the repository containing a given module path by requesting a corresponding HTTPS URL and reading metadata embedded in the HTML response (see go help importpath). Many hosting services already provide that metadata for repositories containing Go code, so the easiest way to make your module available for others to use is usually to make its module path match the URL for the repository.
```

go命令通过请求相应的HTTPS URL并读取嵌入在HTML响应中的元数据来定位包含给定模块路径的仓库(参见go帮助importpath)。许多托管服务已经为包含Go代码的仓库提供了元数据，因此使您的模块可供其他人使用的最简单方法通常是使其模块路径与仓库的URL匹配。

## Importing packages from your module

```
Let's write a morestrings package and use it from the hello program. First, create a directory for the package named $HOME/hello/morestrings, and then a file named reverse.go in that directory with the following contents:
```

让我们编写一个morestrings包并在hello程序中使用它。首先，为名为$HOME/hello/morestrings的包创建一个目录，然后在该目录下创建一个名为reverse.go的文件，包含以下内容:

```GO
// Package morestrings implements additional functions to manipulate UTF-8
// encoded strings, beyond what is provided in the standard "strings" package.
package morestrings

// ReverseRunes returns its argument string reversed rune-wise left to right.
func ReverseRunes(s string) string {
	r := []rune(s)
	for i, j := 0, len(r)-1; i < len(r)/2; i, j = i+1, j-1 {
		r[i], r[j] = r[j], r[i]
	}
	return string(r)
}
```

```
Because our ReverseRunes function begins with an upper-case letter, it is exported, and can be used in other packages that import our morestrings package.

Let's test that the package compiles with go build:
```

因为我们的ReverseRunes函数以一个大写字母开始，所以它被导出，并且可以在导入我们的morestrings包的其他包中使用。

让我们测试一下这个包是否可以用go build编译:

```
$ cd $HOME/hello/morestrings
$ go build
$
```

```
This won't produce an output file. Instead it saves the compiled package in the local build cache.
```

这不会产生输出文件。相反，它将编译后的包保存在本地构建缓存中。

```
After confirming that the morestrings package builds, let's use it from the hello program. To do so, modify your original $HOME/hello/hello.go to use the morestrings package:
```

在确认构建了morestrings包之后，让我们从hello程序中使用它。为此，修改您原来的$HOME/hello/hello。去使用morestrings包:

```go
package main

import (
	"fmt"

	"example.com/user/hello/morestrings"
)

func main() {
	fmt.Println(morestrings.ReverseRunes("!oG ,olleH"))
}
```

```
Install the hello program:
```

安装hello程序:

```
$ go install example.com/user/hello
```

```
Running the new version of the program, you should see a new, reversed message:
```

运行该程序的新版本时，您应该看到一个新的反向消息:

```
$ hello
Hello, Go!
```

## Importing packages from remote modules

```
An import path can describe how to obtain the package source code using a revision control system such as Git or Mercurial. The go tool uses this property to automatically fetch packages from remote repositories. For instance, to use github.com/google/go-cmp/cmp in your program:
```

导入路径可以描述如何使用版本控制系统(如Git或Mercurial)获取包源代码。go工具使用此属性从远程仓库自动获取包。例如，在你的程序中使用github.com/google/go-cmp/cmp:

```
package main

import (
	"fmt"

	"example.com/user/hello/morestrings"
	"github.com/google/go-cmp/cmp"
)

func main() {
	fmt.Println(morestrings.ReverseRunes("!oG ,olleH"))
	fmt.Println(cmp.Diff("Hello World", "Hello Go"))
}
```

```
When you run commands like go install, go build, or go run, the go command will automatically download the remote module and record its version in your go.mod file:
```

当您运行诸如go install、go build或go run这样的命令时，go命令将自动下载远程模块并在您的go.mod文件中记录它的版本:

```
$ go install example.com/user/hello
go: finding module for package github.com/google/go-cmp/cmp
go: downloading github.com/google/go-cmp v0.4.0
go: found github.com/google/go-cmp/cmp in github.com/google/go-cmp v0.4.0
$ hello
Hello, Go!
  string(
- 	"Hello World",
+ 	"Hello Go",
  )
$ cat go.mod
module example.com/user/hello

go 1.14

require github.com/google/go-cmp v0.4.0
$
```

```
Module dependencies are automatically downloaded to the pkg/mod subdirectory of the directory indicated by the GOPATH environment variable. The downloaded contents for a given version of a module are shared among all other modules that require that version, so the go command marks those files and directories as read-only. To remove all downloaded modules, you can pass the -modcache flag to go clean:
```

Module依赖项自动下载到GOPATH环境变量指定的目录的pkg/mod子目录。模块的给定版本的下载内容在需要该版本的所有其他模块之间共享，因此go命令将这些文件和目录标记为只读。要删除所有下载的模块，你可以通过-modcache标志来清除:

```
$ go clean -modcache
$
```

## Testing

```
Go has a lightweight test framework composed of the go test command and the testing package.

You write a test by creating a file with a name ending in _test.go that contains functions named TestXXX with signature func (t *testing.T). The test framework runs each such function; if the function calls a failure function such as t.Error or t.Fail, the test is considered to have failed.
```

Go有一个轻量级的测试框架，由Go测试命令和测试包组成。

通过创建一个名称以_test.go结尾的文件来编写含有带func (t *test . t)签名的函数TestXXX的测试包。测试框架运行每个这样的函数，如果该函数调用了一个失败函数，如t.Error或t.Fail，则认为测试失败。

```
Add a test to the morestrings package by creating the file $HOME/hello/morestrings/reverse_test.go containing the following Go code.
```

通过创建一个包含以下Go代码的$HOME/hello/morestrings/reverse_test.go文件向morestrings包添加一个测试包：

```go
package morestrings

import "testing"

func TestReverseRunes(t *testing.T) {
	cases := []struct {
		in, want string
	}{
		{"Hello, world", "dlrow ,olleH"},
		{"Hello, 世界", "界世 ,olleH"},
		{"", ""},
	}
	for _, c := range cases {
		got := ReverseRunes(c.in)
		if got != c.want {
			t.Errorf("ReverseRunes(%q) == %q, want %q", c.in, got, c.want)
		}
	}
}
```

```\
Then run the test with `go test`:
```

然后用“go test”运行测试:

```
$ go test
PASS
ok  	example.com/user/morestrings 0.165s
$
```

```
Run go help test and see the testing package documentation for more detail.
```

运行go help test查看测试包文档以了解更多细节。

## What's next

```
Subscribe to the golang-announce mailing list to be notified when a new stable version of Go is released.

See Effective Go for tips on writing clear, idiomatic Go code.

Take A Tour of Go to learn the language proper.

Visit the documentation page for a set of in-depth articles about the Go language and its libraries and tools.
```

订阅[golang-announce](https://groups.google.com/group/golang-announce)邮件列表，当新的Go稳定版本发布时将收到通知。

参见[Effective Go](https://golang.org/doc/effective_go.html)，了解如何编写清晰、习惯的Go代码。

[游览一下](https://tour.go-zh.org/)，好好学习一门语言。

访问[文档页面](https://golang.org/doc/#articles)，获得一组关于Go语言及其库和工具的深入文章。

## Getting help

```
For real-time help, ask the helpful gophers in the community-run gophers Slack server (grab an invite here).

The official mailing list for discussion of the Go language is Go Nuts.

Report bugs using the Go issue tracker.
```

要获得实时帮助，可以在社区运行的gophers Slack服务器上向有帮助的gophers 咨询(点击这里获取邀请)。

讨论Go语言的官方邮件列表是Go Nuts。

使用Go问题跟踪器报告错误。