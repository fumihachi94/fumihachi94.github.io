---
layout: post
title:  "Deep Learning FAQ Search"
date:   2024-02-05 20:00:00 +0900
categories: DeepLearning
---

設計開発や評価の現場において、この仕様どうなってたっけ？このエラーのトラブルシュートどうやるんだっけ？といった場面に直面することは非常によくあること。こんな時すぐに情報を知りたいですよね？
最近はChatGPTなど非常に強力なAIツールが出てきましたが、一般知識の検索とかには使えますが、機密に関わる仕様や評価方法などについてはもちろん回答してくれません。では例えば、社内情報に特化した生成系AIをファインチューニングして作ったとしましょう。この場合でも、100%正しい回答が返ってくるとは限りません。仕様について聞いて、間違った回答がきてそれを信じてしまったら非常にまずいですね・・・。
ではどうすればいいか。このような正確性が求められるFAQは、やはり現時点では人力で作成するしかないと思います。でも、FAQを体系的に管理したとしてもその数が膨大になってくるし、管理だるいですよね。
なので、類似文書検索のNNを活用して入力したQに近しいFAQを探してきてくれる機能を作ろうと思います。
キーワード検索ではヒットしにくい文書についても検索できるようにすることを目標とします。

最終的には各種ドキュメント（PDF、Word、Markdownを想定）を取り込んむと、その内容に関するFAQ検索ができるようになること目指します。"Q＝ドキュメントの文脈的の区切り毎の内容"、"A＝元の文章の要約"という関係のリストを作成することを想定し、それをFAQ類似文書検索としてファインチューニングすることで実現を目指します。


# Index

<!-- @import "[TOC]" {cmd="toc" depthFrom=1 depthTo=6 orderedList=false} -->

<!-- code_chunk_output -->

