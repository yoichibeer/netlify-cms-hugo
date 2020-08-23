---
title: "[作業メモ] Netlify でブログはじめます"
date: 2020-08-14T08:53:40+09:00
draft: false
tags:
  - netlify
  - netlify cms
  - hugo
description: Netlify にブログを構築した時のメモ
---
## \- Why -

アウトプットは、ほぼ本業のプログラムだけな昨今、プログラム以外のアウトプット場所を作って、頭の整理に使おうかと。

- - -

## \- How -

Qiita とか note とか 既存のブログサービスに乗っかるのが早いと思ったけど、制限あるみたいだし、やめ。メンテナンスコストはさげつつ独自ブログにチャレンジ。

無料で WordPress できないか探して見たけど、広告つくし、遅いし、いつか急にサービスやめちゃう感もあって、やめ。

有料で安いの探して [Amazon Lightsail](https://aws.amazon.com/jp/lightsail/) の WordPress が、安さ、速さ、学習という観点から一択かなと。ひとまず開始して数ヶ月サービス立ち上げて放置。WordPress は、メンドー、いまさらー、安いとはいえお金かかるし金払わなくなったら閉鎖だし、と思い始めて解約。。

現時点の結論は Netlify。使用する静的サイトジェネレーターは悩んだけど Hugo。静的ジェネレーターは頻繁にビルドするのでビルドが速いのは嬉しい。Hugo は実績あって情報もあるため、構築とかメンテとか比較的楽にいけそう感あり。Vue.js さわりたいという意味で VuePress も魅力だったけどこの目的でハマるのは別にする。

Hugo の theme は git submodule 使う方法だと面倒でオススメしないと言っている人いたり git submodule じゃなくて git clone で取得して .git 削除している情報も見かけるけど Hugo 本家の方法の git submodule 使う。git submodule 使って theme 自体は直接変えずに layouts ディレクトリ以下に同名ファイル置いて上書きし theme のオリジナルの最新を追随する運用を目指す。自分で theme 作って公開する場合も git submodule を使う方が管理しやすそう。
Hugo の theme の選定は tag あって search できて blog かけて mobile 表示できて google analytics できて、シンプル、を考える。

あと GitHub に直接 push じゃなくて Netlify CMS 入れて更新の障壁さげる。Netlify CMS には template を使って Hugo のサイトも新規に作る方法と、既存の Hugo のサイトに Netlify CMS を追加する方法がある。[Start with a Template](https://www.netlifycms.org/docs/start-with-a-template/) で template 使う方法をやってみたが、あっという間にできる。ただ、その後どうすればいいかわからん。ググってもうまくカスタマイズできず [Add to Your Site](https://www.netlifycms.org/docs/add-to-your-site/) で Netlify CMS を手作業で追加をしつつ、理解を深める。

マークダウンは、共通のフォーマットで Netlify への依存もなくて、Hugo への依存はありそうだけど、汎用の http サーバーならどこでも置けて、昨今は CDN で速くなる。Headless CMS がもっと扱いやすくなったり、プラグイン的な機能の追加が楽になれば WordPress 使う理由は減っていきそう。静的サイトジェネレーター使うとフォーム系ができないけど、他のサービスと組み合わせて使うと実現できそう。

あー、でも GitHub 上のファイルをデータベースの代わりってセンスいい。
Linux はあらゆるものをファイルに抽象化して扱えるって話を t-wada さんが [28. 技術選定の審美眼(1) w/ twada | fukabori.fm](https://fukabori.fm/episode/28) で話してたけど、なんか似た話。

- - -

## \- What -

[Add to Your Site | Netlify CMS | Open-Source Content Management System](https://www.netlifycms.org/docs/add-to-your-site/) を見ながら Netlify CMS の追加を既存の Hugo のサイトにやるんだけど、その前にまずは既存のサイトを作らなければ、ということで Hugo 本家のドキュメントである [Quick Start | Hugo](https://gohugo.io/getting-started/quick-start/) に従ってみた。

- - -

### 既存のサイト作る

参考サイト

* Hugo 本家のクイックスタート [Quick Start | Hugo](https://gohugo.io/getting-started/quick-start/)
* Hugo 本家のリファレンス [Hugo Documentation | Hugo](https://gohugo.io/documentation/)
* config.toml 編集する上で Hugo の中身の理解に助かりました [静的サイトジェネレータ「Hugo」と技術文書公開向けテーマ「Docsy」でOSSサイトを作る | さくらのナレッジ](https://knowledge.sakura.ad.jp/22908/)

以下、打ち込んだコマンドと応答たち。Mac で brew とか git とか入っている前提。

```sh
$ brew install hugo
$ hugo version
Hugo Static Site Generator v0.74.3/extended darwin/amd64 BuildDate: unknown
$ hugo new site netlify-cms-hugo # サイト名の netlify-cms-hugo のところは好きな名前にしてね
Congratulations! Your new Hugo site is created in /Users/yoichi/work/netlify-cms-hugo.

Just a few more steps and you're ready to go:

1. Download a theme into the same-named folder.
   Choose a theme from https://themes.gohugo.io/ or
   create your own with the "hugo new theme <THEMENAME>" command.
2. Perhaps you want to add some content. You can add single files
   with "hugo new <SECTIONNAME>/<FILENAME>.<FORMAT>".
3. Start the built-in live server via "hugo server".

Visit https://gohugo.io/ for quickstart guide and full documentation.
```

[Quick Start | Hugo](https://gohugo.io/getting-started/) では [Ananke theme](https://themes.gohugo.io/gohugo-theme-ananke/) 使っているけど [Bodhi Theme](https://themes.gohugo.io/bodhi/) にしよう。

```sh
$ cd netlify-cms-hugo
$ git init
$ git submodule add https://github.com/rhnvrm/bodhi.git themes/bodhi
$ echo 'theme = "bodhi"' >> config.toml
$ cat config.toml
baseURL = "http://example.org/"
languageCode = "en-us"
title = "My New Hugo Site"
theme = "bodhi"
$ hugo new posts/netlify-cms-hugo.md
/Users/yoichi/work/netlify-cms-hugo/content/posts/netlify-cms-hugo.md created
```

コミットしないと。

```sh
$ touch layouts/.gitkeep data/.gitkeep resources/.gitkeep static/.gitkeep
$ git add .
$ git status
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
        new file:   .gitmodules
        new file:   archetypes/default.md
        new file:   config.toml
        new file:   content/posts/netlify-cms-hugo.md
        new file:   data/.gitkeep
        new file:   layouts/.gitkeep
        new file:   resources/.gitkeep
        new file:   static/.gitkeep
        new file:   themes/bodhi

$ git commit -m "initial commit" # もっと手前に細かくコミットかも
[master (root-commit) 68615a3] initial commit
 9 files changed, 20 insertions(+)
 create mode 100644 .gitmodules
 create mode 100644 archetypes/default.md
 create mode 100644 config.toml
 create mode 100644 content/posts/netlify-cms-hugo.md
 create mode 100644 data/.gitkeep
 create mode 100644 layouts/.gitkeep
 create mode 100644 resources/.gitkeep
 create mode 100644 static/.gitkeep
 create mode 160000 themes/bodhi
```

ローカルで起動して確認してみる。

```sh
$ hugo server -D
```

ここで -D は以下みたい。`draft: true` になっててもページ作ってくれる。

```
  -D, --buildDrafts            include content marked as draft
```

いい感じに起動。

![デフォルト起動画面](/images/netlify-cms-hugo_01.png)

次に、設定ファイルを [Bodhi Theme](https://themes.gohugo.io/bodhi/) のオススメのに変えてみる。 exampleSite ディレクトリからコピー。

```sh
$ cp themes/bodhi/exampleSite/config.toml .
```

config.toml の設定に試行錯誤した結果、以下に。Google Analytics も設定。

```toml
baseURL = "https://yoichi-beer.netlify.app/"
languageCode = "ja"
hasCJKLanguage = true
title = "loose talk"
theme = "bodhi"
googleAnalytics = "UA-175438985-1"

[params]
    # subtitle = "ゆるい話"
    avatar = "/images/lily.png"
    author = "yoichibeer"
    commentoSrc = "https://commento.example.com/js/commento.js"
    disableSearch = false

[[menu.main_right]]
name = "tags"
url = "/tags"
weight = 1

[[menu.main_right]]
name = "about"
url = "/about/"
weight = 2

[markup.goldmark.renderer]
unsafe= true

[outputs]
  home = ["HTML", "RSS", "JSON"]

[markup]
  [markup.highlight]
    codeFences = true
    guessSyntax = true
    lineNoStart = 1
    noClasses = true
    style = "emacs"
    tabWidth = 4

[[params.social]]
name = "Github"
icon = "github"
url = "https://github.com/yoichibeer"

[[params.social]]
name = "Twitter"
icon = "twitter"
url = "https://twitter.com/yoichibeer"
```

search.js がうまく動かない。exampleSite ディレクトリ以下の search.md をコピーして動いた。

```sh
$ cp themes/bodhi/exampleSite/content/search.md content/
```

about 以下は Hugo 的に _index.md を置く。 avatar 画像をうちのわんこにしてひとまず完成。↓ワンコ画像失敗しているけど。

![少しだけカスタマイズした画面](/images/netlify-cms-hugo_02.png)

- - -

### Netlify CMS 入れて Netlify でホストする

参考サイト

* 既存のサイトに Netlify CMS を追加する [Add to Your Site | Netlify CMS | Open-Source Content Management System](https://www.netlifycms.org/docs/add-to-your-site/)
* 既存の Hugo サイトに Netlify CMS を追加する [Hugo | Netlify CMS | Open-Source Content Management System](https://www.netlifycms.org/docs/hugo/)
* Hugo 本家サイトの Netlify にホストするための記事 [Host on Netlify | Hugo](https://gohugo.io/hosting-and-deployment/hosting-on-netlify/)

static ディレクトリ以下に admin フォルダ作って index.html と

```html
<!DOCTYPE html>
<html>

<head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Content Manager</title>
    <!-- Include the script that enables Netlify Identity on this page. -->
    <script src="https://identity.netlify.com/v1/netlify-identity-widget.js"></script>
</head>

<body>
    <!-- Include the script that builds the page and powers Netlify CMS -->
    <script src="https://unpkg.com/netlify-cms@^2.0.0/dist/netlify-cms.js"></script>
</body>

</html>
```

config.yml を追加。

```yaml
backend:
  name: git-gateway
  branch: master # Branch to update (optional; defaults to master)

# These lines should *not* be indented
media_folder: "static/images" # Media files will be stored in the repo under static/images/uploads
public_folder: "/images" # The src attribute for uploaded media will begin with /images/uploads

collections:
  - name: 'post'
    label: 'Post'
    folder: 'content/posts'
    create: true
    slug: '{{year}}-{{month}}-{{day}}-{{slug}}'
    editor:
      preview: false
    fields:
      - { label: 'Title', name: 'title', widget: 'string' }
      - { label: 'Publish Date', name: 'date', widget: 'datetime' }
      - { label: 'Description', name: 'description', widget: 'string' }
      - { label: 'Body', name: 'body', widget: 'markdown' }
```

git commit して GitHub に置く。

```sh
$ git remote add origin https://github.com/yoichibeer/netlify-cms-hugo.git # Create a new repo on GitHub and add it to this project as a remote repository.
git push -u origin master # Push your changes
```

Netlify のサイトに行ってアカウント作って New site from Git ボタン押して [Netlify App](https://app.netlify.com/start) にいく。GitHub 選択して認証通す。リポジトリのリストが出てくるのでさきほど push したリポジトリを選択。

Build command は `hugo` にして Publish directory は `public` にして Deploy site ボタンおす。

`Site deploy failed` とかエラー発生。

![エラー画面](/images/netlify-cms-hugo_03.png)

上の画面の下の方の Failed リンク辿ると Deploy log が見れる。以下が問題箇所。

```
4:42:01 PM: ERROR 2020/08/15 07:42:01 BODHI theme does not support Hugo version 0.54.0. Minimum version required is 0.70.0
```

<details>
<summary>Deploy log</summary>

```log
4:41:43 PM: Build ready to start
4:41:50 PM: build-image version: b0258b965567defc4a2d7e2f2dec2e00c8f73ad6
4:41:50 PM: build-image tag: v3.4.1
4:41:50 PM: buildbot version: 9042ba4998dab698f1f37fb8d36912c08a387191
4:41:51 PM: Fetching cached dependencies
4:41:51 PM: Failed to fetch cache, continuing with build
4:41:51 PM: Starting to prepare the repo for build
4:41:51 PM: No cached dependencies found. Cloning fresh repo
4:41:51 PM: git clone https://github.com/yoichibeer/netlify-cms-hugo
4:41:52 PM: Preparing Git Reference refs/heads/master
4:41:54 PM: Starting build script
4:41:54 PM: Installing dependencies
4:41:54 PM: Python version set to 2.7
4:41:56 PM: v12.18.0 is already installed.
4:41:57 PM: Now using node v12.18.0 (npm v6.14.4)
4:41:57 PM: Started restoring cached build plugins
4:41:57 PM: Finished restoring cached build plugins
4:41:57 PM: Attempting ruby version 2.7.1, read from environment
4:41:59 PM: Using ruby version 2.7.1
4:42:00 PM: Using PHP version 5.6
4:42:00 PM: 5.2 is already installed.
4:42:00 PM: Using Swift version 5.2
4:42:00 PM: Started restoring cached go cache
4:42:00 PM: Finished restoring cached go cache
4:42:00 PM: go version go1.14.4 linux/amd64
4:42:00 PM: go version go1.14.4 linux/amd64
4:42:00 PM: Installing missing commands
4:42:00 PM: Verify run directory
4:42:01 PM: ​
4:42:01 PM: ┌─────────────────────────────┐
4:42:01 PM: │        Netlify Build        │
4:42:01 PM: └─────────────────────────────┘
4:42:01 PM: ​
4:42:01 PM: ❯ Version
4:42:01 PM:   @netlify/build 3.1.10
4:42:01 PM: ​
4:42:01 PM: ❯ Flags
4:42:01 PM:   deployId: 5f3791b7fb8f78d247f53a10
4:42:01 PM:   mode: buildbot
4:42:01 PM:   timersFile: /tmp/substage_times.txt
4:42:01 PM: ​
4:42:01 PM: ❯ Current directory
4:42:01 PM:   /opt/build/repo
4:42:01 PM: ​
4:42:01 PM: ❯ Config file
4:42:01 PM:   No config file was defined: using default values.
4:42:01 PM: ​
4:42:01 PM: ❯ Context
4:42:01 PM:   production
4:42:01 PM: ​
4:42:01 PM: ┌───────────────────────────────────┐
4:42:01 PM: │ 1. Build command from Netlify app │
4:42:01 PM: └───────────────────────────────────┘
4:42:01 PM: ​
4:42:01 PM: $ hugo
4:42:01 PM: ERROR 2020/08/15 07:42:01 BODHI theme does not support Hugo version 0.54.0. Minimum version required is 0.70.0
4:42:01 PM: Building sites … Total in 15 ms
4:42:01 PM: Error: Error building site: failed to render pages: render of "section" failed: "/opt/build/repo/themes/bodhi/layouts/_default/list.html:9:14": execute of template failed: template: _default/list.html:9:14: executing "main" at <.RegularPages>: can't evaluate field RegularPages in type *hugolib.PageOutput​
4:42:01 PM: ┌─────────────────────────────┐
4:42:01 PM: │   "build.command" failed    │
4:42:01 PM: └─────────────────────────────┘
4:42:01 PM: ​
4:42:01 PM:   Error message
4:42:01 PM:   Command failed with exit code 255: hugo
4:42:01 PM: ​
4:42:01 PM:   Error location
4:42:01 PM:   In Build command from Netlify app:
4:42:01 PM:   hugo
4:42:01 PM: ​
4:42:01 PM:   Resolved config
4:42:01 PM:   build:
4:42:01 PM:     command: hugo
4:42:01 PM:     commandOrigin: ui
4:42:01 PM:     publish: /opt/build/repo/public
4:42:02 PM: Caching artifacts
4:42:02 PM: Started saving build plugins
4:42:02 PM: Finished saving build plugins
4:42:02 PM: Started saving pip cache
4:42:02 PM: Finished saving pip cache
4:42:02 PM: Started saving emacs cask dependencies
4:42:02 PM: Finished saving emacs cask dependencies
4:42:02 PM: Started saving maven dependencies
4:42:02 PM: Finished saving maven dependencies
4:42:02 PM: Started saving boot dependencies
4:42:02 PM: Finished saving boot dependencies
4:42:02 PM: Started saving go dependencies
4:42:02 PM: Finished saving go dependencies
4:42:05 PM: Error running command: Build script returned non-zero exit code: 1
4:42:05 PM: Failing build: Failed to build site
4:42:05 PM: Failed during stage 'building site': Build script returned non-zero exit code: 1
4:42:05 PM: Finished processing build request in 14.93889708s
```

</details>

ググると netlify.toml 書けということで [Host on Netlify | Hugo](https://gohugo.io/hosting-and-deployment/hosting-on-netlify/) にある netlify.toml 追加してうまくいった。 push すると Netlify が自動でデプロイしてくれる。

```toml
[build]
publish = "public"
command = "hugo --gc --minify"

[context.production.environment]
HUGO_VERSION = "0.74.3"
HUGO_ENV = "production"
HUGO_ENABLEGITINFO = "true"

[context.split1]
command = "hugo --gc --minify --enableGitInfo"

[context.split1.environment]
HUGO_VERSION = "0.74.3"
HUGO_ENV = "production"

[context.deploy-preview]
command = "hugo --gc --minify --buildFuture -b $DEPLOY_PRIME_URL"

[context.deploy-preview.environment]
HUGO_VERSION = "0.74.3"

[context.branch-deploy]
command = "hugo --gc --minify -b $DEPLOY_PRIME_URL"

[context.branch-deploy.environment]
HUGO_VERSION = "0.74.3"

[context.next.environment]
HUGO_ENABLEGITINFO = "true"
```

Netlify の管理サイトの | Build & deploy | Environment | Environment variables | に HUGO_VERSION を指定しても良いみたい。

Netliy CMS の /admin/ にアクセスしても認証できないし、そもそも登録者いなそう。今日はやめ。

![](/images/netlify-cms-hugo_04.png)

### 2020/08/23 追記

Netlify の管理サイトの | Site setting | Identity | Registration | Registration preferences | で Invite only から Open にすると 
https://yoichi-beer.netlify.app/admin/#/
にアクセスしたときに Sign up タブが表示されて登録できる。項目埋めて singn up すると admin にログインできる！

最後に、 Invite only に戻しましょ。

![](/images/netlify-cms-hugo_05.png)

ちなみに検索すると出てくる Netlify Identify Widget はすでに admin ディレクトリ以下の index.html にあるので特に作業必要なし。
`<script src="https://identity.netlify.com/v1/netlify-identity-widget.js">`

で、この追記部分は Netlilfy CMS から書いている。画像は Ｍａｒｋｄｏｗｎ モードではなくて Rich Text モードでできる。

<<<< 2020/08/23 追記ここまで

## \- Retrospective -

自前でブログサイト作るの思ったより大変やった。この記事も内容盛り込みすぎた。二日くらいかかったかな。やり残したこともたくさん。 Trello に放り込んでおいて細かく記事あげよ。

結局 Hugo を知らないとできない。 Gatsby とか VuePress とかでも、学習コストはそれほど変わらない予感。Go 言語のテンプレートに依存しなければ、マークダウンの記事は移植しやすいと思うので、マークダウンは汎用的に保とう。どこかで js 系に移動したくなるかも。

デザインも思ったより大事だ。シンプルに倒していこ。

Done is better than perfect だけど、沢山の更新が待っているね。

まー、夏休み中に一通りできてよかった！