---
title: "ナレッジが0のチームにwikiを導入した話"
emoji: "💭"
type: "tech" 
topics: ["wiki","growi","team"]
published: true
---
半年ほど前からチームを異動し、別チームに入ってチームリーダーになりました。
新しく入ったチームも情報を整備されていなく、 知識に偏りがあったり
手順が統一されていなかったりとかなり品質に影響がある状態でした。
チームリーダーになったタイミングでそのあたりを整備していったので書き起こします。

# 要件
まずは前のチームの状況を踏まえて新しいチームの状況と必要なものを整理しました。
## 現状
* ドメインや技術の情報がチーム内で共有されていない。
* 各作業の手順などが整理されておらず属人化している。
* 各メンバーの課題やその解決法が共有されていない。
## 理想
* メンバー間で情報が共有できていること。
* 手順や必要な情報がまとまって管理されており、参照されること。 
* メンバーの抱えている課題や、その解決法が参照できること。
## 制限事項
* クラウドなどの社外に情報を保持するサービスはNG、オンプレであること。
* チーム外のメンバーが見れない状況にすること。
* コンテナで運用可能なこと。
* マークダウンで記述できること。

# どのプラットフォームにしたか
進捗管理は既に別プラットフォームで管理されていたので、情報や課題の解決策をメンバー間で共有できることが最優先だと考えました。
結果として[Growi](https://growi.org/ja/)をDockerで立ち上げて利用することにしました。

# 運用方法
## ページ構成
チーム間での様々な情報伝達ができていなかったので、まずは情報を整理するためのページ構成を考えました。
各トップページからサブページに各情報をサブページで作成していく流れです。
```
/root
  /議事録　　　→　各MTGの議事メモ
  /課題　　　　→　各課題の詳細を記載する
  /メンバー　　→　メンバー各個人のページ
    /Aさん
    /Bさん
    /Cさん
  /Tech　　　 →　各技術のメモ
  /Tutorial　 →　各種手順など
  /Scrap　　　→　記事などのスクラップ
```

## 運用にあたってのルール
一旦まとまった情報を集めたかったので以下の場合はページを記載するルールで最初の1ヶ月は運用しました。
* wikiに記載のない事柄に取り組む場合
* 課題が発生した場合
* 新たに調べる事柄が発生した場合
* メンバー間のメモ

# 半年運用した結果
ルールも少しづつ変えながら半年間運用した結果、以下の結果を得ることができました。
* 情報が1つのプラットフォームに集約されたので、過去のデータが検索で参照しやすくなった。
* 情報を書くハードルが下がったため、細かな情報も上がるようになった。
* 課題の解決法を記載するようになったので同じエラーなどで悩む時間が減った。
* 記事をスクラップするためメンバーの知識が底上げされた。

また以下の課題も見つかりました。
* 課題に対しての解決策がすぐ見つかってしまうため根本的な原因について理解できてないケースがあった。
* Redmineなどの他のプラットフォームとの連携するための手間が増えてしまった。
* メンバーによってページを書く頻度の差が出るようになってしまった。

見つかった課題を解決するために今後も改善していきたいと思います。