- [Index](#index)
- [はじめに](#はじめに)
- [学習データ・環境](#学習データ環境)
- [画面左側のメニュー項目からカテゴリ名とFAQのURLを取得する](#画面左側のメニュー項目からカテゴリ名とfaqのurlを取得する)
  - [seleniumでexecutable_pathを使うとエラーになる（Ver4以降）](#seleniumでexecutable_pathを使うとエラーになるver4以降)
- [FAQ Noの一覧を取得する](#faq-noの一覧を取得する)
- [FAQの内容を取得する](#faqの内容を取得する)
  - [urllib.request.urlopenで取得した日本語が文字化けする](#urllibrequesturlopenで取得した日本語が文字化けする)
- [学習モデルの選定](#学習モデルの選定)
  - [いざデモ](#いざデモ)
- [DeepL APIを使って翻訳する](#deepl-apiを使って翻訳する)
- [ChatGPT APIを使って類似文章を生成する](#chatgpt-apiを使って類似文章を生成する)

<!-- /code_chunk_output -->


---

# はじめに

Sentence BERTを利用してFAQ情報をFine Tuningさせて、ばらつきのある"質問A"に対して、適切な"回答Q"を選択できるようなシステムを考えていきます。Sentence BERTとは、BERTをベースにより精度の高い文章の埋め込み表現を取得できるようFine Tuningする手法になります。

**【最終目標】**
各種仕様書・設計書などのドキュメントを入力すると、その内容に合ったFAQ検索ができるようになること

**【今回目標】**
サンプルのFAQに対し、ゆらぎのあるAを与えて適切なQが検索できるようになること


参考にしたサイト：（この記事わかりやすくて優秀）
[Sentence BERTをFine TuningしてFAQを類似文書検索してみる - Taste of Tech Topics](https://acro-engineer.hatenablog.com/entry/2023/01/16/120000)


上記サイトには載っていないが追加で試せそうなこと
- SBERTの損失関数の選定
- 損失関数にTriplet Lossを採用した場合の入力の与え方
- 事前学習モデルの選定
- 文章ベクトルの生成方法

# 学習データ・環境

**【学習データ】**
今回はテストとして、トヨタの[T-Connectに関するFAQサイト](https://tconnect.jp/faq/)のQA（823個）を学習データとして利用していきます。
また、Aのゆらぎに強くするためChatGPT3.5 APIを利用して、各質問Aの類似文書を5つ作成し、２つを学習データして、残り3つを評価データとして利用していきます。

Sentence BERTはロス関数の種類で入力が変わりますが、今回は`Triplet Loss`を使用します。

入力学習データ例：
```

```



# 画面左側のメニュー項目からカテゴリ名とFAQのURLを取得する

完成したプログラム
> get_tcon_faq_categorylist.py


selenium使って対応するしかなさそう

結局どうしたか

```python
from selenium import webdriver
from selenium.webdriver.chrome.service import Service as ChromeService
from selenium.webdriver.common.by import By
import time

URL='https://tconnect.jp/faq/'
chrome_driver = './driver/chromedriver/chromedriver.exe'

service = ChromeService(executable_path=chrome_driver)
driver = webdriver.Chrome(service=service)
driver.get(URL)
time.sleep(1)

elems = driver.find_elements(By.CLASS_NAME,'accordion-child')
for elem in elems:
    atag_elem = elem.find_element(By.TAG_NAME,'a')
    print(atag_elem.get_property("text")) #カテゴリの名称を取得
    print(atag_elem.get_property("href")) #カテゴリのURLを取得
```

- selenium4から仕様が変わったこと
[Selenium4系でのWebスクレイピング #Python - Qiita](https://qiita.com/hirotaka23/items/011ce524d97c50955296)
- `find_elemets`で取得したアイテムの中身を参照する方法
[【Python】SeleniumのCSSセレクターの使い方 | プログラミングで人生が楽になりました](https://posipochi.com/2021/05/03/python-scraping-css-selector/#toc14)

|指示|メソッド|備考|
|--|--|--|
|クリック|.click()|押せる要素に対して
|ドロップダウンを選択する|.select_by_index(indexnum)|Select要素に対してindexで選択
|ドロップダウンを選択する|.select_by_value(“value”)|Select要素に対してvalueで選択
|ドロップダウンを選択する|.select_by_visible_text(“text”)|Select要素に対して表示されているテキストで選択
|テキストを入力する|.send_keys(“strings”)|入力ボックスに対して
|テキストを取得する|.text|
|属性の値を取得する|.get_attribute(“value”)|“value”以外にも、取得したい属性名を指定出来る

- windowを非表示にする
[【Python】ブラウザを非表示のままスクレイピングselenium｜独学素人の限界突破Blog](https://prcmyself.com/selenium-scraping-with-browser-hidden)



## seleniumでexecutable_pathを使うとエラーになる（Ver4以降）

[【最新版】Seleniumでexecutable_pathを使うと警告文が出るとき【回避方法】 | くまねこPython塾](https://www.kumaneco.net/selenium-executable-warning/)

[Selenium4 WebDriver の executable_path 使用に伴う警告の回避方法 - ガンマソフト](https://gammasoft.jp/blog/python-selenium-webdriver-deprecation-warning-by-executable_path/)


# FAQ Noの一覧を取得する

上記で取得したFAQカテゴリ一覧を使って、今度は各カテゴリに存在するFAQ No.を取得していきます。
開発のために何度もサイトにアクセルするのはナンセンスなので、FAQカテゴリ一覧はcsvに出力したものを利用するようにします。






# FAQの内容を取得する

## urllib.request.urlopenで取得した日本語が文字化けする

最初、UNIX系OSで使われる日本語に対応した文字コード`euc_jp`を指定してみたんですが、以下のように完全に文字化け・・・。
（`title`タグ部分を抽出して確認）

```python
from urllib.request import urlopen
URL = 'https://tconnect.jp/faq/application/howtoappli/1001.html'
print(urlopen(URL).read().decode('euc_jp', 'ignore'))
```
> Output:
> `<h2 class="title">篁箙ｃT-Connect違顙激篁帥с障鐚</h2>`

【解決方法】

以下のように、取得したページの文字コードを取得してそれを使うことで正しく日本語で取得できるようになりました。

```python
from urllib.request import urlopen
URL = 'https://tconnect.jp/faq/application/howtoappli/1001.html'
res = urlopen(URL)
charset=res.info().get_content_charset()
print(res.read().decode('euc_jp', 'ignore'))
```

> Output:
> `<h2 class="title">今乗っているクルマのT-Connectナビを、新しく買うクルマに付け替えて利用することはできますか？</h2>`

※こちらの記事を参考にさせてもらいました。
[猫大好きダメおやじ日記 [Python]urllib.request.urlopen()で文字化けにハマった！](http://toshiboo73.blog.fc2.com/blog-entry-266.html)


---

# 学習モデルの選定

huggingfaceに登録されている日本語用のsentence bertの事前学習モデルを利用します。

[sonoisa/sentence-bert-base-ja-mean-tokens-v2 · Hugging Face](https://huggingface.co/sonoisa/sentence-bert-base-ja-mean-tokens-v2?text=%E3%82%AA%E3%83%97%E3%82%B7%E3%83%A7%E3%83%B3%E3%82%B5%E3%83%BC%E3%83%93%E3%82%B9%E3%81%AE%E7%94%B3%E8%BE%BC%E3%81%BF%E6%96%B9%E6%B3%95%E3%82%92%E6%95%99%E3%81%88%E3%81%A6%E3%81%8F%E3%81%A0%E3%81%95%E3%81%84)

## いざデモ

事前にfugashiとipadicをpip installしておく必要があります


---

# DeepL APIを使って翻訳する

Google Colab src:
- https://colab.research.google.com/drive/1L9CKhfeBa0p6-iE_hl68B-IfUb9AZd6B#scrollTo=4MC__C-6IDSh

参考サイト：
- [Google Colab ではじめる DeepL API Free｜npaka](https://note.com/npaka/n/n25107080a6bb)

DeepLにログイン
API > DeepL API Freeに登録（未登録の場合）
Free版は50万文字/月までご利用可能
アカウントページ下部のAPIキーを確認
https://www.deepl.com/ja/account/summary

Google Colabにログイン
Google Driveをマウント
[Google Colaboratoryでのファイル読み込み方法 | うしじのブログ](https://www.ushiji.online/colab-file-upload)
DeepLのAPIキーをColabのシークレット管理機能に追加
[【Googleコラボ】APIキーの管理が便利になった｜Non](https://note.com/noa813/n/nea9f5dfbe314)

![Google-Colab-Handle-API-KEY](..\assets\images\dn\2024-01-27_Google-Colab-Handle-API-KEY.png)

# ChatGPT APIを使って類似文章を生成する

ChatGPT APIは従量課金制です。料金体系はこちら。
[Pricing](https://openai.com/pricing)
英語で利用した方が安いので、日本語を英語に翻訳して利用するようにします。

ChatGPTにログイン
APIキー発行
https://platform.openai.com/api-keys
Google Colabにログイン
Google Driveをマウント
DeepLのAPIキーをColabのシークレット管理機能に追加
API利用方法
[Quickstart tutorial - OpenAI API](https://platform.openai.com/docs/quickstart?context=python)
モデルの選択
[Models - OpenAI API](https://platform.openai.com/docs/models)

`pip install --upgrade openai`

```python
from openai import OpenAI

# ローカルでAPIキーを読み出す場合
client = OpenAI(api_key=os.environ.get("OPENAI_API_KEY"))
# Google Colabで利用する場合
client = OpenAI(api_key=userdata.get("GPT_API_KEY"))
```


質問フォーマット
> Generate 5 sentences with different expressions, maintaining the original sentence structure. Ensure not to use words from the original text, vary sentence structures, and consider different phrasing preferences. Keep the expressions simple and concise.
>
>Sentence：

