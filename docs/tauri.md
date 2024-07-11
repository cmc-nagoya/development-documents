# Tauri

## 目次

- [Tauri](#tauri)
  - [目次](#目次)
  - [はじめに](#はじめに)
  - [Tauriとは](#tauriとは)
    - [特徴と利点](#特徴と利点)
    - [Electronとの比較](#electronとの比較)
  - [環境](#環境)
    - [必要なソフトウェア](#必要なソフトウェア)
    - [対応プラットフォーム](#対応プラットフォーム)
  - [セットアップ](#セットアップ)
    - [インストール手順](#インストール手順)
    - [プロジェクト初期化](#プロジェクト初期化)
  - [開発](#開発)
    - [フロントエンド開発](#フロントエンド開発)
    - [バックエンド開発（Rust）](#バックエンド開発rust)
    - [APIの利用](#apiの利用)
  - [ビルド](#ビルド)
    - [開発ビルド](#開発ビルド)
    - [本番ビルド](#本番ビルド)
  - [デプロイ](#デプロイ)
    - [パッケージング](#パッケージング)
    - [自動更新](#自動更新)
  - [セキュリティ考慮事項](#セキュリティ考慮事項)
  - [パフォーマンス最適化](#パフォーマンス最適化)
  - [Tips](#tips)
  - [トラブルシューティング](#トラブルシューティング)
  - [FAQ](#faq)
  - [参考リソース](#参考リソース)
    - [公式ドキュメント](#公式ドキュメント)
    - [コミュニティリソース](#コミュニティリソース)
    - [チュートリアルとサンプルプロジェクト](#チュートリアルとサンプルプロジェクト)

## はじめに

Tauriは、Webテクノロジーを使用してクロスプラットフォームデスクトップアプリケーションを構築するための最新のフレームワークです。このガイドでは、Tauriの基本から高度な使用方法まで、包括的な情報を提供します。

## Tauriとは

Tauriは、HTML、CSS、JavaScriptを使用してデスクトップアプリケーションを作成するためのツールキットです。バックエンドにRustを使用し、フロントエンドに任意のWebフレームワークを使用できます。

### 特徴と利点

- 軽量：最小限のバイナリサイズ（約10MB）
- セキュア：デフォルトで安全な設定
- パフォーマンス：Rustによる高速な実行
- クロスプラットフォーム：Windows、macOS、Linuxに対応
- カスタマイズ可能：必要な機能のみを含めることが可能
- 移植性: ロジックをRustで記述することで、他言語からのFFIを介した利用や、WebAssemblyによる移植が可能

### Electronとの比較

| 特徴 | Tauri | Electron |
|------|-------|----------|
| 言語 | Rust + Web技術 | Node.js + Web技術 |
| メモリ使用量 | 低 | 高 |
| バイナリサイズ | 小（〜10MB） | 大（〜100MB） |
| セキュリティ | デフォルトで高 | 設定が必要 |
| 学習曲線 | やや急（Rust） | なだらか（JavaScript） |

## 環境

### 必要なソフトウェア

- Rust
- Node.js
- プラットフォーム固有の依存関係（例：Windows上ではVisual Studio Build Tools）

### 対応プラットフォーム

- Windows 7以降
- macOS 10.13以降
- Linux（Debian 10、Ubuntu 18.04以降など）

## セットアップ

### インストール手順

[公式はこちら](https://www.rust-lang.org/ja/tools/install)

1. Rustをインストール：

   ```bash
   # WSL
   curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
   ```

2. Node.jsをインストール（公式サイトから）

### プロジェクト初期化

1. Tauriセットアップ：

   ```bash
   npm create tauri-app@latest <プロジェクト名>

    # 質問に答える
    ✔ Project name · [任意のプロジェクト名を入力]
    ✔ Choose which language to use for your frontend · TypeScript / JavaScript - (pnpm, yarn, npm, bun)
    ✔ Choose your package manager · npm
    ✔ Choose your UI template · Vanilla
    ✔ Choose your UI flavor · JavaScript
   ```

   > [!NOTE]
   > プロジェクト作成は`bash`,`Powershell`,`Cargo`,`yarn`,`pnpm`,`bun`でも可能です。
   > 詳しくは[公式ドキュメント](https://tauri.app/)を参照してください。

2. プロジェクトディレクトリに移動：

   ```bash
   cd <プロジェクト名>
   ```

3. 依存関係のインストール：

   ```bash
   npm install
   ```

4. 開発サーバーの起動：

   ```bash
   npm run tauri dev
   ```

## 開発

### フロントエンド開発

Tauriは任意のWebフレームワーク（React、Vue、Svelteなど）と組み合わせて使用できます。

例（React）：

```jsx
import { useState } from 'react'
import { invoke } from '@tauri-apps/api/tauri'

function App() {
  const [greeting, setGreeting] = useState('')

  async function greet() {
    // invoke関数を使用してバックエンドの関数を呼び出す
    setGreeting(await invoke('greet', { name: 'World' }))
  }

  return (
    <div>
      <button onClick={greet}>Greet</button>
      <p>{greeting}</p>
    </div>
  )
}

export default App
```

### バックエンド開発（Rust）

Rustを使用してネイティブ機能を実装します。

例（src-tauri/src/main.rs）：

```rust
#[tauri::command]
fn greet(name: &str) -> String {
    format!("Hello, {}!", name)
}

fn main() {
    tauri::Builder::default()
        // greet関数をフロントエンドに公開
        .invoke_handler(tauri::generate_handler![greet])
        .run(tauri::generate_context!())
        .expect("error while running tauri application");
}
```

### APIの利用

Tauriは様々なAPIを提供しています：

- ファイルシステム操作
- HTTP要求
- システム情報の取得
- ウィンドウ管理
- 通知機能

フロントエンドは[こちらを](https://tauri.app/v1/api/js/)参照してください。\
バックエンドは[こちらを](https://docs.rs/tauri/1.7.1/tauri/)参照してください。

例（ファイル読み込み）：

```javascript
import { readTextFile } from '@tauri-apps/api/fs';

async function readFile() {
  const contents = await readTextFile('path/to/file.txt');
  console.log(contents);
}
```

例（通知表示）：

```rs
use tauri::api::notification::Notification;

// タイトルと本文を指定して通知を表示する
Notification::new(&app.config().tauri.bundle.identifier)
  .title("New message")
  .body("You've got a new message.")
  .show();

```

## ビルド

### 開発ビルド

開発中は以下のコマンドを使用：

```bash
npm run tauri dev
```

### 本番ビルド

リリース用のビルドは以下のコマンドで作成：

```bash
npm run tauri build
```

> [!CAUTION]
> tauri.conf.jsonの`identifier`を初期値だとビルドに失敗するため変更が必要です。

## デプロイ

### パッケージング

Tauriは自動的に各プラットフォーム用のインストーラーを生成します。

- Windows: MSI, EXE
- macOS: DMG, APP
- Linux: AppImage, Deb, RPM

### 自動更新

Tauriは自動更新機能を提供しています。`tauri.conf.json`で設定：

```json
{
  "tauri": {
    "updater": {
      "active": true,
      "endpoints": [
        "https://releases.myapp.com/{{target}}/{{current_version}}"
      ],
      "dialog": true,
      "pubkey": "YOUR_PUBKEY"
    }
  }
}
```

## セキュリティ考慮事項

- CSP（Content Security Policy）の適切な設定
- 必要最小限の権限のみを付与
- ユーザー入力の適切な検証
- 定期的な依存関係の更新

## パフォーマンス最適化

- 非同期処理の活用
- メモリリークの防止
- 適切なキャッシング戦略の実装
- バンドルサイズの最小化

## Tips

- `tauri.conf.json`を活用してアプリケーションをカスタマイズ
- デバッグにはDevToolsを使用（開発モードで自動的に有効）\
    `Ctrl + Shift + I`で開く

## トラブルシューティング

- ビルドエラー：依存関係が最新かチェック
- 起動しない：ログを確認（`~/.tauri/dev-logs`）
- パフォーマンス問題：プロファイリングツールを使用

## FAQ

Q: Tauriは商用利用可能ですか？
A: はい、MITライセンスで提供されています。

Q: モバイルアプリの開発は可能ですか？
A: 現在のところ、デスクトップアプリケーションのみをサポートしています。

## 参考リソース

### 公式ドキュメント

- [Tauri公式サイト](https://tauri.app/)
- [API リファレンス](https://tauri.app/v1/api/js/)

### コミュニティリソース

- [GitHub リポジトリ](https://github.com/tauri-apps/tauri)
- [Discord コミュニティ](https://discord.gg/tauri)

### チュートリアルとサンプルプロジェクト

- [Tauri公式チュートリアル](https://tauri.app/v1/guides/getting-started/setup)
- [Awesome Tauri](https://github.com/tauri-apps/awesome-tauri)

このガイドを通じて、Tauriを使用したデスクトップアプリケーション開発の基礎から応用まで理解できるはずです。さらに詳細な情報や最新のアップデートについては、公式ドキュメントを参照してください。
