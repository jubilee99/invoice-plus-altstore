# AltStore 配布リポジトリ セットアップガイド

このガイドでは、`altstore_distribution` ディレクトリの内容をGitHub Pagesで公開する手順を説明します。

## 📁 準備完了ファイル

以下のファイルが `altstore_distribution/` ディレクトリに準備されています:

```
altstore_distribution/
├── apps.json                           # AltStore設定ファイル
├── README.md                           # ユーザー向けドキュメント
├── ipa/
│   └── invoice_mobile_v1.0.0.ipa      # アプリ本体 (9.9MB)
└── assets/
    └── icon.png                        # アプリアイコン
```

## 🚀 GitHub リポジトリのセットアップ

### Step 1: Public リポジトリの作成

1. GitHubにアクセス
2. 新しいリポジトリを作成:
   - **Repository name**: `invoice-plus-altstore`
   - **Visibility**: **Public** (重要!)
   - **Initialize**: README は追加しない (既に準備済み)

### Step 2: ローカルリポジトリの初期化

```bash
cd /volume1/docker/invoice/mobile_app/altstore_distribution

# Gitリポジトリを初期化
git init

# すべてのファイルを追加
git add .

# 初回コミット
git commit -m "Initial release: invoice+ v1.0.0"

# リモートリポジトリを追加 (ORGとREPO_NAMEを実際の値に置き換え)
git remote add origin https://github.com/<ORG>/invoice-plus-altstore.git

# メインブランチにプッシュ
git branch -M main
git push -u origin main
```

### Step 3: GitHub Pages の有効化

1. GitHubリポジトリページにアクセス
2. **Settings** タブを開く
3. 左サイドバーから **Pages** を選択
4. **Source** セクションで:
   - **Branch**: `main` を選択
   - **Folder**: `/ (root)` を選択
5. **Save** をクリック

数分後、以下のURLでアクセス可能になります:
```
https://<ORG>.github.io/invoice-plus-altstore/
```

### Step 4: apps.json の URL 更新

`apps.json` 内の `<ORG>` プレースホルダーを実際の組織名/ユーザー名に置き換えてください:

```bash
# macOS/Linux
sed -i 's/<ORG>/your-org-name/g' apps.json

# または手動で編集
nano apps.json
```

置き換え後、再度コミット&プッシュ:

```bash
git add apps.json
git commit -m "Update organization name in apps.json"
git push
```

## 📱 AltStore での追加方法

GitHub Pages が有効化されたら、以下のURLをAltStoreに追加します:

```
https://<ORG>.github.io/invoice-plus-altstore/apps.json
```

### ユーザー向け手順

1. AltStore アプリを開く
2. **Browse** タブ → 右上の **+** アイコン
3. 上記URLを入力
4. **TigerRack Apps** ソースが追加される
5. **invoice+** をインストール

## 🎨 追加推奨アセット (オプション)

より魅力的な配布ページにするため、以下のアセットを追加することをお勧めします:

### 1. スクリーンショット

アプリのスクリーンショットを撮影し、`assets/` に配置:

```bash
# 例
assets/screenshot1.png  # ログイン画面
assets/screenshot2.png  # スキャン画面
```

### 2. ヘッダー画像

ソースのヘッダー画像 (推奨サイズ: 1200x400px):

```bash
assets/header.png
```

### 3. ニュースバナー

リリース告知用のバナー画像:

```bash
assets/news_banner.png
```

これらのファイルを追加後、再度コミット&プッシュしてください。

## 🔄 アプリの更新手順

新しいバージョンをリリースする場合:

### 1. 新しいIPAを追加

```bash
cp ../invoice_mobile.ipa ipa/invoice_mobile_v1.1.0.ipa
```

### 2. apps.json を更新

- `version`: 新しいバージョン番号
- `versionDate`: リリース日
- `versionDescription`: 変更内容
- `downloadURL`: 新しいIPAのURL
- `size`: 新しいIPAのファイルサイズ

### 3. コミット & プッシュ

```bash
git add .
git commit -m "Release v1.1.0: [変更内容の概要]"
git push
```

AltStoreは自動的に新しいバージョンを検出し、ユーザーに更新を通知します。

## ⚠️ 重要な注意事項

### ファイルサイズ制限

- GitHub Pages は **1ファイル100MB** まで
- リポジトリ全体で **1GB** まで
- 現在のIPA (9.9MB) は問題なし

### Raw URL の使用

`apps.json` 内のURLは必ず **raw.githubusercontent.com** を使用してください:

```
https://raw.githubusercontent.com/<ORG>/invoice-plus-altstore/main/apps.json
```

GitHub Pages のURL (`github.io`) ではなく、Raw URLを使用することで、AltStoreが正しくファイルを読み込めます。

### プライベートリポジトリは不可

AltStore がアクセスするため、リポジトリは **Public** である必要があります。

## 🔒 セキュリティ考慮事項

- IPAファイルは署名なしでビルドされているため、AltStoreが再署名します
- 社内ネットワーク外からもダウンロード可能になります
- アプリ自体は社内サーバー (`192.168.24.151:9005`) にしか接続しないため、外部からの不正利用リスクは低い
- 必要に応じて、GitHub Actionsでプライベートリリースを管理することも検討可能

## 📊 配布状況の確認

GitHub Insights で以下を確認できます:

- **Traffic**: ページビュー数
- **Clones**: リポジトリのクローン数
- **Releases**: ダウンロード数 (Releaseを使用する場合)

---

**次のステップ**: このガイドに従ってGitHubリポジトリをセットアップしてください。
