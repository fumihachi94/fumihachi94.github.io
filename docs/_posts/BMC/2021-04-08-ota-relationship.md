# OTA技術の各社相関図

```mermaid
graph LR
    VW[fa:fa-car VW]
    FD[fa:fa-car Ford]
    TT[fa:fa-car TOYOTA]
    MB[fa:fa-car Mercedes]
    BS(fa:fa-cog Bosch) 
    WR(fa:fa-cog WindRiver)
    AQ(fa:fa-cog Airbiquity)
    NV(fa:fa-cog NVIDIA)
    MS(fa:fa-cloud MicroSoft)
    VL(fa:fa-car Volvo)
    GL(fa:fa-car Geely)

    VW --- L1(Ref. URL) --- MS
    MS --- L2(Ref. URL) --- BS
    MB --- L3(Ref. URL) --- NV
    WR --- L4(Ref. URL) --- FD
    WR --- AQ
    AQ --- TT
    VL --- GL
    
    click L1 "https://jidounten-lab.com/w_continental-ota-vw" "VW-MS"
    click L2 "https://jidounten-lab.com/u_bosch-microsoft-ota-opensource" "MS-Bosch"
    click L3 "https://monoist.atmarkit.co.jp/mn/articles/2006/24/news056.html" "Mercedes-Nvidia"
    click L4 "https://www.windriver.com/news/press/news-21606" "Ford-WindRiver"
    style L1 fill:#fff, stroke:#aaa, stroke-dasharray: 5
    style L2 fill:#fff, stroke:#aaa, stroke-dasharray: 5
    style L3 fill:#fff, stroke:#aaa, stroke-dasharray: 5
    style L4 fill:#fff, stroke:#aaa, stroke-dasharray: 5
```

<br>

- [Airbiquityがトヨタ自動車、デンソー、豊田通商、からの戦略的投資の受け入れを発表 :: Airbiquity](https://www.airbiquity.com/jp/news-press/press-releases/airbiquity)
- [ウインドリバーとAirbiquityが提携 | Airbiquityのプレスリリース | 共同通信PRワイヤー](https://kyodonewsprwire.jp/release/201906057175)
- [ボルボ、吉利系列の技術開発企業と合弁会社設立 次世代車載システム開発で協力 | 36Kr Japan | 最大級の中国テック・スタートアップ専門メディア](https://36kr.jp/126247/)

WindRiver
- [Wind River CarSync](https://cdn.windriver.com/japan/products/automotive/edge-sync/)
- [Ford Selects Wind River Over-the-Air Update Technology | Wind River](https://www.windriver.com/news/press/news-21606)
- [ウインドリバー、差分システム技術を搭載した「Edge Sync」の機能拡張を発表：フォードを含む大手自動車メーカーに供給 - TechFactory](https://techfactory.itmedia.co.jp/tf/articles/1811/28/news043.html#_ga=2.106457345.326697866.1617804165-1799664173.1605191265)

Airbiquity
- [Japanese :: Airbiquity](https://www.airbiquity.com/jp)

Mercedes
- [新型Sクラスがレベル3の自動運転に対応、2021年後半からドイツで利用可能に：自動運転技術（1/2 ページ） - MONOist](https://monoist.atmarkit.co.jp/mn/articles/2009/03/news049.html)

Toyota
- [トヨタ、遠隔ソフト更新で手放し自動運転機能を拡張　20年量産へ | 日経クロステック（xTECH）](https://xtech.nikkei.com/atcl/nxt/column/18/00001/03175/)
- [トヨタが新型LSで4つのLiDARを採用、高速道路出口まで運転支援：安全システム - MONOist](https://monoist.atmarkit.co.jp/mn/articles/2007/08/news051.html)

VW
- [Volkswagen ID.4 Already Gets Its First Major Software Upgrade | CarBuzz](https://carbuzz.com/news/volkswagen-id-4-already-gets-its-first-major-software-upgrade)

NIO
- [NIO explains its improved OTA capability brought by NIO OS 2.8.0 - CnTechPost](https://cntechpost.com/2020/12/11/nio-explains-its-improved-ota-capability-brought-by-nio-os-2-8-0/)


# 市場調査

## Tesla

**Ver.2021.4.15 ('21/4/11)**

所要時間：~25分
https://twitter.com/RedTeslaGirl/status/1380372679606296577/photo/1

**Ver.2021.4.11 ('21/3/12)**

所要時間：40-50分
https://teskas.net/forums/topic/42831/#post-42877

テスラアプリからのアップデートでは50%から進捗が進まない問題。
車載HMIから「ソフトウェア」をタップすることで進捗が再開した事象。
https://teskas.net/forums/topic/42831/#post-42839

リアカメラを表示した状態でも時計やナビ（次曲がるまでの距離）が表示されるようになりました。
https://teskas.net/forums/topic/42831/#post-42872

現在と同じバージョンのソフト更新の通知がくる問題
うまくインストールが出来なかった場合、サービスが再度プッシュすることがあるもよう。
ただし、ユーザーからの連絡があった場合に再配信されるとのこと。
更新エラーの場合は、アプリ経由で再プッシュする方法があるとこのこと。
https://teskas.net/forums/topic/42831/#post-43088


- [歩行者、自転車の認識結果も画面表示](https://teskas.net/forums/topic/42831/#post-42897)
- [通信速度の表示がある。](https://teskas.net/forums/topic/42831/#post-42881)
![](2021-04-11-19-18-04.png)


知らぬ間にアップデートが進まなくなるケースがある。
予約されたサービスセンターに電話したら、10月にエラーになっていて、アップデートが進まない状態になっていたことが発覚。
サービスセンタ側から配信ソフトのプッシュしてもらって解決となった。
https://teskas.net/forums/topic/30670/

これは言い得て妙だなw
>テスラの話ではないですが、とあるクラウドサービス会社の社長に「USばっかりアップデートが早くて、僕ら日本人はいつも後回しだからなんとかしてよ」と言ったら、「アメリカ人みんなで日本人のみんなのために初期不良を潰してるんだよ」と言いくるめられたことがありました。
物は言いようですね。
https://teskas.net/forums/topic/30670/#post-30717

アップデートの更新が60%でとまる問題。
コールセンタからの遠隔配信で旧ソフトバージョンにもどっていたとのこと。
その後の配信通知はすぐには来ていないもよう。
（バージョンだけおかしかったのか、ソフト内容自体もロールバック可能？）
https://teskas.net/forums/topic/30670/#post-31080


アップデートしたところ、オートライトが自動でも日中点灯しっぱなしになってしまいました。
https://teskas.net/forums/topic/30670/#post-31113

警告頻度があがったり、出来たことができなくなるとユーザーは不満を感じる
テスラのその例
https://teskas.net/forums/topic/30670/#post-31241


# Tesla

Tesla内部カメラの見えているデータの動画 (twiter)
https://twitter.com/i/status/1380160809700065285




