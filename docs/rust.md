# Rust

- [Rust](#rust)
  - [Rustとは](#rustとは)
  - [特徴](#特徴)
    - [用途](#用途)
    - [例](#例)
  - [インストール](#インストール)
  - [学習](#学習)
  - [ライブラリ、フレームワーク](#ライブラリフレームワーク)
  - [ドキュメント](#ドキュメント)

## Rustとは

Rustとは、Mozillaが開発したシステムプログラミング言語です。\
Rustは、安全性、並行性、および実用性を重視して設計されており、\
メモリ安全性を保証するための機能が組み込まれています。
  
## 特徴

- メモリ安全性\
  Rustは、メモリ安全性を保証するための機能が組み込まれています。\
  具体的には、所有権システム、借用システム、およびライフタイムシステムがあります。\
  実際に、Rustはメモリリークやダングリングポインタなどのバグをコンパイル時に検出することができメモリが起因する多くのセキュリティホールを防ぐことができます。

- 実用性\
  Rustは、高いパフォーマンスと安全性を兼ね備えた実用的な言語です。\
  また、C言語とのインタフェースをサポートしているため、既存のCライブラリを利用することもできます。
  
- 移植性\
  Rustは、異なるプラットフォーム（Windows、macOS、Linuxなど）で動作するように設計されています。\
  また、WebAssemblyをサポートしているため、Webブラウザ上での実行も可能です。\
  低レベルな言語であるため、他の言語とのインタフェースも容易です。

### 用途

-　ツール開発\
  Rustは、高いパフォーマンスと安全性を提供するため、システムツールやCLIツールの開発に適しています。

- Webバックエンド\
  Rustは、高いパフォーマンスと安全性を提供するため、Webバックエンドの開発にも適しています。\
  また、RocketやActixなどのWebフレームワークが利用できます。

- サーバーレスコンピューティング\
  Rustは、高いパフォーマンスと安全性を提供するため、サーバーレスアプリケーションの開発にも適しています。\
  特に1ms単位で料金が発生するAWS Lambdaでは、より低コストに抑えることができます。

### 例

**所有権と借用**\
Rustの特徴的な機能である所有権システムは、メモリ安全性を保証します。

```rs
let s1 = String::from("hello");
let s2 = s1; // s1の所有権がs2に移動
// println!("{}", s1); // エラー: s1はもう有効ではない
println!("{}", s2); // OK

let s3 = String::from("world");
let len = calculate_length(&s3); // s3の参照を渡す
println!("The length of '{}' is {}.", s3, len); // s3はまだ使える
```

**パターンマッチと列挙型**\
Rustは強力なパターンマッチング機能を持ち、列挙型と組み合わせて使用することで、複雑なデータ構造を簡単に処理できます。

```rs
enum WebEvent {
    PageLoad,
    PageUnload,
    // enumにデータを持たせることもできる
    KeyPress(char),
    // フィールドを持たせることもできる
    Click { x: i64, y: i64 },
}

match event {
    WebEvent::PageLoad => println!("page loaded"),
    WebEvent::PageUnload => println!("page unloaded"),
    WebEvent::KeyPress(c) => println!("pressed '{}'", c),
    // if条件を使ってパターンを絞り込むこともできる
    WebEvent::Click { x, y } if x < 0 || y < 0 => println!("clicked at negative x or y"),
    WebEvent::Click { x, y } => println!("clicked at x={}, y={}", x, y),
}
```

**エラーハンドリング**\
RustはResult型を使用して、エラーを明示的に処理することを推奨しています。

```rs
use std::fs::File;
use std::io::Read;

fn read_file_contents(path: &str) -> Result<String, std::io::Error> {
    let mut file = File::open(path)?;
    let mut contents = String::new();
    file.read_to_string(&mut contents)?;
    Ok(contents)
}

let contents:Result<String,std::io::Error>=read_file_contents("./sample.txt");
// パターンマッチを使ってエラーを処理できる
match contents {
    Ok(contents) => println!("File contents: {}", contents),
    Err(err) => eprintln!("Error reading file: {}", err),
}
```

**トレイト**\
トレイトはRustの抽象化メカニズムで、他の言語のインターフェースに似ています。

```rs
trait Printable {
    fn format(&self) -> String;
}

struct Printer {
    value: String,
}
// トレイトを実装することで、formatメソッドを持つようになる
impl Printable for Printer {
    fn format(&self) -> String {
        format!("Printer: {}", self.value)
    }
}

// トレイトをジェネリック関数で使用する
fn print<T: Printable>(item: T) {
    println!("{}", item.format());
}

let printer = Printer { value: "Hello".to_string() };
// Printer構造体はPrintableトレイトを実装しているので、print関数で使用できる
print(printer);

```

## インストール

  [公式サイト](https://www.rust-lang.org/ja)からインストールしてください。

## 学習

- [Tour of Rust](https://tourofrust.com/00_ja.html)\
    Rustの基本的な機能を学ぶためのオンラインツアーです。

## ライブラリ、フレームワーク

- [Tauri](https://tauri.app/): Rust製のデスクトップアプリケーションフレームワーク
- [Rocket](https://rocket.rs/): Rust製のWebフレームワーク
- [Axum](https://github.com/tokio-rs/axum): Rust製のWebフレームワーク

## ドキュメント

- [Rust公式ドキュメント](https://doc.rust-lang.org/book/)
