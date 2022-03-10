---
title:  "AWS learning log"
categories: Note
tag: aws log
---


<!-- @import "[TOC]" {cmd="toc" depthFrom=1 depthTo=6 orderedList=false} -->

<!-- code_chunk_output -->

- [EC2 : Elastic Compute Cloud](#ec2-elastic-compute-cloud)
- [EBS : Elastic Block Store](#ebs-elastic-block-store)
- [S3 : Simple Storage Service](#s3-simple-storage-service)
- [SQS : Simple Queue Service](#sqs-simple-queue-service)

<!-- /code_chunk_output -->


# EC2 : Elastic Compute Cloud

クラウド(AWS)上に仮想サーバーを構築して、自由にカスタマイズし利用できるサービス。
ざっくり、１インスタンスで１つのパソコンみたいなものです。

![](..\assets\images\aws\aws_ec2_system_img_001.png)
[チュートリアル: Amazon EC2 Linux インスタンスの開始方法 - Amazon Elastic Compute Cloud](https://docs.aws.amazon.com/ja_jp/AWSEC2/latest/UserGuide/EC2_GetStarted.html)

- インスタンスとは
  - 仮想サーバーの単位。用途に合わせて、様々な種類の環境が用意されている。
  - 例えばLinuxやWindowsなどのOS環境が利用できる
- AMIとは
  - AMI : Amazon Machine Image
  - ソフトウェア構成 (オペレーティングシステム、アプリケーションサーバー、アプリケーションなど) を記録したテンプレートのこと
  - [インスタンスと AMI - Amazon Elastic Compute Cloud](https://docs.aws.amazon.com/ja_jp/AWSEC2/latest/UserGuide/ec2-instances-and-amis.html)
- キーペアとは
  - 公開鍵暗号方式で用いる公開鍵と秘密鍵の組のこと
  - インスタンス1つに対して1つのキーペア（公開鍵・秘密鍵）が作成されます
  - プライベートキーはpemファイルと呼ばれる****.pemというファイル
  - [AWS の KeyPair(キーペア) の意味や働きがよくわかってなかったので整理した - stamemo](https://stakiran.hatenablog.com/entry/2019/05/22/203921)
- セキュリティグループとは
  - 各インスタンスの通信アクセス権限を定義したグループのこと
  - Virtual Private Cloud（VPC）を保護する主要なファイヤーウォール
  - [AWSセキュリティグループとは？セキュリティーグループの設定方法やメリットを紹介！ | FEnet AWSコラム](https://www.fenet.jp/aws/column/aws-beginner/340/)
- Elastic IP アドレスとは

# EBS : Elastic Block Store

[Amazon Elastic Block Store (Amazon EBS) - Amazon Elastic Compute Cloud](https://docs.aws.amazon.com/ja_jp/AWSEC2/latest/UserGuide/AmazonEBS.html)

EC2インスタンスで利用できるストレージサービスです。
ファイルシステムを構築でき、ハードディスクと同じような形で利用できます。

ストレージ種類

|     | SSD             |                  | HDD       |      |
| --- | --------------- | ---------------- | --------- | ---- |
|     | General Purpose | Provisioned IOPS | スループット最適化 | Cold |
| 種類  | gp3             | io2Block Express | st1       | st2  |
|     | gp2             | io2              | -         | -    |
|     | -               | io1              | -         | -     |

# S3 : Simple Storage Service

自動的に容量が拡張される、容量無制限なＦＴＰサーバーのようなもの
0.025ドル/GB(東京リージョン, 2021/4時点)程度で利用できる安さと、99.999999999%の耐久性があることが売りです。

[【エンジニア向け】よく聞くAmazon S3とは｜ただのストレージじゃないの？ | PINTO! by PLAN-B](https://service.plan-b.co.jp/blog/creative/4981/)

# SQS : Simple Queue Service

> キューイングとは異なるソフトウェア間でデータの送受信する手法の一つで、
ソフトウェア間を直接データを渡すのではなく、第三者経由でデータを渡すことで、送信側と受信側が好きなときに処理を行うことができます。
そのキューイングができるサービスがSQSになります。
[Amazon SQSとは - Qiita](https://qiita.com/miyuki_samitani/items/e46ef9452fcd73f9d240)

![](..\assets\images\aws\aws_sqs_system_img_001.png)
[AWS再入門ブログリレー Amazon SQS編 | DevelopersIO](https://dev.classmethod.jp/articles/re-introduction-2020-amazon-sqs/)
