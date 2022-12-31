---
title: "Hugo+Firebase+CircleCIで独自ドメインの個人ブログをさっと構築"
date: 2017-07-23T22:49:08+09:00
Categories: ["development"]
Description: ""
Tags: ["GoLang", "Firebase", "CircleCI"]
canonicalUrl: "https://www.t104fk.xyz/posts/how-to-build-hugo-firebase-blog"
---

## なんで構築する必要があったか

必要はない。  
ただ、はてなブログ課金するのめんどくさくなってきただけ。  
しかもめちゃめちゃブログ更新するわけじゃないし(しろよ)。

## なんでこの構成か

個人ブログ構築するとして何がいいかなーって探してみたけど、周りに Hugo 使ってる人がわりといたのと、Firebase にのせとけば無料だし CircleCI でデプロイも自動化できるし、CircleCI 2.0 を試したいなーとちょうど思ってたのでこの構成にした。  
Hugo 自体の特徴はだいたいこんな感じ。

- markdown で記事書いていくだけ
- カテゴリとかタグも記事の頭に書いておけば勝手に作られる
- 保存したらブラウザが即反映
- Githug+CircleCI のインテグレーションにのせられる

あと個人ブログで気になってくるのはデザイン。  
Hugo だと`git clone`で特定のディレクトリに入れて`config.toml`を編集するだけ。  
細かい修正は記事が増えてきたら少しずつやってきたいなと。

## 作業ログ

やることはだいたいこんな感じ。

0. Golang, Hugo のインストール
1. Firebase の登録とプロジェクト追加
2. Route53 でドメインの取得
3. Firebase の設定とかトークン取得とか  
   Firebase のドキュメントが詳しい 👉 [Connect a Custom Domain](https://firebase.google.com/docs/hosting/custom-domain)
4. Theme を選んで配置
5. Theme の example を参考にしながら config.toml の修正
6. `.circleci/config.yml`の記述  
   詳しくはこちら 👉[CircleCI2.0 事始め -新しい circle.yml と workflows 編- #circleci](https://blog.stormcat.io/post/entry/circleci2.0-overview01/)

ハマったのはこれと全然関係ないところで、SSL 証明書が何故かずっと broken HTTPS と Not Secure な状態になってた。  
なんちゃない`config.toml`に`http://`で powered by Hugo のリンクを設定してしまっていてそれのせいだったっぽい(サボりすぎ)。

## 所感

- 一回構築してしまえば後は楽
- Theme がわりといろいろあるし、Blog じゃなくてもいろんな種類があるしすぐ当てられる。その代わりかっこいいテーマは被る。  
  👉[Hugo Themes](https://themes.gohugo.io/)
- CircleCI 2.0 は Docker が first-class citizen でキャッシュも勝手にやってくださいって感じでよさげ
- Firebase のカスタムドメイン設定チュートリアルが親切で助かった(スクショ撮っておけばよかった)
- Github Pages を一瞬使ってたけど、こうやって全部構築すると自分の資産としての意識が高まる 📈

過去記事はめんどくさいので捨てましたｗ  
今のプロジェクトの開発改善方針とか技術的な落とし所が見えてきたのでまた書いていく 💪
