---
sidebar_position: 3
---

# IntelliJ IDEA

我们提供了两个版本的 IntelliJ KCL 插件

- IntelliJ KCL: 基础版本的 KCL 插件提供了语法高亮和补全的基础能力，可以在更多版本使用。

- IntelliJ KCL LSP: 基于 [LSP(Language Server Protocol)](https://code.visualstudio.com/api/language-extensions/language-server-extension-guide) 实现的 KCL 插件提供了更多功能，如错误提示，智能补全，悬停等功能。但因为一些[原因](https://plugins.jetbrains.com/docs/intellij/language-server-protocol.html#supported-ides)，只能在特定版本中使用。

> 与 Language Server Protocol 的集成是作为基于 IntelliJ 的付费 IDE 的扩展而创建的。 因此，使用 LSP 集成的插件在 JetBrains 产品和 Google 的 Android Studio 的社区版本中不可用。
> 从 2023.2 发布周期开始，LSP API 作为 IntelliJ 平台的一部分在以下 IDE 中公开提供：IntelliJ IDEA Ultimate、WebStorm、PhpStorm、PyCharm Professional、DataSpell、RubyMine、CLion、Aqua、DataGrip、GoLand、Rider 和 RustRover.

## IntelliJ KCL

基础版本的 KCL 插件可以直接在 IntelliJ 的插件市场下载使用。基础版本提供了语法高亮和关键字补全的能力
![intellij](/img/docs/tools/Ide/intellij/kcl.png)

## IntelliJ KCL KCL

基于 LSP 版本的插件可以从插件市场下载

![intellij](/img/docs/tools/Ide/intellij/kcl-lsp.png)

除此之外，您还需要[安装 kcl-language-server](https://www.kcl-lang.io/docs/user_docs/getting-started/install#install-language-server) 并检查 `kcl-language-server` 命令在您的 PATH 中:
在 MacOs 和 Linux中：

```bash
which kcl-language-server
```

在 Windows 中:

```bash
where kcl-language-server
```

注意，LSP 版本的插件不一定在所有版本的 IntelliJ IDE 中可以使用。
