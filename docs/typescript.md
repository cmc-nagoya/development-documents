# TypeScript

- [TypeScript](#typescript)
  - [TypeScriptとは](#typescriptとは)
  - [特徴](#特徴)
    - [例](#例)
  - [インストール](#インストール)
  - [学習](#学習)
  - [ライブラリ、フレームワーク](#ライブラリフレームワーク)
  - [ドキュメント](#ドキュメント)

## TypeScriptとは

TypeScriptは、Microsoftが開発したオープンソースのプログラミング言語です。
JavaScriptに静的型付けとその他の機能を追加し、大規模なアプリケーション開発に
より適した言語として設計されています。\
TypeScriptは、トランスパイルすることでJavaScriptに変換されます。

## 特徴

- 静的型付け
  TypeScriptは、変数や関数の引数、戻り値に型を指定できます。
  これにより、多くのバグを事前に防ぎ、コードの可読性と保守性が向上します。

- JavaScriptとの互換性
  TypeScriptはJavaScriptの上位集合であり、既存のJavaScriptコードは
  有効なTypeScriptコードとして動作します。

- 豊富な開発ツール
  強力な型推論や自動補完機能により、開発効率が大幅に向上します。

- オブジェクト指向プログラミングのサポート
  クラス、インターフェース、モジュールなどの機能を提供し、
  大規模なアプリケーション開発をサポートします。

### 例

**基本的な型付け**
TypeScriptの基本的な型付けの例です。

```typescript
// 変数に型を指定
let name: string = "Alice";
let age: number = 30;
let isStudent: boolean = false;

// 関数の引数と戻り値に型を指定
function greet(person: string): string {
    return `Hello, ${person}!`;
}

console.log(greet(name));
```

**インターフェースと型エイリアス**
TypeScriptのインターフェースと型エイリアスの使用例です。

```typescript
// インターフェース
interface Person {
    name: string;
    age: number;
}

// 型エイリアス
type Point = {
    x: number;
    y: number;
};

function printPerson(person: Person) {
    console.log(`${person.name} is ${person.age} years old.`);
}

function calculateDistance(p1: Point, p2: Point): number {
    return Math.sqrt(Math.pow(p2.x - p1.x, 2) + Math.pow(p2.y - p1.y, 2));
}

let alice: Person = { name: "Alice", age: 30 };
printPerson(alice);

let point1: Point = { x: 0, y: 0 };
let point2: Point = { x: 3, y: 4 };
console.log(`Distance: ${calculateDistance(point1, point2)}`);
```

**ジェネリクス**
TypeScriptのジェネリクスを使用した例です。

```typescript
function identity<T>(arg: T): T {
    return arg;
}

let output1 = identity<string>("myString");
let output2 = identity<number>(100);

console.log(output1);
console.log(output2);

// ジェネリクスを使用した配列の型指定
function getFirstElement<T>(arr: T[]): T | undefined {
    return arr[0];
}

let numbers = [1, 2, 3, 4, 5];
let strings = ["a", "b", "c"];

console.log(getFirstElement(numbers));
console.log(getFirstElement(strings));
```

**非同期処理とPromise**
TypeScriptでの非同期処理とPromiseの使用例です。

```typescript
function delay(ms: number): Promise<void> {
    return new Promise(resolve => setTimeout(resolve, ms));
}

async function fetchData(url: string): Promise<string> {
    await delay(1000); // 1秒待機
    // 実際のアプリケーションではここでHTTPリクエストを行います
    return `Data from ${url}`;
}

async function main() {
    try {
        const data = await fetchData("https://api.example.com");
        console.log(data);
    } catch (error) {
        console.error("An error occurred:", error);
    }
}

main();
```

## インストール

Node.jsがインストールされている環境で、以下のコマンドを実行してください：

```bash
npm install -g typescript
```

また,DenoやBunではTypeScriptが標準でサポートされており、追加のインストールは不要です。

```bash
deno run -A script.ts
```

```bash
bun run script.ts
```

## 学習

- [TypeScript公式ハンドブック](https://www.typescriptlang.org/docs/)
  TypeScriptの基本から応用までを学べる公式ドキュメントです。

- [TypeScript Playground](https://www.typescriptlang.org/play)
  ブラウザ上でTypeScriptのコードを試せるオンラインツールです。

## ライブラリ、フレームワーク

- [Angular](https://angular.io/): Googleが開発したTypeScriptベースのWebアプリケーションフレームワーク
- [React](https://reactjs.org/) + [TypeScript](https://www.typescriptlang.org/docs/handbook/react.html): ReactとTypeScriptを組み合わせた開発
- [Vue.js](https://vuejs.org/) + [TypeScript](https://vuejs.org/v2/guide/typescript.html): Vue.jsとTypeScriptを組み合わせた開発
- [NestJS](https://nestjs.com/): TypeScriptで書かれたサーバーサイドアプリケーションフレームワーク

## ドキュメント

- [TypeScript公式ドキュメント](https://www.typescriptlang.org/docs/)
- [TypeScriptディープダイブ](https://basarat.gitbook.io/typescript/): TypeScriptの詳細な解説書