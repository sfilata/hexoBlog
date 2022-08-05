---
title: VSCode 开发相关知识
tags: 技术
date: 2022-08-05 10:50:58
updated:
---
# vscode 项目简介

参考wiki: [link](https://github.com/microsoft/vscode/wiki/How-to-Contribute?_blank)

## 运行vscode
```shell
./scripts/code.sh
./scripts/code-cli.sh # for running CLI commands (eg --version)
```

## 打包vscode
```shell
yarn gulp vscode-win32-x64
```

## vscode 插件开发

左边的view只能设置id和name [Link](https://code.visualstudio.com/api/references/contribution-points#contributes.views), 具体高度应该需要WebviewView或者TreeView去撑

#### activationEvents
- onLanguage:${language}
- onCommand:${command}
- onDebug
- workspaceContains:${toplevelfilename}
- onFileSystem:${scheme}
- onView:${viewId}
- onUri
- *

#### 跳转定义
vscode.languages.registerDefinitionProvider

### 自动补全
vscode.languages.registerCompletionItemProvider--
