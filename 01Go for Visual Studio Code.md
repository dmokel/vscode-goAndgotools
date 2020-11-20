# Go for Visual Studio Code

[![Slack](https://img.shields.io/badge/slack-gophers-green.svg?style=flat)](https://gophers.slack.com/messages/vscode/)

<!--TODO: We should add a badge for the build status or link to the build dashboard.-->

This extension provides rich language support for the [Go programming language](https://golang.org/) in VS Code.

```
这个扩展在VS Code中为Go programming language(https://golang.org/)提供了丰富的语言支持。
```

Take a look at the [Changelog](https://github.com/golang/vscode-go/blob/master/CHANGELOG.md) to learn about new features.

> This is the **new** home for the VS Code Go extension. We just migrated from [Microsoft/vscode-go](https://github.com/Microsoft/vscode-go). Learn more about our move on the [Go blog](https://blog.golang.org/vscode-go).

```
查看[Changelog](https://github.com/golang/vscode-go/blob/master/CHANGELOG.md)了解新特性。

这是VSCode Go扩展的**new** home。我们刚刚从[Microsoft/vscode-go](https://github.com/Microsoft/vscode-go)迁移过来。了解更多关于我们的行动[Go博客](https://blog.golang.org/vscode-go)。
```

## Overview

* [Getting started](#getting-started)
* [Support for Go modules](#support-for-go-modules)
* [Features](#features)
  * [Debugging](#debugging)
* [Customization](#customization)
  * [Linter](#linter)
  * [GOPATH](#gopath)
* [Language server](#language-server)
* [Troubleshooting](https://github.com/golang/vscode-go/blob/master/docs/troubleshooting.md)
* [Ask for help](#ask-for-help)
* [Preview version](#preview-version)
* [Contributing](#contributing)

## Getting started

Welcome! Whether you are new to Go or an experienced Go developer, we hope this extension will fit your needs and enhance your development experience.

```
欢迎光临!无论你是新手Go开发人员或有经验的Go开发人员，我们希望这个扩展将适合您的需要，并提高您的开发经验。
```

### Install Go

Before you start coding, make sure that you have already installed Go, as explained in the [Go installation guide](https://golang.org/doc/install).

```
在开始编码之前，请确保您已经安装了Go，如[Go安装指南](https://golang.org/doc/install)中所解释的那样。
```

If you are unsure whether you have installed Go, open the Command Palette in VS Code (Ctrl+Shift+P) and run the [`Go: Locate Configured Go Tools`](https://github.com/golang/vscode-go/blob/master/docs/commands.md#go-locate-configured-go-tools) command. If the `GOROOT` output is empty, you are missing a Go installation. For help installing Go, ask a question on the `#newbies` [Gophers Slack] channel.

```
如果您不确定是否安装了Go，请在VSCode中打开命令面板(Ctrl+Shift+P)并运行[' Go: Locate Configured Go Tools '](https://github.com/golang/vscode-go/blob/master/docs/commands.md# Go - locate-configuredgo - Tools)命令。如果' GOROOT '输出为空，则丢失了Go安装。如果需要安装Go的帮助，可以在“#newbies”(Gophers Slack)频道问一个问题。
```

### Set up your environment

Read about [Go code organization](https://golang.org/doc/code.html) to learn how to configure your environment. This extension works in both [GOPATH](https://github.com/golang/vscode-go/blob/master/docs/gopath.md) and [module](https://github.com/golang/vscode-go/blob/master/docs/modules.md) modes. We suggest using modules, as they are quickly becoming the new standard in the Go community.

```
了解[Go code organization]（03go modules,How to write Go Code）以了解如何配置您的环境。该扩展在[GOPATH])和[module]模式下工作。我们建议使用Modules，因为Modules正在迅速成为Go社区的新标准。
```

Here are some additional resources for learning about how to set up your Go project:

```
这里有一些额外的资源来学习如何设置你的Go项目:
```

* [Using Go modules](https://blog.golang.org/using-go-modules)
* [Modules wiki](https://github.com/golang/go/wiki/Modules)
* [GOPATH](https://golang.org/cmd/go/#hdr-GOPATH_environment_variable)

**NOTE: If you are using modules, we recommend using the Go [language server](#language-server), which is explained below.**

```
**注意:如果您正在使用模块，我们建议使用Go [language server](#language-server)，它将在下面解释。**
```

More advanced users may be interested in using different `GOPATH`s or Go versions per-project. You can learn about the different `GOPATH` manipulation options in the [`GOPATH` documentation](https://github.com/golang/vscode-go/blob/master/docs/gopath.md). Take a look at the other [customization](#customization) options as well.

```
更高级的用户可能会对每个项目使用不同的“GOPATH”或Go版本感兴趣。您可以在[' GOPATH '文档](https://github.com/golang/vscode- go/blob/master/docs/gopath.md)中了解不同的' GOPATH '操作选项。也可以看看其他的[定制](#定制)选项。
```

### Install the extension

If you haven't already done so, install and open [Visual Studio Code](https://code.visualstudio.com). Navigate to the Extensions pane (Ctrl+Shift+X). Search for "Go" and install this extension (the publisher ID is `golang.Go`).

```
如果您还没有安装并打开[Visual Studio Code](https://code.visualstudio.com)。导航到扩展窗格(Ctrl+Shift+X)。搜索"Go"并安装此扩展(发布者ID为' golang.Go ')。
```

### Activate the Go extension

To activate the extension, open any directory or workspace containing Go code.

```
要激活扩展，请打开包含Go代码的任何目录或工作区。
```

You should immediately see a prompt in the bottom-right corner of your screen titled `Analysis Tools Missing`. This extension relies on a suite of [command-line tools](https://github.com/golang/vscode-go/blob/master/docs/tools.md), which must be installed separately. Accept the prompt, or use the [`Go: Install/Update Tools`](https://github.com/golang/vscode-go/blob/master/docs/commands.md#go-installupdate-tools) command to pick which tools you would like to install.

```
你应该会在屏幕的右下角看到一个标题为“Analysis Tools Missing”的提示。该扩展依赖于一套必须单独安装的[命令行工具](https://github.com/golang/vscode-go/blob/master/docs/tools.md)。接受提示，或者使用[' Go: Install/Update Tools '](https://github.com/golang/vscode-go/blob/master/docs/commands.md# Go -installupdate-tools)命令来选择您想要安装的工具。
```

If you see an error that looks like `command Go: Install/Update Tools not found`, it means that the extension has failed to activate and register its commands. Please uninstall and then reinstall the extension.

```
如果你看到一个错误，看起来像' command Go: Install/Update Tools not found '，这意味着扩展没有激活和注册它的命令。请卸载，然后重新安装扩展名。
```

When the extension is active, you should see the [Go status bar](https://github.com/golang/vscode-go/blob/master/docs/ui.md) in the bottom left corner.

```
当扩展激活时，您应该会在左下角看到[Go状态栏](https://github.com/golang/vscode-go/blob/master/docs/ui.md)。
```

### Start coding

You're ready to Go!

Be sure to learn more about the many [features](#features) of this extension, as well as how to [customize](#customization) them. Take a look at [Troubleshooting](https://github.com/golang/vscode-go/blob/master/docs/troubleshooting.md) and [Help](#ask-for-help) for further guidance.

```
一定要了解更多关于这个扩展的许多[特性](#特性)，以及如何[定制](#定制)它们。查看[Troubleshooting](https://github.com/golang/vscodego/blob/master/docs/troubleshooting.md)和[Help](#ask-for-help)以获得进一步的指导。
```

## Support for Go modules

[Go modules](https://blog.golang.org/using-go-modules) have added a lot of complexity to the way that most tools and features are built for Go. Some, but not all, [features](https://github.com/golang/vscode-go/blob/master/docs/features.md) of this extension have been updated to work with Go modules. Some features may also be slower in module mode. The [features documentation](https://github.com/golang/vscode-go/blob/master/docs/features.md) contains more specific details.

**In general, we recommend using [`gopls`, the official Go language server](https://golang.org/s/gopls), if you are using modules.** Read more [below](#language-server) and in the [`gopls` documentation](https://github.com/golang/vscode-go/blob/master/docs/gopls.md).

## [Features](https://github.com/golang/vscode-go/blob/master/docs/features.md)

This extension has a wide range of features, including [Intellisense](https://github.com/golang/vscode-go/blob/master/docs/features.md#intellisense), [code navigation](https://github.com/golang/vscode-go/blob/master/docs/features.md#code-navigation), and [code editing](https://github.com/golang/vscode-go/blob/master/docs/features.md#code-editing) support. It also shows build, vet, and lint [diagnostics](https://github.com/golang/vscode-go/blob/master/docs/features.md#diagnostics) as you work and provides enhanced support for [testing](https://github.com/golang/vscode-go/blob/master/docs/features.md##run-and-test-in-the-editor) and [debugging](#debugging) your programs. For more detail, see the [full feature breakdown](https://github.com/golang/vscode-go/blob/master/docs/features.md).

In addition to integrated editing features, the extension also provides several commands for working with Go files. You can access any of these by opening the Command Palette (Ctrl+Shift+P) and typing in the name of the command. See the [full list of commands](https://github.com/golang/vscode-go/blob/master/docs/commands.md#detailed-list) provided by the extension.

The majority of the extension's functionality comes from command-line tools. If you're experiencing an issue with a specific feature, you may want to investigate the underlying tool. You can do this by taking a look at the [full list of tools used by this extension](https://github.com/golang/vscode-go/blob/master/docs/tools.md).

### [Debugging](https://github.com/golang/vscode-go/blob/master/docs/debugging.md)

Debugging is a major feature offered by this extension. For a comprehensive overview of how to debug your Go programs, please see the [debugging guide](https://github.com/golang/vscode-go/blob/master/docs/debugging.md).

## Customization

This extension needs no configuration; it works out of the box. However, you may wish to modify settings to adjust your experience.

Many of the features are configurable to your preference. A few common modifications are mentioned below, but take a look at the [full list of settings](https://github.com/golang/vscode-go/blob/master/docs/settings.md) for an overview.

### [Linter](https://github.com/golang/vscode-go/blob/master/docs/tools.md#diagnostics)

A commonly customized feature is the linter, which is a tool used to provide coding style feedback and suggestions. By default, this extension uses the official [`golint`].

However, you are welcome to use more advanced options like [`staticcheck`](https://pkg.go.dev/honnef.co/go/tools/staticcheck?tab=overview), [`golangci-lint`](https://golangci-lint.run/), or [`revive`](https://pkg.go.dev/github.com/mgechev/revive?tab=overview). This can be configured via the [`"go.lintTool"`](https://github.com/golang/vscode-go/blob/master/docs/settings.md#go.lintTool) setting, and the different options are explained more thoroughly in the [list of diagnostic tools](https://github.com/golang/vscode-go/blob/master/docs/tools.md#diagnostics).

### [GOPATH](https://github.com/golang/vscode-go/blob/master/docs/gopath.md)

Advanced users may want to set different `GOPATH`s for different projects or install the Go tools to a different `GOPATH`. This is possible and explained in the [`GOPATH documentation`](https://github.com/golang/vscode-go/blob/master/docs/gopath.md).

## [Language Server](https://github.com/golang/vscode-go/blob/master/docs/gopls.md)

In the default mode, the Go extension relies upon a suite of [command-line tools](https://github.com/golang/vscode-go/blob/master/docs/tools.md). A new alternative is to use a [single language server](https://langserver.org/), which provides language features through the [Language Server Protocol](https://microsoft.github.io/language-server-protocol/).

The Go team at Google has developed [`gopls`](https://github.com/golang/vscode-go/blob/master/docs/gopls.md), which is the official Go language server. It is currently in an alpha state and under active development.

[`gopls`] is recommended for projects that use Go modules.

To opt-in to the language server, set [`"go.useLanguageServer"`](https://github.com/golang/vscode-go/blob/master/docs/settings.md#go.useLanguageServer) to `true` in your settings. You should then be prompted to install [`gopls`]. If you are not prompted, you can install `gopls` manually by running the [`Go: Install/Update Tools`](https://github.com/golang/vscode-go/blob/master/docs/commands.md#go-installupdate-tools) command and selecting `gopls`.

For more information, see the [`gopls` documentation](https://github.com/golang/vscode-go/blob/master/docs/gopls.md).

## Ask for help

If you're having issues with this extension, please reach out to us by [filing an issue](https://github.com/golang/vscode-go/issues/new/choose) or asking a question on the [Gophers Slack]. We hang out in the `#vscode` channel!

Take a look at [learn.go.dev](https://learn.go.dev) and [golang.org/help](https://golang.org/help) for additional guidance.

## [Preview version](https://github.com/golang/vscode-go/blob/master/docs/nightly.md)

If you'd like to get early access to new features and bug fixes, you can use the nightly build of this extension. Learn how to install it in by reading the [Go Nightly documentation](https://github.com/golang/vscode-go/blob/master/docs/nightly.md).

## [Contributing](https://github.com/golang/vscode-go/blob/master/docs/contributing.md)

We welcome your contributions and thank you for working to improve the Go development experience in VS Code. If you would like to help work on the VS Code Go extension, please see our [contribution guide](https://github.com/golang/vscode-go/blob/master/docs/contributing.md). It explains how to build and run the extension locally, and it describes the process of sending a contribution.

## [Code of Conduct](https://github.com/golang/vscode-go/blob/master/CODE_OF_CONDUCT.md)

This project follows the [Go Community Code of Conduct](https://golang.org/conduct). If you encounter an issue, please mail conduct@golang.org.

## [License](https://github.com/golang/vscode-go/blob/master/LICENSE)

[MIT](https://github.com/golang/vscode-go/blob/master/LICENSE)

[`golint`]: https://pkg.go.dev/golang.org/x/lint/golint?tab=overview
[Gophers Slack]: https://gophers.slack.com/
[`gopls`]: https://golang.org/s/gopls