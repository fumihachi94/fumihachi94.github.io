# MS Teams Tips

## pymsteamsでTeamsへの投稿を自動化する

1. 該当チャネルの「コネクタ」から"Incoming Webhook"を追加
2. 任意のWebHook名を選択（これが投稿者名として表示される）
3. 案内通りに進めWebHook URLを取得する
4. pymstamsをpipインストールする
   `pip install pymsteams`
5. プログラム例

Markdown形式
```py
import pymsteams

message = pymsteams.connectorcard(<URL>)
message.text(\
    "This is *Bold*. </br>" +\
    "This is _Italic_. </br>" +\
    "Thin is [Link](https://www.google.com)</br>")
message.send()
```

リンクを追加
```py
import pymsteams

message = pymsteams.connectorcard(<URL>)
message.text("[Google](https://www.google.com)")
message.send()
```

リンクボタンを追加（ボタンは投稿の最後尾にくる）
```py
import pymsteams

message = pymsteams.connectorcard(<URL>)
message.text("Add a link button.")
message.addLinkButton("Jump to Google.", "https://www.google.com")
message.send()
```

- [Python から Microsoft Teams へ投稿する - らくがきちょう](https://sig9.hatenablog.com/entry/2019/06/21/000000)
- [pyteams で送信したメッセージを Teams 上で確認してみる - らくがきちょう](https://sig9.hatenablog.com/entry/2019/06/30/000000)
- [pymsteams · PyPI](https://pypi.org/project/pymsteams/)