---
title: "[作業ログ] VS Code Extesion で Hello World!"
date: 2020-08-23T07:58:14.846Z
description: MessagePack 形式のファイル を  JSON 形式で見えるようにする VS Code Extesion を作る途中の作業メモ
---
PS C:\Users\y\MessagePackViewer> yo code（かきかけー）

### \- Why -

[MessagePack](https://msgpack.org/ja.html) 形式のファイルを VS Code 上で json 形式にして見たいなと。まずは、やっぱり、 Hello World! でて動かす。

### \- How -

本家サイト [Your First Extension | Visual Studio Code Extension API](https://code.visualstudio.com/api/get-started/your-first-extension) で Hello World つくって作り方をざっくり把握して message pack 読み込みはあとから考えよう。

message pack 読み込みは以下でいけるかな。

\[mcollina/msgpack5: A msgpack v5 implementation for node.js, with extension points / msgpack.org[Node]](https://github.com/mcollina/msgpack5)

### \- What -

#### Visual Studio Code の Extesion で Hello World!

##### 準備

* Visual Studio Code は version 1.48.1
* node は 12.18.3
* nvm-windows 使ってる [coreybutler/nvm-windows: A node.js version management utility for Windows. Ironically written in Go.](https://github.com/coreybutler/nvm-windows)
* git は 2.28.0

```powershell
PS C:\Users\y> nvm list available

|   CURRENT    |     LTS      |  OLD STABLE  | OLD UNSTABLE |
|--------------|--------------|--------------|--------------|
|    14.8.0    |   12.18.3    |   0.12.18    |   0.11.16    |
|    14.7.0    |   12.18.2    |   0.12.17    |   0.11.15    |
|    14.6.0    |   12.18.1    |   0.12.16    |   0.11.14    |
|    14.5.0    |   12.18.0    |   0.12.15    |   0.11.13    |
|    14.3.0    |   12.16.3    |   0.12.13    |   0.11.11    |
|    14.2.0    |   12.16.2    |   0.12.12    |   0.11.10    |
|    14.1.0    |   12.16.1    |   0.12.11    |    0.11.9    |
|    14.0.0    |   12.16.0    |   0.12.10    |    0.11.8    |
|   13.14.0    |   12.15.0    |    0.12.9    |    0.11.7    |
|   13.13.0    |   12.14.1    |    0.12.8    |    0.11.6    |
|   13.12.0    |   12.14.0    |    0.12.7    |    0.11.5    |
|   13.11.0    |   12.13.1    |    0.12.6    |    0.11.4    |
|   13.10.1    |   12.13.0    |    0.12.5    |    0.11.3    |
|   13.10.0    |   10.22.0    |    0.12.4    |    0.11.2    |
|    13.9.0    |   10.21.0    |    0.12.3    |    0.11.1    |
|    13.8.0    |   10.20.1    |    0.12.2    |    0.11.0    |
|    13.7.0    |   10.20.0    |    0.12.1    |    0.9.12    |
|    13.6.0    |   10.19.0    |    0.12.0    |    0.9.11    |
|    13.5.0    |   10.18.1    |   0.10.48    |    0.9.10    |

This is a partial list. For a complete list, visit https://nodejs.org/download/release
PS C:\Users\y> nvm install 12.18.3
Downloading node.js version 12.18.3 (64-bit)...
Complete
Creating C:\Users\y\AppData\Roaming\nvm\temp

Downloading npm version 6.14.6... Complete
Installing npm v6.14.6...

Installation complete. If you want to use this version, type

nvm use 12.18.3
PS C:\Users\y> nvm use 12.18.3
Now using node v12.18.3 (64-bit)
PS C:\Users\y> git --version
git version 2.28.0.windows.1
PS C:\Users\y>
```

##### Yeoman and VS Code Extension Generator をインストール

* [The web's scaffolding tool for modern webapps | Yeoman](https://yeoman.io/)
* [generator-code  -  npm](https://www.npmjs.com/package/generator-code)

```powershell
npm install -g yo generator-code
```

を実行する。数分かかる。

```
PS C:\Users\y\MessagePackViewer> npm --version
6.14.6
PS C:\Users\y\MessagePackViewer> npm list
C:\Users\y\MessagePackViewer
`-- (empty)

PS C:\Users\y\MessagePackViewer> npm install -g yo generator-code
npm WARN deprecated request@2.88.2: request has been deprecated, see https://github.com/request/request/issues/3142
npm WARN deprecated har-validator@5.1.5: this library is no longer supported
npm WARN deprecated cross-spawn-async@2.2.5: cross-spawn no longer requires a build toolchain, use it instead
C:\Program Files\nodejs\yo -> C:\Program Files\nodejs\node_modules\yo\lib\cli.js
C:\Program Files\nodejs\yo-complete -> C:\Program Files\nodejs\node_modules\yo\lib\completion\index.js

> ejs@2.7.4 postinstall C:\Program Files\nodejs\node_modules\generator-code\node_modules\yeoman-environment\node_modules\ejs
> node ./postinstall.js

Thank you for installing EJS: built with the Jake JavaScript build tool (https://jakejs.com/)


> core-js@3.6.5 postinstall C:\Program Files\nodejs\node_modules\yo\node_modules\core-js
> node -e "try{require('./postinstall')}catch(e){}"

Thank you for using core-js ( https://github.com/zloirock/core-js ) for polyfilling JavaScript standard library!

The project needs your help! Please consider supporting of core-js on Open Collective or Patreon:
> https://opencollective.com/core-js
> https://www.patreon.com/zloirock

Also, the author of core-js ( https://github.com/zloirock ) is looking for a good job -)


> ejs@2.7.4 postinstall C:\Program Files\nodejs\node_modules\yo\node_modules\ejs
> node ./postinstall.js

Thank you for installing EJS: built with the Jake JavaScript build tool (https://jakejs.com/)


> spawn-sync@1.0.15 postinstall C:\Program Files\nodejs\node_modules\yo\node_modules\spawn-sync
> node postinstall


> yo@3.1.1 postinstall C:\Program Files\nodejs\node_modules\yo
> yodoctor


Yeoman Doctor
Running sanity checks on your system

√ No .bowerrc file in home directory
√ Global configuration file is valid
√ NODE_PATH matches the npm root
√ No .yo-rc.json file in home directory
√ Node.js version
√ npm version
√ yo version

Everything looks all right!
+ yo@3.1.1
+ generator-code@1.2.21
added 1164 packages from 395 contributors in 183.926s
PS C:\Users\y\MessagePackViewer>
```

グローバルに入る。

##### Run the generator

次に、つくってみる。

```powershell
yo code
```

を実行する。以下がでる。

```
PS C:\Users\y\MessagePackViewer\messagepackviewer> yo code
yo : このシステムではスクリプトの実行が無効になっているため、ファイル C:\Program Files\nodejs\yo.ps1 を読み込むことがで
きません。詳細については、「about_Execution_Policies」(https://go.microsoft.com/fwlink/?LinkID=135170) を参照してくださ
い。
発生場所 行:1 文字:1
+ yo code
+ ~~
    + CategoryInfo          : セキュリティ エラー: (: ) []、PSSecurityException
    + FullyQualifiedErrorId : UnauthorizedAccess
PS C:\Users\y\MessagePackViewer\messagepackviewer> cd ..
PS C:\Users\y\MessagePackViewer> yo code
yo : このシステムではスクリプトの実行が無効になっているため、ファイル C:\Program Files\nodejs\yo.ps1 を読み込むことがで
きません。詳細については、「about_Execution_Policies」(https://go.microsoft.com/fwlink/?LinkID=135170) を参照してくださ
い。
発生場所 行:1 文字:1
+ yo code
+ ~~
    + CategoryInfo          : セキュリティ エラー: (: ) []、PSSecurityException
    + FullyQualifiedErrorId : UnauthorizedAccess
PS C:\Users\y\MessagePackViewer> Get-ExecutionPolicy
Restricted
PS C:\Users\y\MessagePackViewer> Set-ExecutionPolicy RemoteSigned
Set-ExecutionPolicy : レジストリ キー 'HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\PowerShell\1\ShellIds\Microsoft.PowerShell
' へのアクセスが拒否されました。 既定 (LocalMachine) のスコープの実行ポリシーを変更するには、[管理者として実行] オプシ
ョンを使用して Windows PowerShell を起動してください。現在のユーザーの実行ポリシーを変更するには、"Set-ExecutionPolicy
-Scope CurrentUser" を実行してください。
発生場所 行:1 文字:1
+ Set-ExecutionPolicy RemoteSigned
\+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : PermissionDenied: (:) [Set-ExecutionPolicy], UnauthorizedAccessException
    + FullyQualifiedErrorId : System.UnauthorizedAccessException,Microsoft.PowerShell.Commands.SetExecutionPolicyComma
   nd
PS C:\Users\y\MessagePackViewer>
```

つまり、powershell を管理者権限で起動して `Set-ExecutionPolicy RemoteSigned` しろということなので、

```powershell
PS C:\Users\y> Set-ExecutionPolicy RemoteSigned
PS C:\Users\y>
```

では、再度、

```powershell
yo code
```

を実行して、いくつか質問されるのでてきとーに答えるとできたっぽい。

```
PS C:\Users\y\MessagePackViewer> yo code
? ==========================================================================
We're constantly looking for ways to make yo better!
May we anonymously report usage statistics to improve the tool over time?
More info: https://github.com/yeoman/insight & http://yeoman.io
========================================================================== Yes

     _-----_     ╭──────────────────────────╮
    |       |    │   Welcome to the Visual  │
    |--(o)--|    │   Studio Code Extension  │
   `---------´   │        generator!        │
    ( _´U`_ )    ╰──────────────────────────╯
    /___A___\   /
     |  ~  |
   __'.___.'__
 ´   `  |° ´ Y `

? What type of extension do you want to create? New Extension (JavaScript)
? What's the name of your extension? MessagePackViewer
? What's the identifier of your extension? messagepackviewer
? What's the description of your extension? Converts MessgePack format to JSON format and displays.
? Enable JavaScript type checking in 'jsconfig.json'? Yes
? Initialize a git repository? Yes
? Which package manager to use? npm
   create messagepackviewer\.vscode\extensions.json
   create messagepackviewer\.vscode\launch.json
   create messagepackviewer\test\runTest.js
   create messagepackviewer\test\suite\extension.test.js
   create messagepackviewer\test\suite\index.js
   create messagepackviewer\.vscodeignore
   create messagepackviewer\.gitignore
   create messagepackviewer\README.md
   create messagepackviewer\CHANGELOG.md
   create messagepackviewer\vsc-extension-quickstart.md
   create messagepackviewer\jsconfig.json
   create messagepackviewer\extension.js
   create messagepackviewer\package.json
   create messagepackviewer\.eslintrc.json


I'm all done. Running npm install for you to install the required dependencies. If this fails, try running the command yourself.


npm WARN deprecated flat@4.1.0: Fixed a prototype pollution security issue in 4.1.0, please upgrade to ^4.1.1 or ^5.0.1.npm notice created a lockfile as package-lock.json. You should commit this file.
npm WARN optional SKIPPING OPTIONAL DEPENDENCY: fsevents@~2.1.2 (node_modules\chokidar\node_modules\fsevents):
npm WARN notsup SKIPPING OPTIONAL DEPENDENCY: Unsupported platform for fsevents@2.1.3: wanted {"os":"darwin","arch":"any"} (current: {"os":"win32","arch":"x64"})
npm WARN messagepackviewer@0.0.1 No repository field.
npm WARN messagepackviewer@0.0.1 No license field.

added 215 packages from 150 contributors and audited 218 packages in 40.981s

30 packages are looking for funding
  run `npm fund` for details

found 0 vulnerabilities


Your extension messagepackviewer has been created!

To start editing with Visual Studio Code, use the following commands:

     cd messagepackviewer
     code .

Open vsc-extension-quickstart.md inside the new extension for further instructions
on how to modify, test and publish your extension.

For more information, also visit http://code.visualstudio.com and follow us @code.


PS C:\Users\y\MessagePackViewer>
```

##### 開発を始める

以下を実行

```powershell
cd messagepackviewer
code .
```

![](/images/messagepackviewer-01.png)

##### Hello World を実行してみる

* 新しく起動した VS Code の Window にて F5 を押す
* Ctrl+Shift+P して `Hello World` Command を打ち込むと以下のウィンドウ

  ![](/images/messagepackviewer-02.png)

### Retrospective

まーできた。中身、理解しよ。次は MesseageMack かな。
﻿