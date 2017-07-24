---
title: "Hugo+Firebase+CircleCIで独自ドメインの個人ブログをさっと構築"
date: 2017-07-23T22:49:08+09:00
Categories: ["Development","GoLang","Firebase","CircleCI"]
Description: ""
Tags: ["Development","GoLang","Firebase","CircleCI"]
date: "2017-04-19T20:15:30+09:00"
menu: "main"
---

## なんで構築する必要があったか
必要はない。  
ただ、はてなブログ課金するのめんどくさくなってきただけ。  
しかもめちゃめちゃブログ更新するわけじゃないし(しろよ)。

## なんでこの構成か
個人ブログ構築するとして何がいいかなーって探してみたけど、周りにHugo使ってる人がわりといたのと、Firebaseにのせとけば無料だしCircleCIでデプロイも自動化できるし、CircleCI 2.0を試したいなーとちょうど思ってたのでこの構成にした。  
Hugo自体の特徴はだいたいこんな感じ。  

- markdownで記事書いていくだけ
- カテゴリとかタグも記事の頭に書いておけば勝手に作られる
- 保存したらブラウザが即反映
- Githug+CircleCIのインテグレーションにのせられる

あと個人ブログで気になってくるのはデザイン。  
Hugoだと`git clone`で特定のディレクトリに入れて`config.toml`を編集するだけ。  
細かい修正は記事が増えてきたら少しずつやってきたいなと。

## 作業ログ
やることはだいたいこんな感じ。  

0. Golang, Hugoのインストール
0. Firebaseの登録とプロジェクト追加
0. Route53でドメインの取得
0. Firebaseの設定とかトークン取得とか  
Firebaseのドキュメントが詳しい👉 [Connect a Custom Domain](https://firebase.google.com/docs/hosting/custom-domain)
0. Themeを選んで配置
0. Themeのexampleを参考にしながらconfig.tomlの修正
0. `.circleci/config.yml`の記述  
詳しくはこちら👉[CircleCI2.0事始め -新しいcircle.ymlとworkflows編- #circleci](https://blog.stormcat.io/post/entry/circleci2.0-overview01/)

ハマったのはこれと全然関係ないところで、SSL証明書が何故かずっとbroken HTTPSとNot Secureな状態になってた。  
なんちゃない`config.toml`に`http://`でpowered by Hugoのリンクを設定してしまっていてそれのせいだったっぽい(サボりすぎ)。

## 所感
- 一回構築してしまえば後は楽
- Themeがわりといろいろあるし、Blogじゃなくてもいろんな種類があるしすぐ当てられる。その代わりかっこいいテーマは被る。  
👉[Hugo Themes](https://themes.gohugo.io/)
- CircleCI 2.0はDockerがfirst-class citizenでキャッシュも勝手にやってくださいって感じでよさげ
- Firebaseのカスタムドメイン設定チュートリアルが親切で助かった(スクショ撮っておけばよかった)
- Github Pagesを一瞬使ってたけど、こうやって全部構築すると自分の資産としての意識が高まる📈

過去記事はめんどくさいので捨てましたｗ  
今のプロジェクトの開発改善方針とか技術的な落とし所が見えてきたのでまた書いていく💪

