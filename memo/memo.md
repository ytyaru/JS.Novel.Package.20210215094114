# 小説作品のディレクトリ・ファイル構造を定義する

　Web上で小説を公開する。このときどんなデータが必要か。どんなディレクトリやファイルとして実装するかを定義する。自動化するための礎とする。

# データ

　小説作品に必要なデータを考える。

* 著者
    * 固定
    * 集計
* 読者

## 著者による固定メタデータ

項目|英名|概要
----|----|----
本文|content|日本語テキスト。
題名|title|作品名。
著者|author|著者名。

## 著者による集計メタデータ

項目|英名|概要
----|----|----
作成日|created|作品を作成した最初の日付
公開日|published|作品を公開した最初の日付
更新日|updated|作品を公開した最後の日付
文字数|charactorCount|作品全体の文字数

### セルフレイティング

　著者が読者にむけて注意を促すラベルのこと。特定の表現を苦手とする人にむけて読む前に警告する。R15。

項目|英名|概要
----|----|----
暴力描写あり||
性的描写あり||
残酷描写あり||

　レイティングには正解がない。基準も曖昧だ。人それぞれ常識や基準がちがう。趣味嗜好もしかり。よってレイティングはあくまで著者の独断と偏見と主観にもとづく。つまるところ不快に思う人がこの作品を読むことを避けるための警告である。刺さる対象者を狙い撃つの逆だ。

　ところで、一切の暴力、性的、残酷な描写がない小説は、どれくらいあるだろうか。大抵ミステリーではなんちゃら殺人事件といって人が殺される暴力、残酷な描写がある。もしくは事実としては人が死ぬけど、描写としてはオブラートに包んで生々しい描写を回避すればいいのかもしれない。しかしそこで想像できてしまったらアウトなわけで。そこは受け手次第でもある。著者はどこまで想定できるのか。すべきなのか。あくまで著者が自分基準で判断するしかない。

### インデックス系

項目|英名|概要
----|----|----
カテゴリ|category|
キーワード|keyword|
タグ|tag|

## 読者による固定メタデータ

項目|英名|概要
----|----|----
いいね|good|ボタンを押して評価する
評価|rate|★5つのうちいくつかで採点する
コメント|comment|感想などのテキストを送る
プルリクエスト|pullRequest|誤字などの修正案を送る

　読者メリットとしては自分が過去によいと思った作品や場面をリストアップできること。

## 読者による集計メタデータ

項目|英名|概要
----|----|----
PV|PV|プレビュー数。
二次創作数|fanFictionCount|他者がつくった二次創作作品数

# 作品構成データ

* 作品ディレクトリ
    * メタデータファイル
    * 本文データファイル

　作品を検索するときはメタデータファイルのみ読み込む。

　作品を閲覧するときは本文データファイルを順次読み込む。

　各ディレクトリ名やファイル名はID番号とする。

## 構成パターン

### 規模

　作品の規模によって適切な構成がある。

規模|文字数
----|------
掌編|300〜800
SS|800〜8000
短編|4000〜32000
長編|40000〜120000

### 目的

　作品を公開する目的によって適切な構成がある。

目的|概要|作者|読者
----|----|----|----
練習|著者が文章を書く練習をする|書く|読む
練習|皆で文章を書く練習をする|読む,書く|読む,書く
自己満足|著者が自己満足で作品を公開する|書く|読む
自己PR|著者が周囲に自分の能力をアピールする|書く,魅せる|読む
集客|人を集める|書く,魅せる|読む
営利|人に作品を書かせてコンテンツにする|プラットフォームを作る|書く,読む

### 通信

　著者と読者の通信方法によって適切な構成が決まる。

通信|サーバ保存|ローカル保存|概要
----|----------|------------|----
ダウンロード|なし|なし|クライアントが固定アプリをダウンロードする。
ダウンロード|なし|あり|クライアントが固定アプリをダウンロードする。以降ローカル内で保存する。
ダウンロード|あり|なし|クライアントが固定アプリをダウンロードする。部分的にデータをサーバへ保存する。ユーザ間で共有できる。
ダウンロード|あり|あり|クライアントが固定アプリをダウンロードする。部分的にデータをサーバへ保存する。ユーザ間で共有できる。共有不要なユーザ固有データはローカル保存する。
ダウン,アップ|なし|なし|クライアントが更新されうるページと双方向通信する。保存はなし。
ダウン,アップ|なし|あり|クライアントが更新されうるページと双方向通信する。保存はローカル。
ダウン,アップ|あり|なし|クライアントが更新されうるページと双方向通信する。保存はサーバ。ユーザ間で共有できる。
ダウン,アップ|あり|あり|クライアントが更新されうるページと双方向通信する。保存はサーバ。ユーザ間で共有できる。共有不要なユーザ固有データはローカル保存する。

　HTML,JSならService Worker APIと、Cache APIを使ってWebアプリ形式にするとよい。

## contents

* contents/
    * meta.txt
    * 作品1/
        * meta.txt
        * 0.txt
        * 1.txt
    * 作品2/
        * meta.txt
        * 章1/
            * meta.txt
            * 0.txt
            * 1.txt

### contents/meta.txt

　全作品のメタデータ。おもに作品IDと作品名を紐づけている。

```tsv
ID  作品名  字数    日付    カテゴリ    タグ    キーワード
```

* 客観事実
    * ID
    * 作品名
    * 字数
    * 日付
* 主観分類
    * レイティング
    * ジャンル（芸術表現の分類）
    * カテゴリ（物事の性質を区分する）
    * タグ（キーワード）

### contents/作品/meta.txt

### contents/作品/章/meta.txt

## 本文データファイル

　1ファイルあたり2000文字程度を指標とする。

　ファイル名は`0`か`1`からはじまる連番。拡張子は`txt`。

### 章立て

　ディレクトリ構造が章になる。

* 作品ID/
    * 章ID/
        * 章メタデータ.txt
        * 本文ID.txt

　章名の先頭は`0`か`1`からはじまる連番。

