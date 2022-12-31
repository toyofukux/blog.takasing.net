---
title: "営業メールを送る際のメールトラッキングサービスでよさそうなサービスを見つけたメモ"
date: 2022-08-20T16:53:46+09:00
Description: "業務で新サービスのメール営業をする際にサービス選定をした際のメモ"
images: ["/mailtracker/main.png"]
Categories: ["BizDev"]
Tags: ["SaaS", "email"]
canonicalUrl: "https://www.t104fk.xyz/posts/mailtracker"
---

![main](/mailtracker/main.png)

業務で新サービスの営業をする際に

1. そこそこの量の営業メールを一気に送る
2. 打率を上げるために開封したかどうかをトラッキングし、フォローメールを送る
3. 施策の効果を振り返りたいため全体の開封率を知りたい

みたいな要件のものが発生した際のメモ。  
自分で SpreadSheet にチェックボックス作ってトラッキングタグを入れたメールを GAS で一斉送信するみたいなのを実装しそうになったが、流石にその手の SaaS があったので紹介。

# 課題

そもそも今回選定したサービスを利用する前の課題として、

- 素早く仮説検証したい
- そのため Fat な統合的マーケティング SaaS ツールみたいな学習コストがかかるようなものはいやだ
- 営業も永遠に続くようなものではないのでぱっと始められて、ぱっと終われるものがよい

みたいな話があった。  
はじめは Zoho みたいなでかい有名なサービスを使ってみたが、思ったより学習コストが高いなと感じた上に、複数のサービスを内包しているので基本料金＋ α が必要なことがわかった。  
検証段階で潰せたのはえらい。  
結果、最悪 Zoho を使うが、もっと軽量なソリューションを調査もしくは実装みたいな話になった。

## [mailtrack ↗](https://mailtrack.io/)

冒頭書いた要件を満たすものを調査する中で、mailtrack というツールを見つけたので紹介する。  
このサービスは pixel-based tracking といって、html メールに 1px のトラッキング用の img タグを Chrome Extension が送信前にメール内に入れて、それを使って開封したかどうかを測定している。  
受信者がメールを開くとその 1px 画像がネットワーク経由でリクエストされ、mailtrack 側でリクエストを確認して開封確認をする。

機能としては以下のようなものがある。

- Gmail 上で送信メールが開封されたかどうか分かる
- Gmail 上で既読メールをフィルタしやすくするための仕組みもある(Gmail のラベルを使っているっぽい)
- メールが既読になった際に Chrome の通知を受け取ることができる
- SpreadSheet で複数の連絡先を管理し、そこに記載した人たちにメールを一斉送信できる
- メールの一斉送信では変数を使ってテンプレメールをカスタマイズできる
- 一斉送信したメールの開封率を Dashboard で確認することができる
- 既読メールの一覧を CSV でダウンロードすることができる

シンプルとかいいつつ必要十分そうに見える。

### シンプルにメールを送信

Chrome Extension をインストールすると、Google アカウントの認証フローをはさんだ後すぐ使えるようになる。  
これがドラフトの状態。  
![draft](/mailtracker/draft.png)

いくつかのオプションが選べる。

![options](/mailtracker/options.png)

"when opened for the first time"にチェックを入れておくと、以下のようなブラウザ通知が受け取れる。
![notification](/mailtracker/notification.png)

また、Dashboard 機能もあり、Gmail に依存せず開封したメールの一覧をチェックできる。
これが初期状態で、

![initial dashboard](/mailtracker/initial_dashboard.png)

開封後はこうなる。
![opened](/mailtracker/opened.png)

開封メールの詳細ページもある。ドキュメントを開いたかどうか、リンクをクリックしたかどうかも Upgrade すればわかるようだ。便利。
![opened details](/mailtracker/opened_detail.png)

メールを送信すると、このように開封したかどうかが Gmail でわかるようになっている。

![on gmail](/mailtracker/on_gmail.png)

受信者側の見た目はこんな感じ。
Free プランだと下部に強制的にフッターを入れられてしまうが、Upgrade で消すことができるらしい。課金ポイントがイケてる。これは課金せざるを得ない。

![receiver](/mailtracker/receiver.png)

### Campaign 機能で一斉送信

ここからが本題である。ほとんどの人はいちいち労働集約的にメールを個別送信したくないはずだ。

まず以下のような SpreadSheet を用意する。変数にしたい項目と、その値を書く。
![sheet](/mailtracker/sheet.png)

メール送信画面で、"Campaign"を押して、SpreadSheet を選択するとシートの連絡先が BCC にすべてぶち込まれる。  
後は変数を選択してテンプレメールを作る。
![template](/mailtracker/template.gif)

残すは送信するだけ！と思いたいところだが、きめ細かいサポートがある。  
メール送信前に自分のメールアドレスに、SpreadSheet の先頭のレコードを使ってプレビューメールを送信できる。  
一斉送信したら大変なミスをした！とならずに済むいい UX。  
"Send Campaign"を押すと実際に送信ができる。

![sending option](/mailtracker/sending_option.png)

プレビューメールはこんな感じ。
![preview](/mailtracker/preview.png)

あとは Dashboard で開封率を確認できる。(今回はすべて自分のメールアドレスに対してメールを送ったので 1 件のみになっているが、開封するとしっかり 100%になっていることがわかる)
![campaign](/mailtracker/campaign.png)
![open rate](/mailtracker/open_rate.png)

# その他のサービス

ほとんど CRM との統合で、ゼロワンのスタートアップ的にはそれなりにお値段がするものが多かった。一つだけシンプルな物があったので紹介しておく。

### [hunter MailTracker ↗](https://hunter.io/mailtracker)

最初に試したのは hunter MailTracker。  
こっちはまだ完全フリーで、シンプルに送ったメールの開封確認ができるだけでいいなら選択肢になりそう。

# 感想

実装したらそこそこの時間かかっただろうけど、SaaS を使うとお金を払って工数を節約できたいい事例。  
スタートアップで働くコツは、やらなきゃいけないことを「やらない」こと。  
知っている SaaS のレパートリーが多ければ多いほど本質的価値提供にフォーカスできるなと実感したので、定期的にリサーチして行きたい。
