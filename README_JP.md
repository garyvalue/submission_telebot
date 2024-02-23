# 一つの簡単なチャンネル投稿用のTelegramボット

**日本語 | [English](./README.md) | [简体中文](./README_ZH.md)**<br>

## 機能
- [x] 匿名で投稿できます。テキスト、画像、動画などを送信できます。
- [x] 审査が完了したら、実行者はユーザーに拒否または承認の理由を返信できます。
- [x] 複数言語対応。現在は中国語、英語、日本語が対応しています。
- [x] 管理者はユーザーを表示/禁止/禁止解除できます。
- [x] 管理者はBOT内でいくつかの設定を表示/変更できます。
- [x] 転送されたメッセージは匿名投稿できません
- [x] ユーザーは`/feedback`コマンドでフィードバックできます。
- [x] DOCKER対応

## 未実装
- [ ] 投稿制限。文字数制限、画像制限など。
- [ ] 投稿統計
- [ ] 投稿コメントでBOTを使用して発言する（匿名の場合に非常に便利です）


## 说明
1. どのような役割がありますか？

    管理者：BOT管理者は、部分設定を表示/変更できます。ユーザーを表示/禁止/禁止解除できます。

    审査員：投稿を承認/拒否できます。

    一般ユーザー：BOTを使用して投稿できます。

2. 管理者を追加するにはどうすればよいですか？

    サービスを起動する前に、`config.yml`の`super_admins`属性に設定します。複数の管理者は英語のカンマ`,`で区切ります。

3. 审査員を追加するにはどうすればよいですか？

    审査員をチャンネルに招待するだけです。チャンネルに招待されたユーザーはすべて承認者です。

4. デフォルト言語を設定するには

    `i18n.yml`の`langs`プロパティで構成し、デフォルト言語を最初に置きます

## 準備
1. [ここ](https://my.telegram.org/apps)でapi_idとapi_hashを取得します。
2. [@botfather](https://t.me/botfather)でBOTのTokenを取得します。
3. 审査用のチャンネルを準備します。プライベートチャンネルである必要があります。BOTをチャンネルに追加し、管理者権限を付与します。
4. 审査用のグループを準備します。プライベートグループである必要があります。チャンネルをリンクし、BOTをグループに追加し、管理者権限を付与します。
5. 展示用のチャンネルを準備します。BOTをチャンネルに追加し、管理者権限を付与します。承認された投稿はこのチャンネルに転送されます。
6. [@userinfobot](https://t.me/userinfobot)で上記のチャンネルとグループのIDを取得します。

## 部署

ログファイルパス：`appdata/submission_telebot.log`

データベースファイルパス：`appdata/submission_telebot.db`

### DOCKER部署(推奨)

`docker`を使用してサービスを起動するには、`docker`と`docker-compose`をインストールする必要があります。

1. `Dockerfile`、`docker-compose.yml`、`config.yml`ファイルをダウンロードする
```bash
wget https://raw.githubusercontent.com/hormones/submission_telebot/main/Dockerfile -O Dockerfile
wget https://raw.githubusercontent.com/hormones/submission_telebot/main/docker-compose.yml -O docker-compose.yml
wget https://raw.githubusercontent.com/hormones/submission_telebot/main/config_sample.yml -O config.yml
```
2. `config.yml`ファイルを変更する（または`docker-compose.yml`ファイルの環境変数を変更する）。コメントをよく読んで、正しい設定を入力してください
3. `docker-compose up -d`を実行してサービスを起動します。初回起動時にイメージを取得し、依存関係をインストールする必要があるため、しばらくお待ちください
4. `docker logs -f --since=3m submission_telebot`を実行してログを表示するか、`tail -f appdata/submission_telebot.log`を実行してログを表示する

### 手動部署

このBOTサービスを初めて実行するには、1、2、3を実行する必要があります。後続の実行は3のみです。

1. サーバーで次のコマンドを実行します（gitをインストールしてください，python >= 3.7.3）
```bash
git clone https://github.com/hormones/submission_telebot
cd submission_telebot
python3 -m venv venv
source venv/bin/activate
pip install -U pip setuptools
pip install -r requirements.txt

wget https://raw.githubusercontent.com/hormones/submission_telebot/main/config_sample.yml -O config.yml
```
2. `config.yml`ファイルを変更します（または環境変数を設定します）。コメントを読んで正しい設定を入力してください。
3. `source ./venv/bin/activate && nohup python main.py >/dev/null 2>&1 &`を実行してサービスを起動します。

## ユーザー
ユーザーはデフォルトで`/help`、`/lang`、`/feedback`コマンドを使用できます。`user_command`を変更してユーザーが使用できるコマンドを追加または削除できます。


BOT管理者はデフォルトですべてのコマンドを使用できます。変更できません。

| コマンド  | パラメータ               | デフォルトの権限範囲                           | 使用範囲                                                   | 説明                                                         |
| --------- | ------------------------ | ---------------------------------------------- | ---------------------------------------------------------- | ------------------------------------------------------------ |
| /help     | オプション、コマンド名   | すべての人が使用できます                       | BOT内で使用できます                                        | ヘルプを表示します。コマンド名を入力して、コマンドの詳細な使用方法を確認できます |
| /setting  | -                        | BOT管理者のみ使用できます                      | BOT内で使用できます                                        | 部分設定を表示/変更できます                                  |
| /lang     | -                        | 所有人均可使用                                 | BOT中可用                                                  | 多语言                                                       |
| /lang     | -                        | すべての人が使用できます                       | BOT内で使用できます                                        | 複数の言語                                                   |
| /feedback | 必須、フィードバック内容 | 一般ユーザーのみ使用できます                   | BOT内で使用できます                                        | 問題をフィードバックできます                                 |
| /reply    | 必須、返信内容           | 审査チャンネルのすべてのメンバーが使用できます | 审査チャンネルのメッセージにコメントするときに使用できます | 审査が完了した後、ユーザーが拒否または承認された理由を返信できます |

## ライセンス
MIT

## 例
[@submission_telebot](https://t.me/submission_telebot)

## 現在の問題点/改善点
- [ ] デスクトップバージョンでは、1つのメッセージに複数の画像を投稿すると、画像を圧縮しない場合、Telegramによって順番に送信され、投稿が複数のメッセージに分割されます（画像を圧縮すると、この問題は発生しません。現時点では、良い解決策はありません）

## その他
1. `config.yml`ファイルの設定はすべてシステム環境変数に設定できます。対応する設定がある場合、設定ファイルの設定が上書きされます
1. デフォルトの設定では、ユーザーは`/feedback`コマンドを使用してフィードバックを送信できます。`user_command`を変更してユーザーが使用できるコマンドを追加または削除できます。
1. 本プロジェクトの英語と日本語の翻訳は、[Github Copilot](https://github.com/features/copilot)プラグインの翻訳から来ています。問題がある場合は、issueを提出してください
1. もしカスタマイズ情報表示の必要がある場合は、`i18n.yml`ファイルの多言語設定を編集して、よりプロジェクトテーマに合わせることができます 
1. その他の問題や提案がある場合は、issueを提出してください
