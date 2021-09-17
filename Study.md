# Nuxt.jsの基礎
[Nuxt.jsチュートリアル](https://nuxtjs.org/ja/docs/get-started/installation)

## ルーティング
/pagesの.vueファイル名を元に自動追加

### ナビゲーション
- aタグ：リロードあり
- NuxtLinkコンポーネント：リロード無しでページ遷移。aタグに変換されている。

## ディレクトリ構造
- components：コンポーネントファイル
- assets：スタイル・画像・フォントなどコンパイルされていないファイル
- static：直接サーバーのルートに配置され、名前やファイル自体が変更されない可能性のあるファイル。
- nuxt.config.js：Nuxt.jsの設定ファイル。

その他いろいろあるが小規模では使わなくてもよい。
(layout, storeなど)

## コマンドと開発
コマンドはpackage.jsonに記述する。

```
"scripts": {
  "dev": "nuxt",
  "build": "nuxt build",
  "start": "nuxt start",
  "generate": "nuxt generate"
}
```

実行コマンド↓
```
npm run <command>
```

コマンド一覧
[リンク](https://nuxtjs.org/ja/docs/get-started/commands#%E3%82%B3%E3%83%9E%E3%83%B3%E3%83%89%E4%B8%80%E8%A6%A7)

targetによって異なるコマンドを実行できる。
<font color="orange">staticに対してnuxt startコマンドは静的ホスティング(Vercel)などと同じように/distディレクトリを作成するため、デプロイ前のテストに最適。</font>

# コンセプト
## View
Nuxt.js アプリケーション内の特定のルートのデータとビューを設定するために必要なこと。
ビューは、アプリテンプレート、レイアウト、ページで構成されている。

### ページコンポーネント
ページコンポーネントのプロパティはたくさんNuxt独自のものが用意されている。
例えば、head()はそのページ向けのメタタグの設定など。