## アプリケーション
### いっしょにタイマー (Timer Together)
./nuxt-prc
> リアルタイム・タイマー共有アプリケーション

米ハーバード大学が公開しているコンピュータサイエンス入門講座「[CS50x](https://cs50.jp/)」の[FinalProject](https://cs50.jp/x/2021/final-project/)で制作した、Webアプリ「[いっしょにタイマー](https://youtu.be/CxgfTfbmOSY)」をNuxt.js・Vue.jsを使って再度作成中。

#### 作成のきっかけ
「いっしょにタイマー」はオンライン勉強会特有の以下の問題を解決するために作成しました。
- 「集中していると『シーン』と静まり返った雰囲気になってしまい、いっしょに勉強している感覚がない」
- 「その結果、複数人で集まって勉強することによるモチベーションが生まれにくい」
- 「それぞれが何を行っているのか、リアルタイムで把握しにくい」
- 「そのため、勉強中のコミュニケーションがしづらい・会話を始めるタイミングが分からない」

#### 機能など
**CS50x[ファイナルプロジェクト](https://youtu.be/CxgfTfbmOSY)で作成したもの**
(今回はNuxt・Vueで完全に作り直します)
- タイマー・タスクのリアルタイム共有機能
  - 実行中のタスク名
  - 実行中のタイマーの進捗(リアルタイム)
  - 自分のニックネーム
- アカウント作成
  - ユーザー登録
  - ログイン・ログアウト
- ルーム機能
  - ルームの作成・共有・削除

**新たに追加予定**
- ルームメンバーがいっしょに利用できるポモドーロタイマー機能
- ステータス(離席中・休憩中など)表示機能

※2021年9月27日現在・作成中

#### 技術スタック
- Nuxt.js
- WebSocket
- node v14.17.6 (LTS)
- npm v6.14.15
- nvm

#### 開発環境
- Windows10
- WSL2(Ubuntu)
- VS CODE
