# Go

- [Go](#go)
  - [Goとは](#goとは)
  - [特徴](#特徴)
    - [用途](#用途)
    - [例](#例)
  - [インストール](#インストール)
  - [学習](#学習)
  - [ライブラリ、フレームワーク](#ライブラリフレームワーク)
  - [ドキュメント](#ドキュメント)

## Goとは

Goは、Googleが開発したオープンソースのプログラミング言語です。
シンプルさ、効率性、および並行処理のサポートを重視して設計されており、
大規模なソフトウェア開発に適しています。

## 特徴

- **シンプルな文法**\
  Goは、学習しやすく読みやすい文法を持っています。
  言語仕様がコンパクトで、多くの機能を組み込み関数やライブラリとして提供しています。

- **高速なコンパイルと実行**\
  Goは、コンパイル速度が非常に速く、生成された実行ファイルも高速に動作します。
  静的型付け言語でありながら、動的型付け言語のような開発体験を提供します。

- **並行処理のサポート**\
  Goroutineとチャネルを使用した並行処理モデルにより、
  効率的かつ簡単に並行プログラミングを実現できます。

- **クロスコンパイル**\
  Goは、異なるプラットフォーム（Windows、macOS、Linux）向けに
  簡単にクロスコンパイルできる機能を持っています。

### 用途

- **Webサーバー**\
    Goは高性能なWebサーバーを構築するための言語として広く使用されています。\
    ビルド後は単一のバイナリファイルとして配布できるため、デプロイメントが容易です。\
    また、標準ライブラリにはHTTPサーバーの機能が組み込まれているため、サードパーティーライブラリの依存が少ないのも特徴です。

- **マイクロサービス**\
    Goは軽量なバイナリサイズと高速な起動時間により、コンテナ化された環境での利用が容易です。\
    また、gRPCなどのプロトコルをサポートしており、マイクロサービス間の通信に適しています。

- **ツール開発**\
    Goは、高速なコンパイル速度と実行速度を持つため、CLIツールやシステムツールの開発に適しています。

### 例

**Goroutineと並行処理**\
Goの特徴的な機能であるGoroutineを使用した並行処理の例です。

```go
package main

import (
    "fmt"
    "time"
)

func say(s string) {
 for i := 0; i < 50; i++ {
  time.Sleep(100 * time.Millisecond)
 }
 fmt.Println(s)
}
func say_async(s string, ch chan string) {
 for i := 0; i < 50; i++ {
  time.Sleep(100 * time.Millisecond)
 }
 fmt.Println(s)
 ch <- s
}

func main() {
 // 同期で実行される(終了を待つ)
 say("hello")
 // 非同期で実行される(終了を待たない)
 go say("world")

 // 非同期で実行される(終了を待つ)
 ch := make(chan string)
 defer close(ch)
 go say_async("async", ch)
 <-ch
}

```

**インターフェースとダックタイピング**\
Goのインターフェースは暗黙的に実装され、柔軟性が高いです。

```go
package main

import "fmt"

package main

import "fmt"

// Speakerインターフェース
type Speaker interface {
 Speak() string
}

type Dog struct{}

func (d Dog) Speak() string {
 return "Woof!"
}

type Cat struct{}

func (c Cat) Speak() string {
 return "Meow!"
}

func MakeSpeak(s Speaker) {
 fmt.Println(s.Speak())
}

type Human struct{}

func (h Human) Speech() {
 fmt.Println("Hello!")
}

func main() {
 // Dog,CatはどちらもSpeakerインターフェースを実装しているので、MakeSpeak関数に渡すことができる
 animals := []Speaker{Dog{}, Cat{}}
 // animals:= []Speaker{Dog{}, Cat{}, Human{}} HumanはSpeakerインターフェースを実装していないのでエラーになる

 for _, animal := range animals {
  MakeSpeak(animal)
 }
}

```

**エラーハンドリング**\
Goは明示的なエラーハンドリングを推奨しています。

```go
package main

import (
    "fmt"
    "os"
)

// エラーの可能性のある関数は2値返却型,もしくはエラーのみを返す
func readFile(filename string) (string, error) {
    data, err := os.ReadFile(filename)
    if err != nil {
        return "", err
    }
    // エラーがない場合はnilを返す
    return string(data), nil
}

func main() {
    content, err := readFile("example.txt")
    // エラーが発生した場合の処理
    if err != nil {
        fmt.Println("Error reading file:", err)
        return
    }
    fmt.Println("File contents:", content)
}
```

## インストール

[公式サイト](https://golang.org/dl/)からインストールしてください。

## 学習

- [A Tour of Go](https://tour.golang.org/welcome/1)
  Goの基本的な機能を学ぶためのオンラインツアーです。\
  Goはとてもシンプルな言語なので、短時間で学習できます。

## ライブラリ、フレームワーク

- [Gin](https://github.com/gin-gonic/gin): 高性能なWebフレームワーク
- [Echo](https://echo.labstack.com/): 高性能で拡張性のあるWebフレームワーク
- [gRPC-Go](https://github.com/grpc/grpc-go): gRPCのGo言語実装
- [Connect](https://connectrpc.com/docs/introduction): ブラウザおよび gRPC 互換の HTTP API を構築するためのライブラリ

## ドキュメント

- [Go公式ドキュメント](https://golang.org/doc/)
- [Effective Go](https://golang.org/doc/effective_go.html): Goの慣用的な書き方を学ぶためのドキュメント
