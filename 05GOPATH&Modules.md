# GOPATH&Modules

## GOPATH

```
The GOPATH environment variable is a fundamental part of writing Go code without Go modules. It specifies the location of your workspace, and it defaults to $HOME/go. A GOPATH directory contains src, bin, and pkg directories. Your code is typically located in the $GOPATH/src directory.
```

GOPATH环境变量是编写没有Go Modules的Go代码的基本部分。它指定工作区的位置，默认值为$HOME/go。GOPATH目录包含src、bin和pkg目录。您的代码通常位于$GOPATH/src目录中。

```
If you are not familiar with Go and GOPATH, please first read about writing Go code with GOPATH.
```

如果您不熟悉Go和GOPATH，请先阅读有关[使用GOPATH编写Go代码](https://golang.org/doc/gopath_code.html#GOPATH)的内容。

### Overview

```
1、Check the value of GOPATH
Setting GOPATH
3、Different GOPATHs for different projects
4、Automatically inferring your GOPATH
5、Install tools to a separate GOBIN
6、Install tools to a separate GOPATH
```

### Check the value of `GOPATH`

```
First, it's useful to quickly check that you are using the right GOPATH. Two commands report the GOPATH value used by the VS Code Go extension: (1) Go: Current GOPATH, or (2) Go: Locate Configured Go Tools. Use either of these commands to check which GOPATH the extension is using.
```

首先，快速检查是否使用了正确的GOPATH非常有用。两个命令报告VSCode Go扩展使用的GOPATH值:(1)Go: Current GOPATH，或(2)Go: Locate Configured Go Tools。使用这两个命令中的任何一个来检查扩展使用的是哪个GOPATH。

```
If the GOPATH value is incorrect, see the details below on how to configure it.
```

如果GOPATH值不正确，请参阅下面关于如何配置它的详细信息。

### Setting `GOPATH`

```
If you have chosen not to use Go modules, you will need to configure your GOPATH. Modules have largely eliminated the need for a GOPATH, so if you're interested in using them, taking a look at the modules documentation for the VS Code Go extension.
```

如果选择不使用Go Modules，则需要配置GOPATH。Modules在很大程度上消除了对GOPATH的需求，因此如果您对使用它们感兴趣，请查看VSCode Go扩展的Modules文档。

```
Setting GOPATH is typically as simple as setting the environment variable once in your system's configuration. Take a look at the Setting GOPATH Wiki if you're unsure how to do this.
```

设置GOPATH通常与在系统配置中设置一次环境变量一样简单。如果您不确定如何这样做，请查看设置GOPATH Wiki。

```
By default, the extension uses the value of the environment variable GOPATH. If no such environment variable is set, the extension runs go env and uses the GOPATH reported by the go command.
```

默认情况下，扩展使用环境变量GOPATH的值。如果没有设置这样的环境变量，扩展将运行go env并使用go命令报告的GOPATH。

```
Note that, much like a `PATH` variable, `GOPATH` can contain multiple directory paths, separated by `:` or `;`. This allows you to set different `GOPATH`s for different projects.
```

注意，与“PATH”变量非常相似，“GOPATH”可以包含多个目录路径，由“:”或“;”分隔。这允许您为不同的项目设置不同的‘GOPATH’。

```
Still, there are a number of cases in which you might want a more complicated GOPATH set-up. Below, we explain more complex ways to configure and manage your GOPATH within the VS Code Go extension.
```

不过，在许多情况下，您可能需要更复杂的GOPATH设置。下面，我们将解释在VS代码Go扩展中配置和管理GOPATH的更复杂的方法。

### Different `GOPATH`s for different projects

```
Setting go.gopath in your user settings overrides the environment's GOPATH value.
```

设置用户设置中的go.gopath覆盖环境的gopath值。

```
Workspace settings override user settings, so you can use the go.gopath setting to set different GOPATHs for different projects. A GOPATH can also contain multiple directories, so this setting is not necessary to achieve this behavior.
```

工作区设置覆盖用户设置，所以您可以使用go.gopath为不同的项目设置不同的GOPATHS。一个GOPATH还可以包含多个目录，所以这个设置对于实现这个行为是不必要的。

### Automatically inferring your `GOPATH`

```
NOTE: This feature only works in GOPATH mode, not in module mode.
```

注意:此功能仅在GOPATH模式下工作，在Modules模式下无效。

```
The go.inferGopath setting overrides the go.gopath setting. If you set go.inferGopath to true, the extension will try to infer your GOPATH based on the workspace opened in VS Code. This is done by searching for a src directory in your workspace. The parent of this src directory is then added to your global GOPATH (go env GOPATH).
```

go.inferGopath设置覆盖go.gopath设置。如果你go.inferGopath为真，扩展将尝试根据在VSCode中打开的工作空间推断出GOPATH。这是通过在工作空间中搜索src目录来实现的。然后将这个src目录的父目录添加到全局GOPATH中(go env GOPATH)。

```
For example, say your global GOPATH is $HOME/go, but you are working in a repository with the following structure.
```

例如，假设您的全局GOPATH是$HOME/go，但是您正在使用以下结构的存储库中工作:

```
foo/
└── bar
    └── src
        └── main.go
```

```
If you open the foo directory as your workspace root in VS Code, "go.inferGopath" will set your GOPATH to $HOME/go:/path/to/foo/bar.
```

如果您在VSCode中打开foo目录作为工作区根目录，“go.inferGopath"将把你的GOPATH设置为$HOME/go:/path/to/foo/bar。

```
This setting is useful because it allows you to avoid setting the GOPATH in the workspace settings of each of your projects.
```

此设置非常有用，因为它允许您避免在每个项目的工作区设置中设置GOPATH。Install tools to a separate `GOBIN`

### Install tools to a separate GOBIN

```
If you switch frequently between GOPATHs, you may find that the extension prompts you to install tools for every GOPATH. You can resolve this by making sure your tool installations are on your PATH, or you can configure a separate directory for tool installation: GOBIN. This environment variable tells the go command where to install all binaries. Configure it by setting:
```

如果您经常在GOPATH之间切换，您可能会发现扩展提示您为每个GOPATH安装工具。您可以通过确保工具安装在您的路径上来解决这个问题，或者您可以为工具安装配置一个单独的目录:GOBIN。这个环境变量告诉go命令在哪里安装所有二进制文件。通过设置配置:

```
"go.toolsEnvVars": {
    "GOBIN": "path/to/gobin"
}
```

### Install tools to a separate `GOPATH`

```
NOTE: The following is only relevant if you are using a Go version that does not support Go modules, that is, any version of Go before 1.11.
```

注意:以下内容仅适用于不支持Go Modules的Go版本，即1.11之前的任何Go版本。

```
Before Go 1.11, the go get command installed tools and their source code to your GOPATH. Because this extension uses a lot of different tools, this causes clutter in your GOPATH. If you wish to reduce this clutter, you can have the extension install tools to a different location. This also addresses the issue described above, when switching GOPATHs forces you to reinstall Go tools.
```

在Go 1.11之前，Go get命令将工具及其源代码安装到您的GOPATH中。因为这个扩展使用了很多不同的工具，这导致了GOPATH中的混乱。如果您希望减少这种混乱，您可以将扩展安装工具放置到不同的位置。这也解决了上面描述的问题，当切换GOPATHs迫使你重新安装Go工具。

```
This can be done by setting "go.toolsGopath" to an alternate path, only for tool installations. After you configure this setting, be sure to run the Go: Install/Update Tools command so that the Go tools get installed to the provided location.
```

这可以通过设置“go.toolsGopath”到另一个路径来实现，仅用于工具安装。配置此设置后，请确保运行Go: Install/Update Tools命令，以便将Go工具安装到提供的位置。

```
The extension will fall back to your existing GOPATH if tools are not found in the go.toolsGopath directory.
```

如果在go.toolsGopath目录中找不到工具，该扩展将返回到现有的GOPATH。

# Modules

```
This extension mostly supports Go modules. However, certain features have not been ported to work with modules. The reason for this is that the Go team is developing a language server, gopls. This provides a comprehensive solution for Go modules support in all editors.
```

此扩展主要支持Go Modules。但是，某些特性还没有移植到Modules中。这样做的原因是Go团队正在开发一种语言服务器，gopls。这为所有编辑器中的Go Modules支持提供了一个全面的解决方案。

```
However, as described in the language server documentation, gopls is still in an alpha state, so it is not yet the default in VS Code Go. We understand that many users will not want to use alpha software, so this document describes how this extension works with Go modules, without the language server.
```

然而，正如在语言服务器文档中描述的那样，gopls仍然处于alpha状态，所以在VSCode Go中它还不是默认的。我们知道很多用户不想使用alpha软件，所以本文描述了这个扩展如何在没有语言服务器的情况下与Go Modules一起工作。

```
To learn how to use the language server with modules in VS Code, read our gopls documentation.
```

要学习如何在VSCode中使用带有Modules的语言服务器，请阅读我们的gopls文档（见04gopls，the language server）。

## Overview

- Missing features
  - Formatting
  - References and rename
- Performance

## Missing features

```
As mentioned above, not all Go tools have been ported to work with modules. **Many tools will never be ported, as they are being replaced by [`gopls`](https://github.com/golang/vscode-go/blob/master/docs/gopls.md).** [golang/go#24661](https://golang.org/issues/24661) contains up-to-date information about modules support in various Go tools.
```

如上所述，并不是所有Go工具都被移植到与Modules一起工作。**许多工具永远不会被移植，因为它们正在被[' gopls '](https://github.com/golang/vscode-go/blob/master/docs/gopls.md)锁取代**,[golang/go#24661](https://golang.org/issues/24661) 包含了各种go工具中modules支持的最新信息。

### Formatting

```
The default formatting tool, goreturns, has not been ported to work with modules. As a result, you will need to change your "go.formatTool" setting for formatting and import organization support. We recommend changing the value to "goimports" instead. Other options are also available, and you can read about them in the format tools documentation.
```

默认的格式化工具goreturns还没有被移植到模块中。因此，你需要改变你的“go.formatTool”设置为格式化和导入组织提供支持。我们建议将值改为“goimports”。还可以使用其他选项，您可以在format tools文档中了解它们。

### References and rename

```
The tools that provide these two features, guru and gorename, have not been updated for Go modules. Instead, they will be replaced by gopls.
```

提供这两个特性的工具guru和gorename还没有为Go Modules更新。相反，它们将被gopls所取代。

## Performance

```
Go modules introduced additional complexity to Go tooling. As a result, the performance of a number of tools is worse with modules enabled. Hopefully, you will not notice this too frequently.
```

Go Modules为Go工具引入了额外的复杂性。因此，在启用Modules时，许多工具的性能会更差。希望您不会经常注意到这一点。

```
One case in which you may notice this is autocompletion. The best solution to this is to switch to gopls, which makes use of an in-memory cache to provide results quickly.
```

您可能会注意到的一种情况是自动完成。***最好的解决方案是切换到gopls，它利用内存中的缓存快速提供结果。***



