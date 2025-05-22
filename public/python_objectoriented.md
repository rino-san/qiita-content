---
title: オブジェクト指向とは:学生時代の脳筋コードを卒業するための再学習メモ
tags:
  - ''
private: false
updated_at: ''
id: null
organization_url_name: null
slide: false
ignorePublish: true
---
# オブジェクト指向とは


宣言的

命令的

classとmoduleの決定的な違い
- input, outputの間で処理が維持する
- 接続されている
  

エージェントの実装におけるステート管理
- 長期メモリ→Stateを切り出す
- 短期メモリ→classの中に実装、Agentの中に実装する
classだと、同じclassでも10人分のエージェントが別々に作られ、Computerの別々のアドレス空間に作られる


今後の流れ
読める→リファクタリングする→別エージェントを作れる
configの切り離し、dynamoDBからとってこれる


抽象化のポイント
- プロジェクト固有のものはOSSなどに依存しないようにする
- プラグイン化


DDDっぽい
- ロジックを切り離して、ロジックをピュアにする
- 分かりやすいディレクトリに収めている
- エージェントは型がある
  - classifierが変わっても他にん用できる
  - config
  - tools
  - ライブラリに入ってほしくない

Agent Squad
- シンプルなOSS
- classifier
- 基本ルーター
- 軽いOSS→何でもできる
- エージェントを疎結合しない、Scaleするときに辛くなる

## オーバーライド

## 抽象化


## オブジェクト指向によってコードがどう変わるのか？
