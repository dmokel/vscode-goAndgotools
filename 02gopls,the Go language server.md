# gopls,the Go language server

[gopls document](https://github.com/golang/tools/tree/master/gopls/doc)

```
gopls is the official Go language server developed by the Go team. It was developed in response to the release of Go modules, and it is the recommended approach when working with Go modules in VS Code.

gopls is currently in an alpha state, so it is not enabled by default. Please note that gopls only supports Go versions above 1.12.

gopls has its own documentation pages, and they should be treated as the source of truth for how to use gopls in VS Code.
```

gopls是由Go团队开发的官方Go语言服务器。它是为了响应Go Modules的发布而开发的，当在VS代码中使用Go modules时，推荐使用这种方法。

gopls目前处于alpha状态，所以默认没有启用。请注意，gopls只支持1.12以上的Go版本。

gopls有自己的文档页面，它们应该被视为如何在VS代码中使用gopls的真实来源。

## Overview

* [Background](https://github.com/golang/vscode-go/blob/master/docs/gopls.md#background)
* Enable the language server
  - [Automatic updates](https://github.com/golang/vscode-go/blob/master/docs/gopls.md#automatic-updates)
* Configuration
  - [Ignored settings](https://github.com/golang/vscode-go/blob/master/docs/gopls.md#ignored-settings)
  - [`gopls` settings block](https://github.com/golang/vscode-go/blob/master/docs/gopls.md#gopls-settings-block)
* [Troubleshooting](https://github.com/golang/tools/blob/master/gopls/doc/troubleshooting.md)
* [Additional resources](https://github.com/golang/vscode-go/blob/master/docs/gopls.md#additional-resources)

## Background

```
This extension functions by shelling out to a number of command-line tools. This introduces complexity, as each feature is provided by a different tool. Language servers enable all editors to support all programming languages without these individualized tools. They also provide speed improvements, as they can cache and reuse results.

gopls is the official Go language server. Using gopls will enable the VS Code Go extension to provide high-quality Go support as the language evolves.

To learn more about the context behind gopls, you can watch the Go pls, stop breaking my editor talk at GopherCon 2019.
```

这个扩展通过使用许多命令行工具来实现。这引入了复杂性，因为每个特性都由不同的工具提供。语言服务器允许所有编辑器支持所有编程语言，而不需要这些个性化的工具。它们还提供了速度改进，因为它们可以缓存和重用结果。

gopls是官方的Go语言服务器。随着语言的发展，使用gopls将使VS Code Go扩展能够提供高质量的Go支持。

要了解更多关于gopls背后的背景，你可以观看[](https://www.youtube.com/watch?v=EFJfdWzBHwE) talk at 2019年GopherCon。

## Enable the language server

```
To start using the language server, set "go.useLanguageServer": true in your VS Code Go settings.
```

要开始使用语言服务器，请在VS代码Go设置`"go.useLanguageServer": true,`

```
You should see a prompt to install gopls. If you do not see a prompt, please run the Go: Install/Update Tools command and select gopls.
```

您应该会看到一个安装gopls的提示。如果没有提示，请运行Go: Install/Update Tools命令并选择gopls。

### Automatic updates

```
The gopls team releases new versions of gopls approximately once a month (release notes). The Go extension will automatically detect that a new version has been released, and a pop-up will appear prompting you to update.
```

gopls团队大约每月发布一次新的gopls版本(发布说明)。Go扩展会自动检测到一个新版本已经发布，并会出现一个弹出提示您进行更新。

```
If you would like to opt-out of these updates, set "go.useGoProxyToCheckForToolUpdates" to false.
```

如果你想退出这些更新，设置`"go.useGoProxyToCheckForToolUpdates" `为false。

默认为true。

## Configuration

```
There are a number of VS Code Go settings for controlling the language server.
```

有许多VS Code Go设置用于控制语言服务器。

```
"go.languageServerExperimentalFeatures" allows you to disable certain features.
	"diagnostics": false disables diagnostic warnings from gopls. You might want to disable these if you don't like the diagnostics changing as you type.
	"documentLink": false disables document links. The reason to disable these is explained in golang/go#39065: the Ctrl+Click shortcut for clicking on a link collides with the Ctrl+Click shortcut for go-to-definition.
```

`"go.languageServerExperimentalFeatures" `允许您禁用某些功能。

* `"diagnostics:false"`禁用来自gopls的诊断警告。如果您不希望诊断信息在键入时发生更改，可能需要禁用这些选项。
* `"documentLink:false"`禁用文档链接。禁用这些功能的原因在golang/go#39065中有解释:用于点击链接的Ctrl+Click快捷键与用于点击到定义的Ctrl+Click快捷键发生碰撞。

```
"go.languageServerFlags" allows you to pass flags to the gopls process.
	The -rpc.trace flag enables verbose debug logging.
```

`"go.languageServerFlags"`允许您向gopls进程传递标志。`-rpc.trace`启用详细的调试日志记录。

### Ignored settings

```
A number of the extension's settings are not passed in to gopls. We are working on unifying all of the settings, but some may still be ignored. These include:
```

扩展的许多设置没有传递到gopls。我们正在努力统一所有的设置，但仍有一些可能被忽略。这些包括:

```
"go.buildFlags"
```

```
These configurations can be passed to gopls via your environment or the gopls.env setting. Learn more in the gopls VS Code documentation.
```

这些配置可以通过您的环境或gopls.env设置gopls传递给。在[gopls VS Code 文档](https://github.com/golang/tools/blob/master/gopls/doc/vscode.md#build-tags)中了解更多信息。

### `gopls` settings block

```
gopls exposes much more configuration. However, because gopls is in a state of rapid development and change, these settings change frequently. Therefore, we have not yet built these settings into the Go extension.
```

gopls公开了更多的配置。然而，由于gopls处于快速发展和变化的状态，这些设置变化频繁。因此，我们还没有将这些设置构建到Go扩展中。

```
As shown in the gopls VS Code user guide, you can still configure these settings through VS Code by adding a "gopls" block to your settings.json file (Command Palette -> Preferences: Open Settings (JSON)). You will see an Unknown Configuration Setting warning, but the settings will still work. Add any settings there, and gopls will warn you if they are incorrect, unknown, or deprecated.
```

正如在[gopls VS Code 用户指南](https://github.com/golang/tools/blob/master/gopls/doc/vscode.md)（同gopls VS Code文档）中所示，您仍然可以通过在设置中添加一个“gopls”块来通过VS代码配置这些设置。json文件(命令面板->首选项:打开设置(json))。您将看到一个未知的配置设置警告，但设置仍将工作。在那里添加任何设置，如果设置不正确、未知或弃用，gopls将警告您。

```
A full list of gopls settings is available in the gopls settings documentation.
```

在[gopls设置文档(](https://github.com/golang/tools/blob/master/gopls/doc/settings.md)中有完整的gopls设置列表。

## [Troubleshooting](https://github.com/golang/tools/blob/master/gopls/doc/troubleshooting.md)

```
If you suspect that gopls is crashing or not working correctly, please follow the troubleshooting steps below.
```

如果您怀疑gopls正在崩溃或不能正常工作，请遵循下面的故障排除步骤。

```
If gopls is using too much memory, please follow the steps under Memory usage.
```

如果gopls使用太多内存，请按照内存使用的步骤操作。

### Steps

```
1、Make sure your gopls is up to date.
2、Check the known issues.
3、Report the issue.
```

1、确保你的gopls是最新的。

2、检查已知的问题。

3、报告这个问题。

### 文件的问题

```
You can use:

1、Your editor's bug submission integration (if available). For instance, :GoReportGitHubIssue in vim-go.
2、gopls bug on the command line.
3、The Go issue tracker.
```

你可以使用：

1、编辑器的错误提交集成(如果可用)。例如:vim-go中的GoReportGitHubIssue。

2、命令行上的gopls错误。

3、Go问题跟踪器。

```
Along with an explanation of the issue, please share the information listed here:
```

除了对这个问题的解释，请分享这里列出的信息:

```
1、Your editor and any settings you have configured (for example, your VSCode settings.json file).
2、A sample program that reproduces the issue, if possible.
3、The output of gopls version on the command line.
4、The output of gopls -rpc.trace -v check /path/to/file.go.
5、gopls logs from when the issue occurred, as well as a timestamp for when the issue began to occur. See the instructions for information on how to capture gopls logs.
```

1、您的编辑器和配置的任何设置(例如，您的VSCode设置)。json文件)。

2、如果可能的话，一个重现问题的示例程序。

3、命令行上的gopls版本的输出。

4、gopls -rpc -v check /path/to/file.go的输出

5、gopls记录问题发生的时间，以及问题开始发生的时间戳。有关如何捕获gopls日志的信息，请参阅说明。

```
Much of this information is filled in for you if you use gopls bug to file the issue.
```

如果您使用gopls bug来归档问题，那么这些信息中的大部分将被填写。

#### Capturing logs 获取日志

##### VS Code

```
For VSCode users, the gopls log can be found by navigating to View -> Output (or Ctrl+K Ctrl+H). There will be a drop-down menu titled Tasks in the top-right corner. Select the gopls (server) item, which will contain the gopls logs.
```

对于VSCode用户，可以通过导航到View -> Output(或Ctrl+K、Ctrl+H)找到gopls日志。在右上角会有一个名为任务的下拉菜单，选择gopls(服务器)项，它将包含gopls日志。

```
To increase the level of detail in your logs, add the following to your VS Code settings:
```

为了增加你的日志的详细级别，添加以下到你的VS代码设置:

```json
"go.languageServerFlags": [
  "-rpc.trace"
]
```

```
To start a debug server that will allow you to see profiles and memory usage, add the following to your VS Code settings:
```

要启动一个调试服务器，将允许你看到配置文件和内存使用，添加以下到你的VS代码设置:

```
"go.languageServerFlags": [
  "serve",
  "-rpc.trace",
  "--debug=localhost:6060",
]
```

```
You will then be able to view debug information by navigating to `localhost:6060`.
```

然后，您将能够通过导航到' localhost:6060 '来查看调试信息。

##### Other editors

```
For other editors, you may have to directly pass a -logfile flag to gopls.
```

对于其他编辑器，您可能必须直接将-logfile标志传递给gopls。

```
To increase the level of detail in your logs, start gopls with the -rpc.trace flag. To start a debug server that will allow you to see profiles and memory usage, start gopls with serve --debug=localhost:6060. You will then be able to view debug information by navigating to localhost:6060.
```

要提高日志中的详细级别，可以使用-rpc.trace启动gopls。要启动允许您查看概要文件和内存使用情况的调试服务器，请使用serve——debug=localhost:6060启动gopls。然后您就可以通过导航到localhost:6060来查看调试信息。

```
If you are unsure of how to pass a flag to gopls through your editor, please see the documentation for your editor.
```

如果您不确定如何通过编辑器将标志传递给gopls，请参阅编辑器的文档。

#### Restart your editor

```
Once you have filed an issue, you can then try to restart your gopls instance by restarting your editor. In many cases, this will correct the problem. In VSCode, the easiest way to restart the language server is by opening the command palette (Ctrl + Shift + P) and selecting "Go: Restart Language Server". You can also reload the VSCode instance by selecting "Developer: Reload Window".
```

提交问题后，可以通过重新启动编辑器尝试重新启动gopls实例。在许多情况下，这将纠正问题。在VSCode中，重启语言服务器最简单的方法是打开命令面板(Ctrl + Shift + P)并选择`“Go: restart language server”`。您还可以通过选择`“Developer: reload Window”`来重新加载VSCode实例。

### Memory usage

```
gopls automatically writes out memory debug information when your usage exceeds 1GB. This information can be found in your temporary directory with names like gopls.1234-5GiB-withnames.zip. On Windows, your temporary directory will be located at %TMP%, and on Unixes, it will be $TMPDIR, which is usually /tmp. Please create a new issue with your editor settings and memory debug information attached. If you are uncomfortable sharing the package names of your code, you can share the -nonames zip instead.
```

当您的使用量超过1GB时，gopls自动写出内存调试信息。这些信息可以在您的临时目录中找到，该目录的名称为:gopls.1234- 5gb -withnames.zip。在Windows上，您的临时目录将位于%TMP%，在unix上，它将是$TMPDIR，通常是/ TMP。请创建一个新问题，附带您的编辑器设置和内存调试信息。如果您不愿意共享代码的包名，那么可以共享-nonames zip。

## Additional resources

```
If you encounter an issue while using gopls, take a look at these resources:
```

如果您在使用gopls时遇到问题，请查看以下资源:

* [gopls VS Code user guide/gopls VS Code documentation](https://github.com/golang/tools/blob/master/gopls/doc/vscode.md)
* [Known issues](https://github.com/golang/tools/blob/master/gopls/doc/status.md#known-issues)
* [Troubleshooting](https://github.com/golang/tools/blob/master/gopls/doc/troubleshooting.md)
* [`gopls` issue tracker](https://github.com/golang/go/issues?q=is%3Aissue+is%3Aopen+label%3Agopls)

```
If you are unable to resolve your issue, please ask for help. Make sure to mention that you are using gopls. Here's how to ask for guidance:
```

如果你不能解决你的问题，请寻求帮助。一定要提到您正在使用gopls。以下是如何寻求指导:

```
1、File a VS Code Go issue. If it is a bug in gopls, we will transfer your issue to the gopls issue tracker.
2、File a gopls issue directly.
3、Ask a question in the #gopls channel on Gophers Slack.
4、Ask a question in the #vscode channel on Gophers Slack.
```

* [File a VS Code Go issue](https://github.com/golang/vscode-go/issues/new/choose). 如果是gopls的错误，我们将把你的问题转移到gopls问题跟踪器。
* [File a `gopls` issue](https://github.com/golang/go/issues/new) directly.
* 在 [Gophers Slack](https://github.com/gophers.slack.com/)的#gopls频道问问题。
* 在[Gophers Slack](https://github.com/gophers.slack.com/)的#vscode频道问一个问题。

### gopls - vscode

```
Use the VSCode-Go plugin, with the following configuration:
```

使用VSCode-Go插件，配置如下:

```json
"go.useLanguageServer": true,
"[go]": {
    "editor.formatOnSave": true,
    "editor.codeActionsOnSave": {
        "source.organizeImports": true,
    },
    // Optional: Disable snippets, as they conflict with completion ranking.
    "editor.snippetSuggestions": "none",
},
"[go.mod]": {
    "editor.formatOnSave": true,
    "editor.codeActionsOnSave": {
        "source.organizeImports": true,
    },
},
"gopls": {
     // Add parameter placeholders when completing a function.
    "usePlaceholders": true,

    // If true, enable additional analyses with staticcheck.
    // Warning: This will significantly increase memory usage.
    "staticcheck": false,
}
```

```
VSCode will complain about the "gopls" settings, but they will still work. Once we have a consistent set of settings, we will make the changes in the VSCode plugin necessary to remove the errors.
```

VSCode会抱怨“gopls”设置，但它们仍然可以工作。一旦我们有了一组一致的设置，我们将在VSCode插件中进行必要的更改以删除错误。

```
If you encounter problems with import organization, please try setting a higher code action timeout (any value greater than 750ms), for example:
```

如果您在导入组织方面遇到问题，请尝试设置更高的代码操作超时(任何大于750ms的值)，例如:

```json
"[go]": {
  "editor.codeActionsOnSaveTimeout": 3000
}
```

```
To enable more detailed debug information, add the following to your VSCode settings:
```

要启用更详细的调试信息，请在您的VSCode设置中添加以下内容:

```json
"go.languageServerFlags": [
    "-rpc.trace", // for more detailed debug logging
    "serve",
    "--debug=localhost:6060", // to investigate memory usage, see profiles
],
```

```
See the section on [command line](https://github.com/golang/tools/blob/master/gopls/doc/command-line.md) arguments for more information about what these do, along with other things like `--logfile=auto` that you might want to use.
```

有关命令行参数的详细信息，请参阅[命令行参数](https://github.com/golang/tools/blob/master/gopls/doc/command-line.md)，以及您可能想使用的——logfile=auto之类的参数。

```
You can disable features through the "go.languageServerExperimentalFeatures" section of the config. An example of a feature you may want to disable is "documentLink", which opens pkg.go.dev links when you click on import statements in your file.
```

您可以通过"go.languageServerExperimentalFeatures"部分的配置禁用功能。您可能想要禁用的一个特性示例是“documentLink”，当您单击文件中的import语句时，它会打开pkg.go.dev链接。

#### Build tags

```
build tags will not be picked from go.buildTags configuration section, instead they should be specified as part of theGOFLAGS environment variable:
```

build tags将不会从go.buildTags配置部分中选取，相反，它们应该被指定为theGOFLAGS环境变量的一部分:

```
"go.toolsEnvVars": {
    "GOFLAGS": "-tags=<yourtag>"
}
```

#### VSCode Remote Development with gopls

```
You can also make use of gopls with the VSCode Remote Development extensions to enable full-featured Go development on a lightweight client machine, while connected to a more powerful server machine.

First, install the Remote Development extension of your choice, such as the Remote - SSH extension. Once you open a remote session in a new window, open the Extensions pane (Ctrl+Shift+X) and you will see several different sections listed. In the "Local - Installed" section, navigate to the Go extension and click "Install in SSH: hostname".

Once you have reloaded VSCode, you will be prompted to install gopls and other Go-related tools. After one more reload, you should be ready to develop remotely with VSCode and the Go extension.
```

您还可以将gopls与VSCode远程开发扩展一起使用，从而在连接到更强大的服务器机器的同时，在轻量级客户机上启用全功能的Go开发。

首先，安装您选择的远程开发扩展，比如Remote - SSH扩展。在新窗口中打开远程会话后，打开Extensions pane(Ctrl+Shift+X)，您将看到列出的几个不同部分。在“Local - Installed”部分，导航到Go扩展并单击“Install in SSH: hostname”。

重新加载VSCode之后，系统会提示您安装gopls和其他go相关工具。在再次重新加载之后，您应该可以准备使用VSCode和Go扩展进行远程开发了。

### Know issues

```
1、Editing multiple modules in one editor window: 
```

[在一个编辑器窗口中编辑多个模块](https://github.com/golang/go/issues/32394)[#32394](https://github.com/golang/go/issues/32394)

```
2、Type checking does not work in cgo packages: 
```

[类型检查在cgo包中不起作用](https://github.com/golang/go/issues/35721)[#35721](https://github.com/golang/go/issues/35721)

```
3、Does not work with build tags: 
```

[#29202](https://github.com/golang/go/issues/29202)

```
4、Find references and rename only work in a single package: 
```

查找引用和重命名只在一个包中工作[#32877](https://github.com/golang/go/issues/32877)





