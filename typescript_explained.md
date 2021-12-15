# TypeScript 解説

<!-- @import "[TOC]" {cmd="toc" depthFrom=1 depthTo=6 orderedList=false} -->

<!-- code_chunk_output -->

- [TypeScript 解説](#typescript-解説)
  - [想定読者](#想定読者)
- [概念編](#概念編)
  - [TypeScript とは](#typescript-とは)
    - [言語としての Typescript](#言語としての-typescript)
    - [モジュールとしての Typescript](#モジュールとしての-typescript)
    - [メリット](#メリット)
  - [背景](#背景)
    - [歴史](#歴史)
      - [標準](#標準)
        - [ECMA TC39](#ecma-tc39)
      - [モジュール仕様](#モジュール仕様)
      - [まとめ](#まとめ)
    - [複雑化](#複雑化)
    - [コードバンドラ | トランスパイラ](#コードバンドラ-トランスパイラ)
      - [Webpack(コードバンドラ)](#webpackコードバンドラ)
      - [Babel(トランスパイラ)](#babelトランスパイラ)
      - [Rome(all-in-one tool)](#romeall-in-one-tool)
      - [SnowPack(コードバンドラ)](#snowpackコードバンドラ)
    - [AltJs](#altjs)
      - [一覧](#一覧)
      - [そもそも javascript に class もなければ const もなかった(2015 年まで)](#そもそも-javascript-に-class-もなければ-const-もなかった2015-年まで)
      - [javascript を書くのは容易ではなかった。](#javascript-を書くのは容易ではなかった)
      - [代表的なやつ一覧](#代表的なやつ一覧)
    - [Typescript](#typescript)
- [環境構築編](#環境構築編)
  - [導入](#導入)
    - [インストール](#インストール)
    - [初期化](#初期化)
    - [トランスパイル実行](#トランスパイル実行)
  - [typescript の設定](#typescript-の設定)
    - ["compilerOptions"](#compileroptions)
    - ["extends"](#extends)
    - ["include"](#include)
    - ["exclude"](#exclude)
    - ["references"](#references)
  - [tslib の利用](#tslib-の利用)
  - [prettier との連携 (特に設定はいらない)](#prettier-との連携-特に設定はいらない)
  - [eslint との連携](#eslint-との連携)
  - [typedoc との連携](#typedoc-との連携)
  - [jest との連携](#jest-との連携)
  - [React との連携](#react-との連携)
  - [webpack との連携](#webpack-との連携)
- [syntax 編](#syntax-編)
  - [基本的構文・ファイル群の紹介](#基本的構文ファイル群の紹介)
    - [ES Module 構文を用いる。](#es-module-構文を用いる)
    - [.ts ファイル](#ts-ファイル)
    - [.d.ts ファイル](#dts-ファイル)
      - [when to use?](#when-to-use)
    - [.map ファイル](#map-ファイル)
      - [.js.map](#jsmap)
      - [.d.ts.map](#dtsmap)
      - [when to use?](#when-to-use-1)
  - [typescript-in-5-minutes](#typescript-in-5-minutes)
    - [Types by Inference](#types-by-inference)
    - [Defining Types](#defining-types)
    - [Composing Types](#composing-types)
      - [Unions](#unions)
      - [Generics](#generics)
    - [Structural Type System](#structural-type-system)
    - [Next Steps](#next-steps)
  - [basic-typing](#basic-typing)
  - [primitives](#primitives)
  - [type-widening, type-narrowing, type-cast (as operator)](#type-widening-type-narrowing-type-cast-as-operator)
    - [type-widening, type-cast (as operator)](#type-widening-type-cast-as-operator)
    - [type-narrowing](#type-narrowing)
  - [interface, union-type, type-guard](#interface-union-type-type-guard)
    - [interface](#interface)
    - [union-type](#union-type)
    - [type-guard](#type-guard)
  - [typeof, keyof](#typeof-keyof)
  - [optional type](#optional-type)
  - [non-null assersion operator (Postfix `!`)](#non-null-assersion-operator-postfix)
  - [class access modifier](#class-access-modifier)
  - [module, namespace, interface 合成](#module-namespace-interface-合成)
  - [index signature, mapped type](#index-signature-mapped-type)
  - [utility types](#utility-types)
  - [conditional type, never, infer](#conditional-type-never-infer)
    - [conditional](#conditional)
    - [never](#never)
    - [infer](#infer)
  - [generics](#generics-1)
  - [template-literal-type, 動的キーの型付け](#template-literal-type-動的キーの型付け)
  - [@ts comments](#ts-comments)
    - [@ts-ignore](#ts-ignore)
    - [@ts-expect-error](#ts-expect-error)
    - [@ts-check](#ts-check)
    - [@ts-nocheck](#ts-nocheck)
  - [further reading: 型パズルなど参考になりそうな記事へのリンク](#further-reading-型パズルなど参考になりそうな記事へのリンク)
- [プラクティス集](#プラクティス集)
  - [複雑な型とテスト](#複雑な型とテスト)
  - [型は常に書く](#型は常に書く)
  - [Mutation しない](#mutation-しない)
  - [DI する。interface を書く。継承しない。](#di-するinterface-を書く継承しない)
  - [optional/nullish な number, string について truthy/falsy 評価しない](#optionalnullish-な-number-string-について-truthyfalsy-評価しない)
  - [optional な object のメンバに対して "key" in obj オペレータで判定しない](#optional-な-object-のメンバに対して-key-in-obj-オペレータで判定しない)
  - [optional な object のメンバに対して Object.hasOwnProperty で判定しない](#optional-な-object-のメンバに対して-objecthasownproperty-で判定しない)
  - [Enum は使わない](#enum-は使わない)
  - [private メンバには prefix underscore](#private-メンバには-prefix-underscore)
- [おまけ](#おまけ)
  - [create-react-app like build script](#create-react-app-like-build-script)
  - [React/Typescript component sample](#reacttypescript-component-sample)

<!-- /code_chunk_output -->

## 想定読者

- javascript 完全理解済
- 静的型付け言語経験済み
  - Java/C++など
- モダン目な静的型付け言語をかじってるといいかも
  - Rust, Go, Kotlin/Swift など

# 概念編

## TypeScript とは

### 言語としての Typescript

静的・推論的・構造的型付けの言語

- MicroSoft が開発するオープンソース言語
- javascript に型付け機能を追加し、静的型付け言語にしたもの
- javascript の完全なスーパーセット
  - スーパーセット = 何かを付け足すが、それ以外は変えていないもの
- AltJs の一種
  - javascript に変換して実行される言語のこと
- 種々の AltJs の失敗を踏まえ、標準(ECMA)に追従する意向を示す
  - メンテされない心配が比較的薄い方針
  - 標準化前の ECMA 仕様を先取りすることがあるが独自機能は存在しない

### モジュールとしての Typescript

- トランスパイラ
  - 構文エラー・型の違反をエラーとして報告する
  - 各種バージョンの javascript に変換できる
    - ES3、ES5, ES6, ES2016 ... ES2020, ESNEXT
- 言語サーバー
  - [MS がなんか言ってるやつ](https://docs.microsoft.com/ja-jp/visualstudio/extensibility/language-server-protocol?view=vs-2019)
  - エディターにもろもろの開発支援情報を与えるための機能
    - Syntax Highlighting
    - 構文エラーの表示
    - 補完情報の表示
    - ライブラリの定義情報・参照への移動
      など
  - エディターで開くとクラス名が緑になったり変数が青になったりするのはこの言語サーバーのおかげなんです
  - vscode でやる分には自動的に起動するので気にする必要はない

当然構文解析器などの機能もあるが省略。よくわかってないし

### メリット

- 入力値の型バリデーションの必要性が消滅
  - ライブラリとして公開する場合は境界面でバリデーションをつけたほうがいいのは変わりないが
- 補完が利く
- 型定義がプログラムに含まれることで、doc コメントがメンテされない可能性が減る
- コンパイラがエラー吐かないなら大体動作するの状態に
  - 型システム内である型が
    - "HOGE"か"FUGA"のみ
    - 1、2、3 のみ
      のようなことが可能(※リテラルの値で型を指定できる言語はそう多くない印象)
- 複数種の型がありえる数値の、ある型を適切にハンドルしていないようなことが判別可能に

## 背景

背景は飛ばしてもいい。
割とエビデンスレスでいい加減なことを書いてる。
ただし、javascript を取り巻くツールチェーンは複雑な割に俯瞰的に全般的に解説するような文章はあんまり見つからないので書いておく。

### 歴史

詳しいことはよく知らんけどだいたい以下の感じ

この辺とか　https://ja.wikipedia.org/wiki/JavaScript#%E6%AD%B4%E5%8F%B2

#### 標準

- javascript は 1995 年、NetScape 社の自社ブラウザのスクリプト言語になるべく開発された。
  - Java が流行ってたから javascript と命名されたが特に共通点はない
- IE などこれをパクりたかったがライセンスの問題があり完全に同じ仕様を持たせられなかったらしい。
  - -> 独自実装の乱立
- 1997 年から Ecma International による標準化が入り(ECMAScript - ES)、2000 年ころにはブラウザ間の実装差がそれなりになくなるように
- javascript は非常に小さなものから始まり、それを拡大するときに色々揉める。結果 ECMA4 がポシャる
  - ECMA3 - 1999
  - ECMA4(ポシャる)
    この間 10 年
  - ECMA5 - 2009
  - ECMA5.1 - 2011
  - ECMA 6 (ES2015)
- コミュニティー全体の同意をとろうとするとリリーススパンが伸びすぎてしまうため、随時リビングスタンダード更新し、毎年ある時期にその年数のバージョンとして標準化することになった。
  - だから 2015 年以降は ES{年}で呼ばれる

最近では[web-platform-tests](https://github.com/web-platform-tests/wpt)というブラウザ標準の WebAPI の挙動差がないかなどの共通 TestSuite を作る動きがあって、
ブラウザ間の動作の違いは存在しないか、テスト上はっきりと出るかのどちらかになって開発しやすくなってるかも。

[主要ブラウザのテスト結果のダッシュボード](https://wpt.fyi/results/?label=experimental&label=master&aligned)

##### ECMA TC39

- ECMA の TC(Technical Committees)39 が javascript の標準化を行っている
  - https://www.ecma-international.org/technical-committees/tc39/?tab=general
- ECMA によって標準化されている javascript のことを EcmaScript(ES)と呼ぶ。
- ここに毎年更新のスタンダードが乗る
  - https://www.ecma-international.org/publications-and-standards/standards/ecma-262/
- プロポーザルはここ
  - https://github.com/tc39/proposals
  - Stage 0 から 4 までの段階を踏んで実装する。
    - 詳しいプロセスは省略する。
    - "champion"の存在が必須
    - stage 3 で仕様として完成。
    - 2 つ以上の実装で stage 4
      - Chakra.aspx) @ MicroSoft
      - SpiderMonkey @ Mozilla
      - V8 @ Google
      - JavaScriptCore @ Apple
        最近は Edge が Chromium になったので Chakra はもうサポートされないかも。
    - stage 4 が実装済み状態で、4 に進んでいるものが次の ECMAScript 標準に取り込まれる。

#### モジュール仕様

- javascript には(2015 年まで)標準のモジュール仕様がなかったため乱立する
  - AMD, UMD, commonjs2, System, require.js などよく知らないがたくさん
- Node.js の登場(2009 年)
  - V8 javascript engine が速かったことや WHATWG が W3C とバトって javascript で何でもできるようにしたおかげで javascript が再び流行りだした。
  - イベント駆動ができそうで流行ってる言語を使ってイベント駆動のサーバーサイドコード実行環境を作ろうというのが Node.js。
  - 最初から module 構文があった。
    - [Node.js v0.10.48 の module_object](https://nodejs.org/docs/latest-v0.10.x/api/modules.html#modules_the_module_object)
- Node.js のために `npm` という優れたモジュール管理エコシステムが作られる。node.js 用以外のライブラリもここでホストするようになる
- ECMAScript 2015 でモジュール仕様策定
  - ESmodule が仕様策定されたのは ES2015 だがその時点では**実装しているブラウザはなかった**。
    - 最速で 2017/03/27 から
      - [MDN の実装状況。safari が最速で 2017/03/27](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules)
- ECMAScript 2020 でモジュール仕様にメタ情報などの仕様が追加

#### まとめ

- ES4 は存在しない
- ES3, ES5, ES6(ES2015) とそれ以降で大体わかれる
- javascript の前方変換はよく ES3,ES5 が候補としてあがる。
  - ES6 以降が動く環境では最新まで動くことが多いからだと思われる
- commonjs module は標準ではない
- ECMA 標準のモジュール仕様は 2015 年に標準化。
  - ブラウザに実装されだしたのが 2017 - 2018 年の間。
- `npm`でモジュールが管理されることが多い

### 複雑化

- WHATWG が HTML Living Standard を打ち立て、javascript からなんでもできるようになったことで Flash などが廃る。
- 逆に、インタラクティブな WebUI を javascript から提供するための React.js のようなライブラリがで始める。
  - youtube は polymer.js(WebComponent wrapper)
    - polymer.js はメンテナンスモードに突入したため新規プロジェクトには Lit.js を推奨。
  - Netflix などは Next.js(React.js)
  - 日本では Nuxt.js(Vue.js)の人気のほうが高いらしい
  - [JetBrains の調査によると React が一番人気らしい](https://www.jetbrains.com/ja-jp/lp/devecosystem-2020/javascript/)
  - Svelte3 がアツいのでそのうちとってかわられるかも
- 結果、javascript のプロジェクトサイズが大きくなり始める。
- 動的型付けであること・後方互換性のために残された仕様がいまいちなことに起因して、一定サイズ以上のプロジェクトになると開発難易度・メンテ難易度が跳ねあがる。
- そのため、javascript だけではこれ以上立ち行かれなくなる。

### コードバンドラ | トランスパイラ

- web アプリのページ表示を最小にしたい要求が存在する
  - 通販サイトなどの場合、ページ表示までの時間が何秒遅くなると何人が離脱するというようなデータがとられている。

客の手元で実行されるスクリプト限界まで軽く、短くしたものにし、一方で開発環境では開発しやすいものにしたいという要求がある。

そこでコードバンドラ(リソースをまとめたり分けたりする)とトランスパイラ(新しい構文をふるい構文に変換するやつ)が発達した。

#### Webpack(コードバンドラ)

html, js, css, img などをまとめたり、分けたり、不要コードを削除したりするためのコードバンドラ。
2014 年リリース。Node.js 上で動作する。
様々なプラグインを実行可能。
今日の javascript ツールチェーンで無くてはならない。

#### Babel(トランスパイラ)

javascript トランスパイラ。
ES2015 以降の構文をそれ以前の構文に変換する機能など。
後述の Flow や jsx という拡張構文は babel を使用して js に変換していた。

#### Rome(all-in-one tool)

https://rome.tools/

> Rome is a linter, compiler, bundler, and more for JavaScript, TypeScript, JSON, HTML, Markdown, and CSS.
>
> Rome is designed to replace Babel, ESLint, webpack, Prettier, Jest, and others.

散らばった js のツールチェインを一つで賄おうという非常に野心的なプロジェクト。
go が公式でフォーマッターからテストライブラリまでサポートしているのでそこから着想しているのだろうか？

#### SnowPack(コードバンドラ)

https://www.snowpack.dev/

新しい webpack の代替ツール。

ESM を利用することでバンドルしない開発環境を提供する。
npm から提供されるモジュールのうち、cjs(commonj module = node.js のモジュール)鹿サポートしないものは ESM にアップキャストし、変更のあったファイルだけビルドしなおすことで高速なリロード機能を提供するのだとか。

Svelte のフレームワークである Sapper はこちらを取り込みそうな兆しがあるのでこっちが流行るかも・・・

https://svelte.dev/blog/whats-the-deal-with-sveltekit#How_is_this_new_thing_different

> is at the vanguard of this movement, and it's what powers SvelteKit. It's astonishingly fast, and has a beautiful development experience (hot module reloading, error overlays and so on), and we've been working closely with the Snowpack team on features like SSR.

結局プロダクションビルドは webpack か rollup を利用するようなので javascript ツールチェインの混沌さをさらに上げるためのツールでもある。

### AltJs

javascript に変換して実行できる別の言語。Scala や Kotlin のことを AltJava っていうようなもの。

#### 一覧

coffeescript のページでメンテされてる:

https://github.com/jashkenas/coffeescript/wiki/List-of-languages-that-compile-to-JS

#### そもそも javascript に class もなければ const もなかった(2015 年まで)

ECMA2015 での追加項目は以下で見れる:

http://es6-features.org/

`class syntax`がなかったせいで見通しが悪い`prototype`構文を使っていたことや、
継承の構文がなかったため、それぞれが独自に定義した [ObjectDefineProperty のような関数](https://github.com/nodejs/node/blob/master/deps/v8/src/builtins/builtins-object.cc#L39)を使って継承を行っていたことで、継承関係の把握も容易ではなかった。

そもそも module の概念がなかったため、namespace に物事を分割するのは即時関数に収めるなどのテクニックが必要であった。リファレンスは途切れるわ、見通しは悪くなるわで最悪だった。

#### javascript を書くのは容易ではなかった。

だから文法を拡張したり、別の言語から javascript へ変換して実行したいという要求があった。
**ただし今は typescript 以外はほとんど見ない。**
(※そもそも AltJS のプロジェクトは各言語 1 個づつぐらいしか見かけたことない。typescript 以外は本当に見ない。)

#### 代表的なやつ一覧

- Dart
  - 今は実質的に Flutter を動かすための言語
  - 元は javascript に変換して実行する AltJs だった。Chrome に Dart の実行エンジンを載せようとしたりいろいろしていたのはとん挫したが、js への変換機能は今もサポートが続いている。
  - [現行の sass の変換ツールは Dart から変換されたもの](https://sass-lang.com/dart-sass)
- CoffeeScript
  - ES2015 対応が遅かったらしい
- LiveScript
- PureScript
  - 純関数型言語。純関数型言語は結構珍しいらしい
- Flow
  - ALtJs ではないのだが、javascript に静的に型をつける方法として普及はしてたらしい
- Typescript
  - MS がメインメンテナであることや google が社内標準語にしていることから有望

typescript だけ覚えればいいです。

### Typescript

- 独自機能や独自構文がないことで、サポートが切れて使えなくなる心配が存在しない
- 型を追加することで意図されない挙動を検出できること。

などの理由から非常にはやっている。

ECMA の新規構文で javascript も割とかける言語になったことと、typescript 機能の interface などを用いてオブジェクト指向っぽいことも安全にできるようになったことで、わざわざメンテされなくなるかもしれない別言語を使う必要がなくなった。

# 環境構築編

## 導入

### インストール

npm モジュールとして提供されている。
任意の npm プロジェクトにおいて、

```
npm install -D typescript
# もしくは
yarn add -D typescript
```

で導入可能。
**typescript モジュールは semantics versioning に従いわない。**
**マイナーバージョンの変更が破壊的変更であることもある。**
ということでバージョン固定しておきましょう。

package.json:

```json
{
  //...snip...
  "devDependencies": {
    //...snip...
    "typescript": "4.3.2" // 先頭のハット(^)を取り除く
  }
  //...snip...
}
```

### 初期化

実行ファイルは

- トランスパイラ: `node_modules/.bin/tsc`
- 言語サーバー: `node_modules/.bin/tsserver`

の二つである.
(shebang をつけただけの js ファイルだが)
実態は`node_modules/typescript/lib`以下にある同名ファイル。

以下のコマンドで初期化

```shell
node_modules/.bin/tsc --init
# 最近のnpmやyarnは.binの中身を解析しているようなので、以下でもいい
yarn tsc --init
# OR
npx tsc --init
```

### トランスパイル実行

以下のコマンドで、吐き出された tsconfig.json に基づいてトランスパイル実行される。

```shell
yarn tsc
```

コンパイルエラーはコマンドのエラーとして報告される。

デフォルトでは cwd と同階層から上の階層に向けて tsconfig.json を探してきて、最初に見つかったものを設定値とするようだ。
`-p` `--project`オプションで任意の json ファイルを設定ファイルとして指定可能

```shell
yarn tsc -p /path/to/tsconfig.json
```

## typescript の設定

tsconfig.json をいじくることで設定を行う。

ドキュメントはこちら:

https://www.typescriptlang.org/ja/tsconfig

**vscode は/tsconfig(?:\\..\*)?\.json/という規則のファイル名の場合、json with comment として認識するので、ビルド設定を色々変えて複数設定作る場合、tsconfig.prod.json, tsconfig.dev.json のような感じで命名したほうが良いだろう**

### "compilerOptions"

コンパイルのルール。

基本は`tsc --init`したファイルにコメントアウトされた設定値と解説がかいてあるのでわかりにくいものだけ解説。
init されたファイルにすべての設定値がかいてあるわけではないのでリファレンスも確認すること。

- "outFile": "./", /\* Concatenate and emit output to single file. \*/
  - 特定のモジュール形式のみで必要な設定で、`commonjs` や `es2015` を設定しているときに使えるものではない
  - namespace などを設定していると 1 つのファイルに結合してくれるらしいが詳細はよくわかってない
  - コードバンドルをするときは`webpack`や`rollup`を使用すること
- "outDir": "./", /\* Redirect output structure to the directory. \*/
  - 書き出し先ディレクトリを指定する設定。
  - 設定しない場合 ts と同じディレクトリに js ファイルが出力される。
  - ファイルの相対パス関係を変えずに出力するため、`"rootDir"`を合わせて設定しないとコンパイル対象が増えるたびに構造が変わる可能性がある。
    - "rootDir"を設定していても、rootDir より外のファイルを`import`すると構造が変わる
- "importHelpers": true, /\* Import emit helpers from 'tslib'. \*/
  - true の場合、ポリフィル使用部分が tslib をインポートしてそれを使用するようになる。
  - `yarn add tslib`してプロジェクトに tslib を入れる必要がある。
  - `false`,もしくは設定しない場合、古い javascript 仕様に変換した場合のポリフィル(等価コード)はファイルごとにそれぞれ出力される。重複を省いてコードを軽くしたい場合必要な設定。
- "baseUrl": "./", /\* Base directory to resolve non-absolute module names. \*/
  - 相対パスインポート時の解決パスのベースの差し替えだが、型の解決に使うだけで実行時にパスを差し替えるようなスクリプトが入れられるわけではない。
  - webpack などでモジュール解決パスを差し替えた時用の設定っぽいです。
- "paths": "{}", /\* A series of entries which re-map imports to lookup locations relative to the 'baseUrl'. \*/,
  - 上記と同じく、実際に差し替えを行うわけではない。
  - d.ts の入っていないモジュールを扱うとき、自前で用意した d.ts が読み込まれるようにするためにこの設定を使うことができる

```json
{
  "compilerOptions": {
    "paths": {
      "bfj": ["./import_wrapper/bfj.d.ts"]
    }
  }
}
```

```ts
import bfj from "bfj";
//実行時にはnode_modulesからbfjが読み込まれる。
//型情報は./import_wrapper/bfj.d.tsが参照される。
```

### "extends"

別のプロジェクト(別の`tsconfig.json`)を継承する場合のパス指定。

### "include"

`tsc`する対象の glob マッチ条件。

基本は以下のような感じでいいです。

```json
{
  "include": ["**/*.ts", "**/*.tsx"]
}
```

compilerOptions.allowJs などを設定する場合は、rootDir,outDir も合わせて設定して、`"exclude"`には outDir のディレクトリも入れましょう。、

### "exclude"

`"include"`と逆に`tsc`する対象から外すための glob マッチ条件。

**ただし include されているファイルから import 構文で参照されている場合、exlude の条件に一致していても tsc されますのでご注意を**

基本は以下のような感じでいいです。

```json
{
  //node_modulesとテスト向けファイルを外すglob条件
  "exclude": ["node_modules", "**/__tests__/**/*.ts", "**/*.test.ts"]
}
```

### "references"

プロジェクト分割の方法を与えるもの。
使用したことないので以下リファレンスを参照のこと。
https://www.typescriptlang.org/docs/handbook/project-references.html

## tslib の利用

デフォルトの設定だと typescript が古いバージョンの javascript に変換を行った際、ポリフィルは各ファイルごとに吐かれるため、非効率である。
tslib は外部モジュールとしてこのポリフィル部分を取り出したモジュール。

```
yarn add tslib
```

tsconfig.json:

```json
{
  "compilerOptions": {
    //...snip...
    "importHelpers": true /* Import emit helpers from 'tslib'. */
  }
  //...snip...
}
```

ただし typescript のポリフィル含む構文の後方変換は不完全であるとよく言われているので結局 ES2016 あたりをターゲットに変換してから Babel を使うことになるかも

## prettier との連携 (特に設定はいらない)

prettier はコードフォーマッター。フォーマッティングのくだらないいい争いは自動フォーマッターを書けることで存在そのものを消し去ろう。

特に設定はいらない
しいて言えば`.vscode/settings.json`を以下のようにしておくといいかもしれない

```json
{
  "[typescript]": {
    "editor.formatOnSave": true,
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[typescriptreact]": {
    "editor.formatOnSave": true,
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  }
}
```

## eslint との連携

eslint はいわゆる [linter](https://ja.wikipedia.org/wiki/Lint)・・・静的コード解析ツール。
typescript が linter としての役割と果たすので併用する場合はコード規約の自動チェッカのようなことが使用目的になる。

基本的には以下の二つを入れればよいだろう。

```
yarn add -D @typescript-eslint/eslint-plugin @typescript-eslint/parser
```

この順番で extends したらいいんじゃないでしょうか。

.eslintrc:

```json
{
  "extends": [
    //...snip...
    "plugin:@typescript-eslint/eslint-recommended",
    "plugin:@typescript-eslint/recommended"
    //...snip...
  ],
  "plugins": [
    //...snip...
    "@typescript-eslint"
    //...snip...
  ],
  "parserOptions": {
    "sourceType": "module",
    "project": ["./tsconfig.json"]
  }
  //...snip...
}
```

筆者の場合)モジュール境界(≒export してる関数の返り値)での型の明示記述の強制はちょっとうざかったので切ってる。

.eslintrc:

```json
{
  "rules": {
    //...
    "@typescript-eslint/explicit-module-boundary-types": "off"
    //...
  }
}
```

## typedoc との連携

jsdoc の typescript 版。
doc comment とコードの型情報からドキュメントを生成するオートドキュメンタ。

想定読者は jsdoc はすでに知っているので説明は飛ばす。
[typedoc の doc comment の構文](https://typedoc.org/guides/doccomments/)では markdown が許可されていることなどが大きく違うほか、大幅にサポートされているタグの数が減っている。

```shell
yarn add -D typedoc
```

[プロジェクトルートに typedoc.json というファイルを作るか tsconfig.json に追記をするかで設定する](https://typedoc.org/guides/installation/#json-configuration)

out くらいは設定しといた方がいいと思う

```json
{
  "out": "/path/to/output/dir"
}
```

実行は cli から

```shell
yarn typedoc
```

## jest との連携

jest は自動テストのフレームワーク。jest は単体でテストのユースケースを満たすツールなので使いやすい。

`ts-jest` を導入して

```
yarn add -D ts-jest
```

jest の設定を以下のように変更。
//"testEnvironment"とかは環境に合わせて変える

jest.config.js:

```js
module.exports = {
  verbose: true,
  testEnvironment: "node",
  moduleFileExtensions: ["ts", "js"],
  transform: {
    "^.+\\.ts$": "ts-jest",
  },
  globals: {
    "ts-jest": {
      tsConfig: "tsconfig.json",
    },
  },
  testMatch: ["**/__tests__/**/*.test.ts"],
};
```

eslint による jest global の警告を受けないために以下を変更する

.eslintrc:

```json
{
  //...snip...
  "env": { "node": true, "es6": true, "jest": true }
}
```

## React との連携

**そもそも React プロジェクトのほとんどが Next.js を利用しており、その場合殆ど設定らしい設定を加えないのでここに書いてあることは読まなくていい。**

`jsx`でなくて`tsx`で記述する。
typescript は`tsx`のトランスパイル機能を有しており、素のままのコードを利用する場合は特に追加のツールは必要ない

tsconfig.json の[jsx の項目](https://www.typescriptlang.org/tsconfig#jsx)でどのようにトランスパイルされるかを設定できる.

ツールチェインの先のほうで `babel` などで `jsx` の変換をこなう場合は`"preserve"`にしておく

tsconfig.json:

```json
{
  "compilerOptions": {
    "jsx": "preserve"
  }
}
```

[React + Typescript component sample](#reacttypescript-component-sample)

## webpack との連携

webpack は前述したコードバンドラ。
html や css などを使用しない場合にも minify(変数名などを 1 文字に変えたり改行をなくして軽量化する)、tree-shake(未使用コードを削除して軽量化する)、split chunk(遅延ロード可能部分を別スクリプトに分割)などを行う。

単純に設定するだけなら以下のような設定ファイルになる。
`ts-loader`を使って ts -> js のトランスパイルを行い、`fork-ts-checker-webpack-plugin`を使って型チェックを別スレッドで行う。

```
# 特に明示的なimportをしないがts-loaderは外部モジュール
yarn add ts-loader fork-ts-checker-webpack-plugin
```

babel によるこまごました変換を行わない場合、typescript のトランスパイル機能と `webpack` の tree-shake、minify で十分に事足りる・・・かもしれない。

以下のコンフィグを用意してから

```shell
# cli toolをまず入れる
yarn add -D webpack-cli
# 設定ファイルを.tsで書いたのでまずトランスパイル
yarn tsc

# dev build
yarn webpack /path/to/webpack.config.dev
# prod build
yarn webpack /path/to/webpack.config.prod
```

[create-react-app からパクったビルドスクリプトをつかってもいい](#create-react-app-like-build-script)

webpack.config.base.ts:

```ts
import path from "path";

import type webpack from "webpack";
import ForkTsCheckerWebpackPlugin from "fork-ts-checker-webpack-plugin";

export const baseConfig: webpack.Configuration = {
  // target: ["node", "web"],
  entry: "/path/to/entry_point.ts",
  output: {
    // eslint-disable-next-line @typescript-eslint/explicit-module-boundary-types
    filename: (pathData_): string => {
      return pathData_?.chunk?.name === "main"
        ? "index.js"
        : "chunks/[name].js";
    },
    path: "/path/to/output/dir",
    // commonjs2 is extended commonjs of node.js
    libraryTarget: "commonjs2",
    globalObject: "this",
  },
  node: {
    __dirname: true,
    __filename: true,
  },
  resolve: {
    extensions: [".ts", ".json"],
    modules: [path.resolve(__dirname, "../", "node_modules")],
  },
  plugins: [
    new ForkTsCheckerWebpackPlugin({
      typescript: {
        configFile: "/path/to/prod/tsconfig.json",
      },
    }),
  ],
  optimization: {
    splitChunks: {
      chunks: "all",
      name: false,
    },
  },
};
```

webpack.config.dev.ts:

```ts
import type webpack from "webpack";
import { merge } from "webpack-merge";

import { baseConfig } from "./webpack.config.base";

const devConfig: webpack.Configuration = merge(baseConfig, {
  mode: "development",
  devtool: "inline-source-map",
  output: {
    chunkFilename: "chunks/[name].chunk.js",
  },
  module: {
    rules: [
      {
        test: /\.(js|ts)$/,
        use: {
          loader: "ts-loader",
          options: {
            transpileOnly: true,
            configFile: "/path/to/dev/tsconfig.json",
          },
        },
        exclude: /node_modules/,
      },
    ],
  },
});

export default devConfig;
```

webpack.config.prod.ts:

```ts
/* eslint-disable @typescript-eslint/naming-convention */
// import path from "path";

import type webpack from "webpack";
import { merge } from "webpack-merge";
// import TerserPlugin from "terser-webpack-plugin";

import { baseConfig } from "./webpack.config.base";

const prodConfig: webpack.Configuration = merge(baseConfig, {
  mode: "production",
  output: {
    chunkFilename: "chunks/[name].chunk.js",
  },
  module: {
    rules: [
      {
        test: /\.(js|ts)$/,
        use: {
          loader: "ts-loader",
          options: {
            transpileOnly: true,
            configFile: "/path/to/prod/tsconfig.json",
          },
        },
        exclude: /node_modules/,
      },
    ],
  },
});

export default prodConfig;
```

# syntax 編

上から下に向けて徐々に基本から応用へっていうイメージで書いてる。
わかりそうなところは飛ばしてね

基本はドキュメントを参照:
https://www.typescriptlang.org/docs/

外部モジュール無しなコードを試すだけなら typescript playground で:
https://www.typescriptlang.org/play

そのほかは npm init して適当なプロジェクトを作っておく

## 基本的構文・ファイル群の紹介

### ES Module 構文を用いる。

```ts
//このcommonjsモジュール仕様ではなく
const { someProp } = require("./some_mod");
//以下のES Module構文を使う
import { someProp } from "./some_mod";
```

前述のとおり javascript のモジュール方式には色々あり、その中で最も使われているのが、`commonjs module`と`ES Module`形式である。

commonjs 形式は commonjs を Node.js が独自に拡張したモジュール形式であり、ES Module の標準化が遅かった(ES2015=2015 年の夏ごろ策定)ためにデファクトスタンダードとなっている。

ES Module 形式のほうが Typescript には都合がよい(※)ため、ES Module 形式を基本的に使うことが推奨されている。

※・・・import 構文は動的インポートと静的インポートが構文上分かれており、静的インポートの場合モジュール情報を先読みしているため、型情報の読み出しも同時に行うことができる。commonjs の`require()`は実行時までモジュールが存在しているか保証されない。

### .ts ファイル

typescript の実装と型定義がどちらも入ったファイル

### .d.ts ファイル

型定義のみが入ったファイル。実行時には消えるので、型情報だけを定義して実装がない場合には必ず`import type`で行う

手動で作成することができる他、tsconfig.json の compilerOptions.declaration を設定した場合、対応した js と同階層に出力される。

some_declaration.d.ts:

```ts
export interface IDuckInterface {
    quack(duration_ms: number) => void
}
```

```ts
//こうすると実行時にエラーすることあり。
import { IDuckInterface } from "./some_declaration";
//なのでこうする
import type { IDuckInterface } from "./some_declaration";
```

`import type` は typescript が拡張している構文であるので、出力された javascript からは削除される。

#### when to use?

- ファイルに含まれるのが型定義のみで実行時には消えていいとき、もしくは型定義のみを含むことを表現したいとき。
- モジュールとして npm プロジェクトを配布するとき(後述)

### .map ファイル

#### .js.map

source map ファイル。

vscode のようなエディターやデバッガが実行時に元の ts ファイルへと遡るための情報を与える。

これを定義しておくとデバッガで ts ファイルにブレイクポイントを打てる。

inlineSourceMap の設定を有効にしている場合、個別の source map は出力されない。

#### .d.ts.map

宣言ファイルのマップファイル。
vscode のようなエディターが宣言ファイル(.d.ts)から元の ts ファイルへ遡るための情報を与える。

#### when to use?

- デバッグするとき

## typescript-in-5-minutes

ざっくり和訳:
https://www.typescriptlang.org/docs/handbook/typescript-in-5-minutes.html

typescript は javascript と珍しい関係に立つ。
typescript は javascript のすべての機能と、その一番上に追加された層を提供する: typescript の型機能をだ。

つまり、すべての javascript コードは有効な typescript でもある。
typscript のもっとも大きな恩恵は、期待されないコードのふるまいをハイライトすることであり、これによってバグの確率が減少する。

### Types by Inference

`typescript` コンパイラは `javascript` の挙動を把握しているので、ありえる型を推論することができる

```ts
let helloWorld = "Hello World";
//let helloWorld: string
```

### Defining Types

javascript において様々なデザインパターンを使うことができるだろうが、中には型を自動的に推論することが難しいものがある(例えば、動的プログラミングを使うもの)。
これらのケースを考慮するにあたり、typescript は追加構文を用いる。

```ts
const user = {
  name: "Hayes",
  id: 0,
};

//↑はintefaceキーワードを用いて以下のように型として定義できる
//You can explicitly describe this object’s shape using an interface declaration:
interface User {
  name: string;
  id: number;
}

//variable: TypeNameのような構文で型を明示して変数や関数の定義が可能
const user: User = {
  name: "Hayes",
  id: 0,
};
```

型にマッチしないものを書いたらエラー。

```ts
interface User {
  name: string;
  id: number;
}

const user: User = {
  username: "Hayes",
  // Type '{ username: string; id: number; }' is not assignable to type 'User'.
  // Object literal may only specify known properties, and 'username' does not exist in type 'User'.
  id: 0,
};
```

※object literal の時だけ余計なキーがあることをエラーとするルールが存在している。

javascript はクラスを持つオブジェクト指向言語なので、typescript も同様である。 interface 宣言をクラスの型として使える

```ts
interface User {
  name: string;
  id: number;
}

class UserAccount {
  name: string;
  id: number;

  constructor(name: string, id: number) {
    this.name = name;
    this.id = id;
  }
}

const user: User = new UserAccount("Murphy", 1);
```

interface は関数のパラメータにも使える

```ts
function getAdminUser(): User {
  //...
}

function deleteUser(user: User) {
  // ...
}
```

javascript の primitive 値に対応する型定義も存在しており、それらを interface 宣言の中で使用することができる。

- boolean
- bigint (※ES2020 以降, [Node.js ver10.8.0 以降](https://node.green/#ES2020-features-BigInt)),
- null
- number
- string
- symbol
- undefined

※全て小文字であることに注意。Capital ならば Interface 定義になっている(string は primitive, String は interface)

Typescript はここに拡張を加えており、

- any
  - なんでもあり
- [unknown](https://www.typescriptlang.org/play#example/unknown-and-never)
  - 使用者が型チェックをすること保証する
- [never](https://www.typescriptlang.org/play#example/unknown-and-never)
  - ありえない型
- void
  - 関数が`undefined`を返すか、return value を持たない

型を構築するには二つの構文がある:[Interface と Type](https://www.typescriptlang.org/play/?e=83#example/types-vs-interfaces).
基本的に`interface`の方が好ましく、特定の機能を使いたい時`type`を使う。

### Composing Types

単純な型を組み合わせて複雑な型を作っていく。

#### Unions

もっともよくあるユースケースは許容されるリテラル値の集合を表現するときで:

```ts
type WindowStates = "open" | "closed" | "minimized";
type LockStates = "locked" | "unlocked";
type PositiveOddNumbersUnderTen = 1 | 3 | 5 | 7 | 9;
```

複数の値をとるような関数を表現することができる

```ts
function getLength(obj: string | string[]) {
  return obj.length;
}
```

`typeof`で primitive 値を判別できる:

| Type      | Predicate                        |
| --------- | -------------------------------- |
| string    | typeof s === "string"            |
| number    | typeof n === "number"            |
| boolean   | typeof b === "boolean"           |
| undefined | typeof undefined === "undefined" |
| function  | typeof f === "function"          |
| array     | Array.isArray(a)                 |

※T[]を判別するときは`Array.isArray(a) && a.every(aPart => isT(aPart))`のようにさらに`every`で`T`を判別する

例として、引数の型が`string`か`array`科に応じて異なる値を返すことができる

```ts
function wrapInArray(obj: string | string[]) {
  if (typeof obj === "string") {
    return [obj];

(parameter) obj: string
  } else {
    return obj;
  }
}
```

#### Generics

Generics は型に対して変数を与える。
典型的な例は Array で、

```ts
type StringArray = Array<string>;
type NumberArray = Array<number>;
type ObjectWithNameArray = Array<{ name: string }>;
```

```ts
interface Backpack<Type> {
  add: (obj: Type) => void;
  get: () => Type;
}

// This line is a shortcut to tell TypeScript there is a
// constant called `backpack`, and to not worry about where it came from.
declare const backpack: Backpack<string>;

// object is a string, because we declared it above as the variable part of Backpack.
const object = backpack.get();

// Since the backpack variable is a string, you can't pass a number to the add function.
backpack.add(23);
// Argument of type 'number' is not assignable to parameter of type 'string'.
```

※ 他の静的型付け言語と違い、型情報はコンパイラへの最適化のヒントではない。

### Structural Type System

typescript は“duck typing”や“structural typing”と呼ばれる方式を採用しており、オブジェクトの形状があっていればその型であるとみなされる.

この構造的型付けシステムの中では、ある二つのオブジェクトが同じ形をしていれば、実際別物だとしても同じ型だと判別される。

```ts
interface Point {
  x: number;
  y: number;
}

function logPoint(p: Point) {
  console.log(`${p.x}, ${p.y}`);
}

// logs "12, 26"
const point = { x: 12, y: 26 };
logPoint(point);
```

`point` は `Point` として宣言されていないが、typescript はオブジェクトの形状をチェックし、あっているためコードは(コンパイルを)パスする

この shape-matching はオブジェクトのサブセットが一致していればよい

```ts
const point3 = { x: 12, y: 26, z: 89 };
logPoint(point3); // logs "12, 26"

const rect = { x: 33, y: 3, width: 30, height: 80 };
logPoint(rect); // logs "33, 3"

const color = { hex: "#187ABF" };
logPoint(color);
// Argument of type '{ hex: string; }' is not assignable to parameter of type 'Point'.
//   Type '{ hex: string; }' is missing the following properties from type 'Point': x, y
```

形が一致していれば、クラスであろうがオブジェクトであろうが関係ない。

```ts
class VirtualPoint {
  x: number;
  y: number;

  constructor(x: number, y: number) {
    this.x = x;
    this.y = y;
  }
}

const newVPoint = new VirtualPoint(13, 56);
logPoint(newVPoint); // logs "13, 56"
```

必要なプロパティの型が一致していれば、typescript はマッチしていると判別する。そこに実装の詳細は関係ない。

### Next Steps

- Read the full Handbook [from start to finish](https://www.typescriptlang.org/docs/handbook/intro.html) (30m)
- Explore the [Playground examples](https://www.typescriptlang.org/play#show-examples)

## basic-typing

[play it on typescript playground](https://www.typescriptlang.org/play?#code/DYUwLgBAzg9gtiAagQwE4EtkCNQC5pgYB2A5hALwQBEAZjDFmlQNwCwAULAihtqBRACMzCAHpREEKlQxUHDgGMYRKJC4gAYgFciCgQAoAlBQB8EAN4cIEVOC2oi1OgyZt2AXzcdQa+EjQASnYOIAAmGjJw2roC6tEKRiLiEOhENFJhEMhQEABUqsQkuRzqPEFg9kRhEfDxAsJiElIycuzy7EoqkKnp0mEAgtIwAO51lEamFlbWNsGOluwzSxDO+FSMqFTT1u4c7o0pac2Z2RAABp2qh722oYMyozoK+BPkZuYr9PgFqSQi7md2pdICAAB4AB2A6AU6DAwAAngAVeHggZDR4xcaGfAfVbUDZUCD7N5TRYzWwVByk5YzPHrJjbIl7do0J5gdDKa7HcJPCYLJYUyrUmmfGBrAmM3YecQ9blZHJnVm6dmc2V9Hm6Iw40XfQi-f6AtodZRXNW3ADCJrAGgA4k8BEqFCqiHzGYKqfyRXSJWSdntkmaTgrgVz1fcRvEXsYSbivgRCgb2opgNkcgBlZBwSEgc0pqA5T3WPE-UiMjb4IhaOBYKRl5AAL3wWHooGQRHaS2BqC0Ttk+mLetIABoIOWIJXq1IR4xG6OWyA28ZC8swAALdBQAB0zgd9EZSzXG83GwoG33M0PW5nAhnkr2QA)

変数の後ろに Colon(`:`)をつけて型名を書く、もしくは推論されることで型が決定される。

```ts
let someVariable: string = "foobar";
someVariable = 1; // error

const someFunc = () => {
  return "foobar";
};

let someVarReturnedFromFunc = someFunc(); // infered as *string*
someVarReturnedFromFunc = 1; // error
```

関数の型も推論される。戻り値は明確に書くこともできるし、推論されて暗黙的に決定されることもある。

```ts
const inferredArrowFunc = () => {
  return {
    foo: "bar",
  };
}; // inferred as `const inferredArrowFunc: () => { foo: string; }`

const explicitlyTypedArrowFunc = (): { foo: "bar" } => {
  return {
    foo: "bar",
  };
};

function inferredFunc() {
  return {
    foo: "bar",
  };
} //inferred as `function inferredFunc(): { foo: string; }`

const inferredConstFGunc = function () {
  return {
    foo: "bar",
  };
}; // inferred as `const inferredArrowFunc: () => { foo: string; }`
```

関数やクラスメンバーも同様に、変数`:`型で指定する

```ts
class SampleClass {
  foo: string;
  bar: number;
  baz: boolean;

  constructor(foo: string, bar: number, baz: boolean) {
    this.foo = foo;
    this.bar = bar;
    this.baz = baz;
  }
}
```

## primitives

先頭が大文字の型名は、interface として定義されているので、プリミティブタイプを指定するときは小文字でする

一覧はここに:
https://developer.mozilla.org/en-US/docs/Glossary/Primitive

```ts
const strstr: String = String("foobar"); //これはStringというinterfaceを満たす型という記法になる
const primitiveStr: string = "foobar"; //これがprimitiveとしての指定
```

## type-widening, type-narrowing, type-cast (as operator)

- type-widening - 変更される可能性がある変数は変更のある範囲まで広げられ
  - [公式の例](https://www.typescriptlang.org/play#example/type-widening-and-narrowing)
- type-narrowing - flow 解析によってありえる型のみに狭められる。
  - [公式の解説ページ](https://www.typescriptlang.org/docs/handbook/2/narrowing.html)
- type-cast - 別の型へのキャスト。typescript のコンパイラを黙らせる方法。一応変換可能な型にしかキャストはできないが裏技もある。
  - `value as Type`という構文

### type-widening, type-cast (as operator)

[play it on typescript playground](https://www.typescriptlang.org/play?#code/DYUwLgBAhhC8ECIpQQbgPTomAngBxAgEsBnCAAxLACciA7Ac3IFgAoAYwHs6qIAjOIj580mbPkKkKCYQhas2MeAnbs0EMaDCB7BkCQmoBezQOYMgWQZA0QyBpBkCRDLgIBaAO5EAJiDr0GgGQZA7rFm2A5U5O6mJcPGCA1gzagMdygKaKJoBWDIAiDABcAMJQdHSckFAkJEQMdNicEADkfKX8IOxQAK4kkpBSMKFUGWAAdAAUAEwArAAcAwCUbGxinHwAVtVggHYMgIcMgM8MgAsMekZmc4aAgAw2IA7Oru7evhzcvAC2tWBQfKAA8tOCAN5sEBAAZpycyUJQ1AgADRvfhQABevwAjD0AMxsAC+GnQe2IZGen2+vyotEYqFBEIgdFqFz4IGoePhYwUZzCn1qdHYH3p7EEXXoeGuv2eXx+fwBgPxUNh8OGcAAfBBXqx3mIspw8AiqWI9gcXG5GHNANoMiUMgFR9QAOptpANHqgGCNQAyroAkhkAAXaAfQZAKoMSTYTIZzvYXSuNzuIEeU1GrHQACoIGwAILUBjE1yQTgfcQEMronlYmjuPF8cG-IkkskUypSLI5PIFOi3UDFCB4f5QC7gMkQGNxwilROYvnqdME6Ew3MdEEAFQkZAbeGo8rJuDKPMq-0kDM4F0rYCIXt70veEAH8dK2PcebIBegRcKpcIYBKKNKMn+CFK3VhABY+n6A+glVhgEQwGSoMBDIAPs0SG1ABYNQAIFXWO1ABiGUxAE15QAGdQ8JIbUAQIZTUtW1AE0GJ1mVdLpuVbK9+UFCAuxFN8IEAIIZAHUGQB5Bm0XIIFaMALUAf3lAGMGQAzBnIxJEnGLB6MY8JAAiGDi9hIdhaDwMBAGaGJZAEWGQAShkAS4ZwkAZQY5kAKoZADWGQAOhkAcoZAHqGQAJhnAiCkhUwALBgtQAsBMAU-d0NMNhGIY84wHSKgfReEEkzbYE1w7IU4VYRF+JcrCXWZLpGLcsAfT9XjDyEjijUAWoZAGOGQBOhkMwApBhU+JAA8GQAHBhiQBspRMBzWFdHCPRPDz6Lw3kCKBIiSL9MRAAtFajyI4wBCR0ANmDADOg3r4gNPRAG3jC1zEARQZLFMxIPDCxkIvq34EBANamv84jhXeOrvMagVNpa8YgzYVJuAANzJfJuHrWMLxbBq1pAdsMy27sIHhSoz0bBMMQa9MATTV6u1zCALigHAqmgMHSBuABrQhSRqepCDoEBPwACzrFESFqD4PiIdgiCjYBIc4S7qGAKA8DIRwwAx7AsfremyQ6CAAEk7oxqR7Ho+gvzoJduB-AVQgpyAWYgEAAA8RxAIsbu+0p6ThrJ7DoSoCeoKhV3eTd5duitRwIagJ1KKdoGoWcuAXKAlxXEE9YkMpVvWvdCWyZzbeoE9ywvRrb16GE+h6Z9X2pMQVbVuhACN9cJAAA5QAyvV0capoggqZIUxTAB+GC1AE28yxNRm8rKqWvano24HtsPCAo84dWa+WnzmuFVqsEAfwZAAMGQAohnCNj2O0RJJsdakgA)

```ts
let a = "aa"; // type is `string`
const b = "bb"; // type is `"bb"`

a = "cc"; // letは変更できるからtype-wideningが起きる
b = "dd"; // constには代入できない:Cannot assign to 'b' because it is a constant.(2588)

// objectのメンバは変更できるので、type-wideningが起きる
const mutableObj = {
  foo: "bar",
  baz: 123,
}; //type is { foo: string; baz: number; }

const funcfunc = (input: { foo: "bar"; baz: 123 }) => {
  // noop
};

// type-wideningのせいで引数は型境界を満たさない
funcfunc(mutableObj);
/* 
Argument of type '{ foo: string; baz: number; }' is not assignable to parameter of type '{ foo: "bar"; baz: 123; }'.
  Types of property 'foo' are incompatible.
    Type 'string' is not assignable to type '"bar"'.(2345)
*/

// literalで書いた場合は変更される余地がないため型境界を満たす
funcfunc({ foo: "bar", baz: 123 });

// もしくはas constを使ってもいい
// as constによってtypescriptコンパイラにこのオブジェクトは変更されないことを通知する
const constCastObj = {
  foo: "bar",
  baz: 123,
} as const;
funcfunc(constCastObj);

// asによって型キャストおこなえば入力できる
funcfunc(mutableObj as { foo: "bar"; baz: 123 });
// 全くもって無茶苦茶な引数は変換をかけられないが
funcfunc({ foo: "eee", baz: 123 } as { foo: "bar"; baz: 123 });
/* 
Conversion of type '{ foo: "eee"; baz: 123; }' to type '{ foo: "bar"; baz: 123; }' may be a mistake because neither type sufficiently overlaps with the other. If this was intentional, convert the expression to 'unknown' first.
  Types of property 'foo' are incompatible.
    Type '"eee"' is not comparable to type '"bar"'.(2352)
*/

// unknown形に一度変換をかければコンパイラーを黙らせられる
funcfunc({ foo: "eee", baz: 123 } as unknown as { foo: "bar"; baz: 123 });
// みだりに使ってはいけない
```

### type-narrowing

[公式の解説ページ](https://www.typescriptlang.org/docs/handbook/2/narrowing.html)

[play it on playground](https://www.typescriptlang.org/play?#code/PTAEmj5QJBkBwYDkHsAmBTAdAKwM6DsGALgYwAdBehkEaGQYYZAJhm0HGGQH4ZB+hkBKGQTYZBnhksCuGQToZAphjsD2DIGUGbIAiGQGIMgKwZAzQx1A1wyAFhnKB6hkBJDICEbQIEMgaIZAMgyBABgCwAKHzwAdplyhEAQ1y2AogDdk56wF5QACgA2ASys3ZAAnAC4fOwcIgFV-dwAOAEEQkNsAT1AAH1ArEPiAc2zQcwBXX18ASlAPAD5QZ3h-REqIxuaa+oBvE1BQEBMAXxMTKKdXd29vMcq60B7jPpBQfwAzSUBLBkBzBkA1qMAwF0AQt0BrBkBdBmxAaPVAdQZAaQZAIQZAKIZISUARBllASYZARYZuVUAdeUAYhmg+jOXyOgCkGZ6AMwZAJCagAdTARnc6ACwZLhDALFRgA1owCqDH9tL0VqtIvZbCtLA5zPhkPACXFEik0ulqgs+ktgEIhFsBGMBDTcMlUhk8X0zJZ4L40L54AVpkTUAAjdK4ZAAGTcBVwAAtKoLQCFkLhSiFzHjhot8T5cOlCJSCWMah4vAAiPKFB2M7UgNkcrnO8wFbXCzCi8WS6UOVC4eAxQhWkIAYVsmGQ3kqWtNfV1+sNxrxHvZnKJAjKFTxYxA3kItjSAFs9aFqmMIkXfENUyNjMtAPoMgAMGS76eCy9DIfC4bCAWoY6NhABragAp1QCDDIByhhUqkAK+aAfbijkJJIBNBkA8QyARQYOZJAPIMRlMFisuXgNYAYqVyTUfP30BE7wBrczwADu5mqcyZ-VZbBAE7TQBVmwEcRAD8GI9nmuF48TWHx3UAwAhM0ATocOSfAQ3y-I001AC0rSpUAnztR0nyHXAHVAAAyajQCQkQ0K2TDyOHYomx8RxYwAWSSQBVuUADctjgI60SnKXxSNAAByftkHQYcpIEXAQlKZBU2ZYiB1AABCe0xIqGi6KQgAFEJ4BjC1pMDGsADVbF8VSpJseBkEwEp4GsZAAA9AmsCx8MtZBpNY3ApNQbURKIp9UGs5A7IcoL7UdH0CgdPE3Tw6LYvi1SANM8zQksqTsvsxznNc9zPJ8i9-JE4KBwosLs2ME0TGWAAlId4BCRBdEAU0VAGylQASBVubRABYNQAIFQEU8A2sWLb3JAAmB9vCfCJOrMHqAB4UoAGlAN8P2-Wpf26eCCW8SKCSyq84tKxK9KdZSXQyjSPWAsCtkg6Cj39c8g1QCUpRu2z7vDSNo1CeNE2TdTQBNQYgA)

```ts
// 例えばNode.jsのtcpソケットのデータイベントリスナーはこのようなコールバックを求めるが、
const dataEvent = (
  listener: (data: Uint8Array | string | null) => void
): void => {
  //
};

dataEvent((data) => {
  // ifなどで論理的にその型しかありえないコードパスを作れば、そのパスにおいて変数はその型として認識される
  if (data instanceof Uint8Array) {
    //ここではdataはUint8Array
    console.log(data.byteLength);
    return;
  }
  if (typeof data === "string") {
    //ここではdataはstring
    console.log(data.toUpperCase());
    return;
  }
  //ここではdataはnull
  data; //(parameter) data: null
});

// ただし、objectのキーの存在チェックを柔軟にこなすわけではなく、
const someFunc = (obj: unknown) => {
  //この方法はうまくいかない
  if (
    //この時点ではobjはunkown
    typeof obj === "object" &&
    //この時点でobjはobject | null (ECMA仕様的にtypeof null === 'obejct'はtrue)
    obj !== null &&
    //Property 'someValue' does not exist on type 'object'.
    typeof obj.someValue === "string"
  ) {
    obj.someValue; //Property 'someValue' does not exist on type 'object'.

    // typeof input === "object"が通るときに、
    // inputをインデックスアクセス可能なものととらえてくれないのは
    // いろいろな理由があるのだと思われるが、
    // とりあえずunknownは取り扱いにくい
  }
};

// Recordが入力値である場合は、
const someFunc2 = (obj: Record<string, unknown>) => {
  if (typeof obj.someValue === "string") {
    //この方法でうまくいく
    console.log(obj.someValue.toUpperCase());
  }
};
```

## interface, union-type, type-guard

変数が持つべきオブジェクトの構成を型とし定義するのが `interface`.

`type` は `interface` とほぼ同等だが、union-type をとることができる。[詳しい差は example などを参照](https://www.typescriptlang.org/play?q=278#example/types-vs-interfaces)

`type-guard` はある関数が true を返すコードパスにおいては引数となる変数がある型であると思っていいことをコンパイラに伝えるための特別な関数シグネチャ。

### interface

typscript は"構造的"型付けの言語(Go なども同様)。

あるオブジェクトが特定のキー名で特定の型のメンバを持っていれば、クラスのインスタンスでなくても型境界を満たすことができる。

この"構造"を宣言する方法が interface

[play it on playground](https://www.typescriptlang.org/play?ssl=7&ssc=1&pln=6&pc=2&q=278#code/MYGwhgzhAEDKD2BbApgUQB5kQBxMgwuFNAN4CwAUNNBEsgLLKIBGyATgFw0AubAlgDsA5tAC80AEQAzePAmVqtFI24ALeABMAFAEouEXoJHkq1aG2TcArmwGTmYNvNMBfSm4qVKweAIPRuZAMAMSsBYDFoLUFsK24uBBQMLFwCIggdMQA+UgVoAHp89y8KQJCw4C0BZAB3ODpknDxCSAhdHUoy7lDwrRIlBiZWTkkARyt0CQAaGjoVdQ0uXWyxqwmJFx0gA)

```ts
class SomeExampleClass {
  someMember: string = "foo";
  someMethod(): string {
    return "bar";
  }
}

const testFunc = (input: SomeExampleClass) => {
  //
};

testFunc(new SomeExampleClass());
testFunc({ someMember: "qux", someMethod: () => "quux" });
```

クラスインスタンスを引数に求めているのにかかわらず同じ構造のオブジェクトでも型境界が満たされ、エラーが起きない。
引数やシグネチャの返り値がクラスインスタンスになっているとき、`instanceof`で判定するのはうまくいかない可能性を示す。

以下のサンプルで interface の基本的挙動を示す。

関数シグネチャを持たせるには、プロパティ名指定せずに`(input: InputType): ReturnType`と書く。`extends`によって既存 interface からの拡張ができる。

[play it on playground](https://www.typescriptlang.org/play?&q=278#code/JYOwLgpgTgZghgYwgAgMoHsC2ECS5rxLIDeAsAFDLIzroBcyAzmFKAOYDcFAvhRQuhDNqAVxAIAShACOI4FAiM8kWIhQBeZAApgABxFgGGbMoJqAlMnUA+EhSoCh6ADYQAdM-Rsd+sG5ro5lzk3MEUMGKSMnIKSviqSFrE1LQMAEQARnBQacjcQeGRUrLyiqYJEEnIWQBe6bIAHrn5HMgA9G3I0FDoUNUQngDuFG0AVBQAglBsItjgyOgwyGAAnrooAOTJtQzMrCCceRvIwIzIIOhgyHCMjMBsIHAZrsvoyLrZcNgqC0ur68gNsZcPFCBANm57MgAPIZABWEAQV2cwBUcGcyEwcBWCxAzhxjHWCGAMBxAGsLoMQO8eusoGBgIoADTXEAAE0BtWObPQinOly6DVOV1AyzWm2B5TBEK0ACYAMwAFgArOYKKM2nxyB1kHC4AA3G4IVi6MCAawZAFIMgEUGQDRDIAi1MADqaAewZYQikYBzBkAQgw2wB2DO7AIAMoBUYMAMgyOwBJDIALCMAf87uwCyDL6nYBlfUAbI5em0UINmIhStQAFQAFnAwABRBq6WKMRQAMUiDMEdko2lAvgYIFmGWg5jbHegwSojCwEGhYAL0AA0hAVrsWOxgrxyCM2o6fYB2hkADQyAWYZAIMMgGOGcOAQGNACYMWYqFDZiOc2RQjmEg+wtfEDFzSELxbLFcUVcYj6RwEEwT3hAv5aAAjAqBTkLeLjuJ43hAb+bhASOY5QJOKxuGA6AAKq6HSADCNyVOYkFLicoJqE6gAwKoAsCruoA4JEOoAe2qAMXagAAUfG-qAOoMgD6DIALBqABAqAmAAhGgCqDIAMQwZuQp5gsgL4QIRIAAEIQFIF4IFeCgcmQTajuwjAMFIAhQGyAA8ezsCyYgUugVLWAA2gAuvOWoyWockUUgikqWpl7XtpUJ6QcjCyj2mCdlATkuYu5DqZpN6CHeQ74YlhgecGajeapEBxf5YTkEBKVCH4QVsIwrQ6loFboHSqyWPJWW+Rp-mYQW+mGYivRmRZBxWSANl2U5FCFalrX6bKFWdFVtLQHV6XZgpcDKdluVaWNwWhecvaRY5Wo6s1tw+oAEQyAGIMZoAAYQA0kDsow53uoAh0aAKz6gD3yoAvwGZp5N5tc4bLyYKN1smcDVLT5OV+VpjYDkOExsmyqL-o8zjobsKzhS4PBAA)

```ts
interface SomeInterface {
  foo: string;
}

const funcRequiresInterface = (iput: SomeInterface) => {
  console.log(iput.foo);
};

funcRequiresInterface({ foo: "bar" });
funcRequiresInterface({ baz: "qux" }); // error below
/*
Argument of type '{ baz: string; }' is not assignable to parameter of type 'SomeInterface'.
  Object literal may only specify known properties, and 'baz' does not exist in type 'SomeInterface'.(2345)
*/

// javascriptにおける関数はObjectであるので、interfaceが関数を表現できるのは当然である
interface InterfaceThatExpressesFunction {
  (input: number): number;
  someOtherKey: string;
}

//関数のシグネチャを持つinterface
declare const someFunc: InterfaceThatExpressesFunction;
someFunc(123);
console.log(someFunc.someOtherKey.toUpperCase());

// interfaceは同名で複数回宣言でき、した場合合成される
interface InterfaceCanBeRedeclared {
  things: Record<string, unknown>[];
}

interface InterfaceCanBeRedeclared {
  things2: number[];
}

declare const someConst: InterfaceCanBeRedeclared;

someConst.things; // (property) InterfaceCanBeRedeclared.things: Record<string, unknown>[]
someConst.things2; // (property) InterfaceCanBeRedeclared.things2: number[]

// classのように`extends`で拡張可能
interface childInterface extends InterfaceCanBeRedeclared {
  someAdditionalKey: symbol;
}
```

### union-type

[play it on playground](https://www.typescriptlang.org/play?#code/C4TwDgpgBA8gRgKwgY2ANQIYBsCu0C8UASigPYBOAJgDwDOw5AlgHYDmANFDswNbOkB3ZgD4A3AFgAUKEhQAguXIYQmXAS69+QgNoBdCdPDQAUrVLNVeKIXpM2UAD5RmOALZwI5R1DilSWCAxmbxcsLG94JFRLaCcFJRVsPAMpZHN6KAA3JIgARgAuKFNzGOsoACIAMz84DHJagC9ygzTmDOy1ACZC4oscstzOgGYW9OAsnKGesz61MoZk1LGJtQAWaZL+wlCsFMkAen2oADJAcwZACBVABCNAe+VAX4CltvHK7mRn5mQygAoWMBxgQoA3lBqqRCrYWKwoABfE5QIG1chghgQ6EASmswjhUigUEOUihoiAA)

```ts
type ObjectValue = Record<string, unknown>;
type ArrayValue = unknown[];
type JsonValue = string | number | boolean | null | ObjectValue | ArrayValue;

const value1: JsonValue = "foobarbaz";
const value2: JsonValue = 123;
const value3: JsonValue = true;
const value4: JsonValue = null;

// &で合成可能
const funcfunc = (input: { foo: string } & { bar: string }) => {
  //
};
```

### type-guard

クラスインスタンスであれば`instanceof`を使って判別できるが`type`や`interface`は型を自前のバリデータによって保証する必要がある。
`(input: unknown) => input is TypeOrInterface`のような返り値型を書いた場合のみ、typescript コンパイラはこの関数が true を帰すコードパスは指定された`TypeOrInterface`の型なのだと信じる。

[play it on playground](https://www.typescriptlang.org/play?#code/MYewdgzgLgBBIFsCmApeYDKUBOBLMA5jALwwDkARABYggwBGAhtg4wF4VkDcMA9L4EMGQFYMgfwZA1gyBlBkAxDICCGAG6MANrgAmQlBgDyAOQCwAKAOhIsAA7MISFSRgadAOnPZLACnjI04LHkIBKLgb8xtAwTpYqAFwwjGAAngaBvIAWDBKAsQyA5gyAgAx22o4WSID2DICMmoDqpoDqDID6DEmAsgxQVNggAO6AmgyA0QyAdgxZBlCxpkgwmvQAVkjAUABqSgCu-aQASqMg2CoAPNA+BAA0MFNgANZgTWAAfAH6PX0wAILY2Iyxk4ozNrsHRwDaALpnF-2eYI9nqR1vgiAAfGBgKYIehIFgQ+i0RRIGIwCFQxSKNEDYajCbTfoQm53B4Es78QAyDIBjazKgBEGQAr8YAohkAJApJWmACQZWoADBlSgDEGQBADIlLEgolQoFBTBAIvxGrL7NgAGbAAC0VlwUCW9iWBF4iuAuqVAA4AEwAVgAnPYxQhFAl9PxAGiagAuEwAlDIBnhkA-QyAH4ZAKsMgHKGL2AToZANIMgEiGQCmioBspUAqgxSCo5drMgqAVXkKeHAMYMgDMGaqZmSAKQZg2VAPIMQlpgvtvEAugztCqAQIZk2VQ+1AAsMgCuGQDjDF6PRTAKP6gEDI9KAIQZWuXEoB8c0Aofqx1pRwBJDIBTuUA0HJJMqZgz4KBwhWMYD9ACiAA8+mMrABJMCmKZQbxTMZTbD9ADeBhgMAVtCiIMIZ1fTGwUShGE4R-Vg2E-HBQS+ECAEcpgPKJn30V9X1guDwI2EDX1AbACBFOAIO-F8YAAXzOYi7WCWBcAgQ9j03FRz0va8cFvKB71mGAXCI-gJAkAoYlidpAG8GXkKUAX0suQpTJfggYA8FMKACleQ5GjAWdAArjQALTVXWlWiI-AmKiZSjgMXwogMq8YGomBaLxM8LyvG87wfEhjhgRDX1wBVON+EBvIs2AAEJiFICgQFxMYKDRCEApIELISmTFfHcojXwfNjsDAN8lEsEDyMMJCYEomAH2w6xSFixgIBgBYyrWAith2fYVJOM5uN4EqkAyrLfgKREQGRGJ2hEIj0vvLKuMK19fO80qlhUex3zoELQq-AgooAMg21KYBmzqyvsP84tCwDYWwTbtqm65bnuexqOJe4XDm5ZDvYZKtp256FqYNh7CQOQ4ViFwXCQZLiDcvbZniig1ood7LuQ3beiQPz9vm+xYIPY6YDCiKoAuz7FhezGYGC0gMSxD6rr2p6iYWkmqpqun6o2bZjNU45fAxqY4OxmGGoJ6nkdR2mDoZ6ravmlnQTZ5qjk5+xsNwvnYaI-wDFIu0vM46jbJPBiHOY7BWPYlwwisXxko8orwHgZF7EUEACDN-IFqW7Zze+5gPdd162B95wrG5g91f0YigA)

```ts
const someJsonString = '"hoo bar baz"'; //ちなみにこれもvalidなJSON

const parsed = JSON.parse(someJsonString);
//const parsed: any

//ところで、JSON.parseは失敗したときthrowするので、
type ObjectValue = Record<string, unknown>;
type ArrayValue = unknown[];
type JsonValue = string | number | boolean | null | ObjectValue | ArrayValue;
//が正しい返り値といえるだろう。
//see: https://www.rfc-editor.org/rfc/rfc8259.html

//外部インターフェースから入力されたJSONの値は何が入ってきてもおかしくない。
//そのため何しらのバリデータが必要である。

//期待される入力を以下として
interface ExpectedInputStructure {
  foo: string;
  bar: number;
  baz: string[];
  qux: {
    quux: string;
    corge: string;
  };
}

const isExpectedInputStructure = (
  //ここはanyのほうが楽だが、typescriptはunknownを推奨している
  input: unknown
): input is ExpectedInputStructure => {
  if (typeof input !== "object" || input === null) {
    return false;
  }

  const record = input as Record<string, unknown>;

  // return typeはbooleanのみ
  return (
    typeof record.foo === "string" &&
    typeof record.bar === "number" &&
    Array.isArray(record.baz) &&
    record.baz.every((e) => typeof e === "string") &&
    typeof record.qux === "object" &&
    record.qux !== null &&
    typeof (record.qux as Record<string, unknown>).quux === "string" &&
    typeof (record.qux as Record<string, unknown>).corge === "string"
  );
};

if (isExpectedInputStructure(parsed)) {
  console.log(parsed.foo, parsed.bar, parsed.baz, parsed.qux);
}
```

## typeof, keyof

- [typeof operator](https://www.typescriptlang.org/docs/handbook/2/typeof-types.html) - value から type への変換
- [keyof operator](https://www.typescriptlang.org/docs/handbook/2/keyof-types.html) - type が index access 可能な場合 key の Union type をとる。

[play it on playground](https://www.typescriptlang.org/play?#code/MYewdgzgLgBKbAE4FMrIPICMBWMC8MA3gLABQMMYArgLbKICWwAXDAIwBMAzADRkXRGYAOYAZBmkQBDADasARADMQITFMTy+5GJhVyYURFWRaBATxqsAyhd0yAFPLUAveQEotAXzJkoZgA7IMAAqAcgAYoggNFi4BH6BIIpw4EioGDhkAPQAVL5hIWGR0bH4RPwUlLT0TKzUNJj0ANwVAoYMIuKSsqyCHcIt2hS6IPojMshSYIOVMBAWvbajg96kOVk+pAlBANLIZuiKoYFlANb7SYWBxTGZpFlZ2zB7B0cFBPL1NcDyMAA+MHkfU6Enosl+AKceghgPmNHkQA)

```ts
const concreteObj = {
  numeric: 123,
  stringLiteral: "foobar",
  bool: true,
  sym: Symbol("baz"),
};

type TypeFromObj = typeof concreteObj;
/*
type TypeFromObj = {
    numeric: number;
    stringLiteral: string;
    bool: boolean;
    sym: symbol;
}
*/

type KeyOfType = keyof TypeFromObj;
//type KeyOfType = "numeric" | "stringLiteral" | "bool" | "sym"
```

## optional type

colon(`:`)の前に`?`をつけると optional になる。

[play it on playground](https://www.typescriptlang.org/play?target=6&jsx=0#code/MYewdgzgLgBCAOUCW4CGAbAYgVzMGAvDABRJjzZQD8AXDNAE5kDmAlIQHwwDeAsAFAwYZClADcAoQHopxeKgaoAtgFMoKhuxGU6jFjAA+MXABMVAMzIqTAgL4T+AsuobnUwFTADyiFGAwAkmAubh48kjDmICC0MABEAEYKcWIwMnIMCBpQAJ7sPsho6EEh7ioAdFExdInJhsZgZpZg1hFJAF6xAIwATADMqenwmfDZed6+RSUaoRUd3f31phZWNoIwAI7YAB6xbugQKoOyw1kMufmT-sXBM2XlW7t0+4dLjSsta7YCAsDoqBAIBNCtcAML-QHhdYQECqACyKiUCQ0sT0YGYxwyZwuwL8GHBAIg5Rh8MRyIYqKgTHRbyaqzsQA)

```ts
const optionalFunc = (input?: string) => {
  input;
  //(parameter) input: string | undefined
};

interface OptionalInterface {
  foo?: "bar"; //(property) OptionalInterface.foo?: "bar" | undefined
  baz?: 123; //(property) OptionalInterface.baz?: 123 | undefined
  qux?: false; //(property) OptionalInterface.qux?: false | undefined
}

class OptionalClass {
  someMember?: string; //(property) OptionalClass.someMember?: string | undefined
}
```

## non-null assersion operator (Postfix `!`)

https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#non-null-assertion-operator-postfix-

- nullable な型の末尾に`!`をつけると non-null として扱われるようになる。
- 使わない方がいいです。

```ts
function liveDangerously(x?: number | null) {
  // No error
  console.log(x!.toFixed());
}
```

## class access modifier

https://www.typescriptlang.org/docs/handbook/2/classes.html

公式のクラスの項にある通り、アクセス修飾子が使える。
ただし、これはあくまで typescript の type-checker のみで有効であってランタイム中で要素を列挙するなどした場合は普通に見えてる。
厳密に private にしたい場合は、[javascript の private class field](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes/Private_class_fields)を使用する。[node green 曰く Node.js v14.x 以降なら使える](https://node.green/#ES2022)

[play it on playground](https://www.typescriptlang.org/play?target=6&jsx=0#code/MYGwhgzhAEDKD2BbApgUQB5kQBxMgwuFNAN4CwAUNNNgK4BGIAlsALLKL3IBOAXNGAB2AT2gBeaACJJAbkrU6jFgJAR4ABQbM2HLnwEjxU2fJrcmANzAAXZGcs3k7Tj35DRE6XKpn4t4LYAJr7+Qc56boaeJhQAvpSUwPCCENbQyJg4eACSKdZCwHYSgsgA7nBIaJm4BEQQABQAlN4ZWDW5qQXIAHSK2uE8LdU5eV3dYKoaWiwD3ENtI52Chb3mVraz3gD0AFSU6tzw2DzWogDk2GuOs2fQTDCXDrYGwckgomDAhVBMjHalTGsAAsmIJoKBIDAzggUBgFrVIWduvUAEwAZgALABGRqUHZbSitLLIDr5ZY9S5+ZABZCBTaUXb7Q7HbinaAXQ6hWk3O4PTnUoIvaBvD5fZA-P7QAHA0HgursmFVeGERFCwEwCAMCFQcXIlEYjEAVlxFHxCQo2pgGFsgiCcOJKuIGRtgRgivtNUdMHIPiSeW4tAC8G4TVIpmompZTW81GoMogqyeTl0PCMkmAQKEAHNaTFY9BGT5qAcjidzo91smXNxbvd7JWhSKBGKJXgpYCQWDLQrKh68F6kajMTjw9AzUXoPHVlSaXSU9w0xns7mY9B4nEgA)

```ts
class SomeExampleClass {
  publicMember: any = "";
  public alsoPublicMember: any = "";
  private privateMember: any = "";
  protected protectedMember: any = "";
}

const exampleInstance = new SomeExampleClass();
exampleInstance.publicMember;
exampleInstance.alsoPublicMember;
exampleInstance.privateMember;
/*
Property 'privateMember' is private and only accessible within class 'SomeExampleClass'.(2341)
*/
exampleInstance.protectedMember;
/*
Property 'protectedMember' is protected and only accessible within class 'SomeExampleClass' and its subclasses.(2445)
*/

class ExtentedExampleClass extends SomeExampleClass {
  constructor() {
    super();
    this.privateMember = "changed";
    /*
    Property 'privateMember' is private and only accessible within class 'SomeExampleClass'.(2341)
    */
    this.protectedMember = "changed";
  }
}
```

## module, namespace, interface 合成

interface, [namespace, module](https://www.typescriptlang.org/docs/handbook/namespaces-and-modules.html) は同名で宣言するとエラーにならずに合成される仕様となっている。

例えば`window`オブジェクトにメソッドを追加したときに型として追加するのは、以下のような`d.ts`ファイルを tsconfig.json で include に入れるか[Triple-Slash Directives](https://www.typescriptlang.org/docs/handbook/triple-slash-directives.html)で読み込ませる。

preload.d.ts:

```ts
// electronでpreloadを型として追加するときのd.ts
import type { apiKeyName, methods } from "../../electron-src/preload";

declare global {
  interface Window {
    [apiKeyName]: typeof methods;
  }
}

// apiKeyNameが`"electronMain"`でmethodsが{foo: () => string}のとき
console.log(window.electronMain.foo().toUpperCase());
```

import が副作用によって別のモジュールを変更するような実装を module、interface の合成によって表現できる

例: [axios-retry の型定義](https://github.com/softonic/axios-retry/blob/master/index.d.ts#L62-L66)

axios-retry/index.d.ts:

```ts
interface IAxiosRetry {
  //...snip
}

export function isNetworkError(error: Error): boolean;
export function isRetryableError(error: Error): boolean;
export function isSafeRequestError(error: Error): boolean;
export function isIdempotentRequestError(error: Error): boolean;
export function isNetworkOrIdempotentRequestError(error: Error): boolean;
export function exponentialDelay(retryNumber: number): number;

declare namespace IAxiosRetry {
  export interface IAxiosRetryConfig {
    //...snip
  }
}

declare const axiosRetry: IAxiosRetry;

export type IAxiosRetryConfig = IAxiosRetry.IAxiosRetryConfig;

export default axiosRetry;

// axios-retryモジュールの中でaxiosモジュールを宣言(declare)している。
// 結果は合成され、axios-retryのインポートが副作用でaxiosを変更していることを表現している
declare module "axios" {
  export interface AxiosRequestConfig {
    "axios-retry"?: IAxiosRetryConfig;
  }
}
```

axios のある関数の引数に`"axios-retry"`キーの定義が追加される

## index signature, mapped type

[公式の解説](https://www.typescriptlang.org/docs/handbook/2/mapped-types.html)

```ts
interface IndexAccessible {
  foo: string;
  bar: string;
  baz: string;
  [x: string]: string;
}
```

`[x: string]: string` - [javascript の computed prop name](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Object_initializer#computed_property_names) のような構文をとったこのシグネチャが index signature で、これがある場合動的なキー名からの index アクセスを型的に許すということになる。

余談：index signature で object を HashMap のように扱うぐらいなら[Map](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map)を使った方がいいので、後方互換性以上にこのシグネチャの価値はないかもしれない

mapped type の基本は index signature に似ていて、以下のように感じ。

```ts
type SomeMappedType<
  K extends readonly string[],
  T extends Record<string, unknown>
> = { [P in K]: T[P] };
```

- P - K から取り出される型、prop name になる
- K - prop name の Union Type
- T - P にアクセスしたときの Value 側の型,このフィールドで P を使うことができる

[ここの解説が詳しいのでここで](https://zenn.dev/qnighy/articles/dde3d980b5e386)

適当に実用的な例を以下に

エラーコードリストに自動的に連番のビットフラグを割り当ててそれを Object で扱えるようにするもの。

[play it in playground](https://www.typescriptlang.org/play?target=6#code/MYewdgzgLgBAigVwJbAKICd0nQYRAEwFMIYBeGAbQFgAoGGAIgDkB5AfVQCVOXOGAaWvQYBJJgBUuTAIIAZDt14ChjHCyZNUOcSPVtOqAGIBVAMqoAIsrqNDslgHU2aiT3lcefQTYanxB6QBZNlkRQJFxBU9rYT8A4L9pSSilb2FDMTk2UxEALVQUrxUGQ04ggtQmNQsxAHFCmMZ-aSZTAAVeSLbpMsDUSU4GtNV1TW1dJjYRCxCwiKHitp5xFjV5ADVdWSSJxtEmdblpthWAaUq96Ta20JwdvQ9U4pxOAE02lbYAIWNDQy4OAANHCoSyWPbnV5sYxtCxJCqKIo+aSoaQzULhSIBHAACXBw2Y7E20i+sgK3XEOL2L3en0eSIAujAAIYkUCQKAAblotHZ0BgAEdkGhMNg8ERAsyAA5kGAsABGACtCMAoAA6ABmWAAtqgwFB0EhiAAKFSIFAYLC4AjENXa6Wmmz0Y2gIj8GBIMBEAAeAEoyAA+Siuwjuz0+sikcgABhgAH4YLGAFyJ+XRgCMMAAPFmYMbw4RvTAALQwdO+pmsmB8qAqX20X3cmgAegAVLRm83pJhmQBPNVSrBQEBQXtSwh26WAOwZANHqgArjQBrUYAZBkAYgyAPwZAPIMgBEGQDSDIAtBi1IF1+sNxEA5gwzwCjBoBGDSXgAjbQCMmrzwPyhRbRdaJdKUwBvGAqegKAAaxTaBDTAABzBkUzABBtXlQh0CbABfWhW2bHkaBreBhUtMUbUlGVyDfEUrXFQhCJZEgfxUCg2g9MAYFHccQA1HD3zIm0IAoWD4MQhloJgXiEKQ2hkKbNtnw5djSPwr8pV-ACYFYQoYLgkSm3oegxAGGR3ERNS+NEp0YBcMYdD0AwTHMCxDI0pS7EcZx1H8Fh9M8OzEM0rS4lRYIMXmelPOMrSYF8oJsnEeFVKE9SvIczJ5ByfIYuE+KTNKcoOCqFgaiYeogtiozvPoNUypgABOGBtWwQgYDKtUStMt4PnYQq0uM1CaHQoA)

```ts
const QuicErrorCodes = [
  "NO_ERROR",
  "INTERNAL_ERROR",
  "CONNECTION_REFUSED",
  "FLOW_CONTROL_ERROR",
  "STREAM_LIMIT_ERROR",
  "STREAM_STATE_ERROR",
  "FINAL_SIZE_ERROR",
  "FRAME_ENCODING_ERROR",
  "TRANSPORT_PARAMETER_ERROR",
  "CONNECTION_ID_LIMIT_ERROR",
  "PROTOCOL_VIOLATION",
  "INVALID_TOKEN",
  "APPLICATION_ERROR",
  "CRYPTO_BUFFER_EXCEEDED",
  "KEY_UPDATE_ERROR",
  "AEAD_LIMIT_REACHED",
  "NO_VIABLE_PATH",
  "CRYPTO_ERROR",
] as const;

const quicErrorCodeMap = Object.fromEntries(
  QuicErrorCodes.map(
    (code, index) => [code, index === 0 ? 0 : 0b01 << (index - 1)] as const
  )
);
/*
//Array.prototype.mapの型推論がうまくいかずfromEntriesで型情報が消失
const quicErrorCodeMap: { 
    [k: string]: number;
}
*/

const QuicErrorCodeMap = quicErrorCodeMap as {
  [P in typeof QuicErrorCodes[number]]: number;
};
/*
const QuicErrorCodeMap: {
    NO_ERROR: number;
    INTERNAL_ERROR: number;
    CONNECTION_REFUSED: number;
    FLOW_CONTROL_ERROR: number;
    STREAM_LIMIT_ERROR: number;
    STREAM_STATE_ERROR: number;
    FINAL_SIZE_ERROR: number;
    FRAME_ENCODING_ERROR: number;
    ... 9 more ...;
    CRYPTO_ERROR: number;
}
*/
```

## utility types

[ドキュメント](https://www.typescriptlang.org/docs/handbook/utility-types.html)

[この辺で定義されている](https://github.com/microsoft/TypeScript/blob/main/lib/lib.es5.d.ts#L1468-L1561)

組み込みで用意されてる utility type。
ユーザーが定義しそうな典型的な型を公式が先んじて実装しているもの。

ただし、template-literal type 系のユーティリティーは例えば

```ts
// 'hoge'のようなもの食わせてCapitalize<"hoge">すると出力が"Hoge"という型になる
type Capitalize<S extends string> = intrinsic;
```

のようになっている。内部のコンパイラと組みついているのでユーザー定義で何とかなるものではないようだ

あんまり投げっぱなしなのもあれなんで

以下の例では組み込み型の`Required<T>`を利用して optional 外しをして[Indexed Access Types](https://www.typescriptlang.org/docs/handbook/2/indexed-access-types.html)で型情報を interface から取り出している

[playground](https://www.typescriptlang.org/play?target=6&jsx=0#code/LAKAlgdgLgpgTgMwIYGMYAIDKB7AtjAETBSnQG9R10FtsAudAZyjkgHMBuS9AIyTgYQArrh7wuIKnwBeAfgY9aAGxhIIEgL6hQKbBGbpdEFHBiwiJdAF5y3GvXQAiAI5CAHo4A03PgPQBGACYAZm9JXiRpBmQlRhgwjQkjAxg3FlQoADUkJSEYACEATwBpGELrdAAeABV0VNgIABNGdABrMuwELDxCYigAPgAKbnbChmrQAEoGACUYVzBTRsqcfAsBgG1qgF1rfttwgHpD9DmFpfQAVSgwJTAocurCgAcYQHsGAHElbD4lQGsGQD4roAJBkAZgwgwCeTn8wLhntg4FBAFYMgHUGQDmDIB-eSBgGiGbimKBCOAQQx6ExmXokDajbaaCQ6PQGewVepwDLZXIFEplQaOeyOSYSY7JUj2BjMVgQNi0-SkXyMtLMkisvJFUqFLm+Xn8w6CiJ+YSieCSgwyWXpBU5JUc1WOGQa0ACunSyIKZSqCAMSoAWlO8yEixgy2q-RR2GeNz0OUAaJpIwAyDIBVBkAMQwgzGAaQZAJEMimwKjU6AAPughE0YAhIP6UW8EYB5BkAgAwZrMQaOAFfjAFEMgBIFP4IwDGDCDACIMmKAA)

```ts
interface SomeDict {
  foo: string;
  bar: number;
  baz?: boolean;
}

const concreteDict = {
  foo: "qux",
  bar: 123,
  baz: false,
};
const extractValueByKey = <T extends keyof SomeDict>(
  key: T
): Required<SomeDict>[T] => {
  // Required Utility TypeはGlobalに生えてて特にimportなしで使える
  return concreteDict[key];
};

const foo = extractValueByKey("foo");
//const foo: string
const bar = extractValueByKey("bar");
//const bar: number
const baz = extractValueByKey("baz");
//const baz: boolean: <- Required<T>でoptional外しがされてるからboolean | undefinedではなく、booleanが返り値になっている
```

## conditional type, never, infer

### conditional

[三項演算子(ternary operator)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Conditional_Operator)の構文は以下のようであるが、

```js
// ternary operator
const foo = condition ? if_truthy : if_falsy;
```

conditional type もこの三項演算子みたいなもので

```ts
type CondType<T> = T exnteds SomeInterface ? TypeTruthy : TypeFalsy
```

`T exnteds SomeInterface`が condition 部分、後ろはそのまま三項演算子の構文みたいなもの。

この type コンテキストにおける exnteds の意味あいは、`⊂`が近く、`A extends B`は`A⊃B`を意味する。

つまり、以下のような挙動を示す。

```ts
interface InterfaceB {
  someKey: string | number;
}

type IsExtendingB<T> = T extends InterfaceB ? "True" : "Flase";

type shouldBeFalse = IsExtendingB<{ otherKey: string }>;
//type shouldBeFalse = "Flase"

type shouldBeTrue = IsExtendingB<{ someKey: string }>;
//type shouldBeTrue = "True"

type alsoThisShouldBeTrue = IsExtendingB<{ someKey: number }>;
//type alsoThisShouldBeTrue = "True"

// typescriptは構造的型付けであるので、余計なキーがあることは気にしない
type itIgnoresAdditionalKeys = IsExtendingB<{
  someKey: number;
  someOtherKey: boolean;
}>;
//type itIgnoresAdditionalKeys = "True"
```

### never

ありえない状態を表すための型。

conditional type の分岐の中に never 型はいっていると、never が返されるときにコンパイルエラーが発生するようになるのでそのような使い方をする。

[公式のドキュメント](https://www.typescriptlang.org/docs/handbook/2/narrowing.html#the-never-type)だと switch case のハンドリング忘れを検出するような使い方ができるとありますね。

> The never type is assignable to every type; however, no type is assignable to never (except never itself). This means you can use narrowing and rely on never turning up to do exhaustive checking in a switch statement.

なんにでも代入できるが、何も代入できないとのこと。

### infer

この conditional type の文脈で飲み使えるオペレータであり、

```ts
type UnderSomeProp<T> = T extends { someKey: infer R } ? R : never;

type foo = UnderSomeProp<{ someKey: "foobar" }>;
//type foo = "foobar"
type bar = UnderSomeProp<{ bar: "foobar" }>;
//type bar = never
```

上記のように、`infer TypeName`とすると、その条件に合った値の型を`TypeName`に代入しますよ、という挙動だと思えばよい

## generics

- 普通、generics は入力された型に基づいて処理を分岐させるためのもの
  - java における List\<T\>や、C++における Vecotr\<int\>のようなもの
  - 静的型付け言語では、型ごとに実行バイナリを生成する必要があるのため、コンパイラが実際に作るべきコードパスの種類を判別するために必要な構文。
- javascript のほとんどすべてのメソッドはジェネリックである
- typescript における generics は入力された型に基づいて引数や返り値の型を指定する方法

以下のような構文をとる。

```ts
function identity<Type>(arg: Type): Type {
  return arg;
}

const arrowIdentity = <Type>(arg: Type): Type => {
  return arg;
};
```

入力される型は複数あってもいい。

```ts
const genericFunc = <T, U, R>(arg1: T, arg2: U): R => {
  return; //snip
};
```

型に境界条件を持たせることができる

```ts
const genericFunc = <T extends Record<string, unknown>>(arg: T): T => {
  return; //snip
};
```

例として以下のような generics を使った関数を示す。

- 境界条件は`Record<string, unknown>`
- `someKey`に値がある場合以下を返す
  - string | number なら string | number
  - それ以外の場合、`symbol`
- `someKey`が存在しない場合、`Error`を返す

flow 解析がうまくいかず、`//@ts-ignore`でコンパイラを黙らせてるところがある。(`@ts-ignore` なしで書ける方法をご存じだったら教えてください)

[play it on playground](https://www.typescriptlang.org/play?jsx=0#code/MYewdgzgLgBA5gUzAgTgS2AMQK5mDAXhgB4AVGBADyiQBMIYAlBUFW46dMOAGhlwDWYEAHcwAPnEAKAFAwYAQxRwAXDFIyAlGvJUaYejADeMCCAC2CANIIAnmrRgAZqiYwAvnJgB+N3roMnI5wMAA+MGDY5gBGqF7yvozxMGoQtjEgADZeagCiKCggKITixl5oTjBSAPLRAFYsUAB0AA6FUCBQti0ITQAWChDVYgAKhT0oXU3ACpmZUkq8MABEZpY2tsuammXy8hVVXT0glYtNa9Z2hAREq1BccMth4UcIJ4rK5xaXtte3kTFUFtdnt5AB6MEAASgEAAtGg4MIUAhkvJkVBsCgwB84F91nYANyomAQ6FwhFIlGg9wUTIQBAwdGY7EAZXS0SyTScRSky25IGiSi2RPknnBUJh8MRRSpjIQGKxEQQIhg+UKKF5wpk7iJMghoEgsAgfRA2EytAAQggAMIWFpoTIKKBocBqoqEeBIVAYHB4Xn8wUoLVggBUMgAgsookhYO9XjAAORBbgJmBoBjCWCDCAUhTRTIMjowFpKBSWGjFOPdBkJ5isdjJpaCYRicQJppSABMAGYACwAVk0MhDYJkMgN0FMJrNloQLPuwQ9iGQ6CwuGAUhMFw2aj5IBAT3cmiJBykr3extN5qt84eMAAhDcVo3gVA+oUVcgVW6Natp+aYFiUwF24LUJyNf9ZwAOSiWJiiIZdvTXP0t2+HcYAARh7DxjxkU9z0qS8ZytGDAWKR9-lgoEdjfD8lW-AoeT-K9aEAhkATgsDwEnIjrzndksiXL1V19DdUPxewIjNTIcJPSoz2rC9IJvASZIo59VNfd9RHo1VGN-XjWKAtIMkyLVx24iCWKtH8hJXH1103RRMz6VB0MiOZZLw+T7ykQybP0tNDQUPA3kqH9thgWidK-PT1V5Qy2LiopzIhKCQBgTIQDgCAmiAA)

```ts
const genericFunc = <T extends Record<string, unknown>>(
  arg: T
): T extends { someKey: infer R }
  ? R extends string | number
    ? R
    : symbol
  : Error => {
  if (Object.prototype.hasOwnProperty.call(arg, "someKey")) {
    if (typeof arg.someKey === "string" || typeof arg.someKey === "number") {
      //@ts-ignore
      return arg.someKey;
      //@ts-ignore
    } else return Symbol.for("foobar");
  }
  //@ts-ignore
  return new Error("");
};

//const shouldBeCompilationError = genericFunc("foobar");
/*
Argument of type 'string' is not assignable to parameter of type 'Record<string, unknown>'.(2345)
*/

const shouldBeString = genericFunc({ someKey: "foo" });
if (typeof shouldBeString !== "string") throw new Error("should be string");
const shouldBeNumber = genericFunc({ someKey: 123 });
if (typeof shouldBeNumber !== "number") throw new Error("should be number");
const shouldBeSymbol = genericFunc({ someKey: null });
if (typeof shouldBeSymbol !== "symbol") throw new Error("should be symbol");

const shouldBeError = genericFunc({ anotherKey: null });
if (!(shouldBeError instanceof Error)) throw new Error("should be Error");

//No logs.
```

## template-literal-type, 動的キーの型付け

ちょっと普通の静的型付け言語じゃありえないレベルの型付け機能。template-literal type.

そもそも、変更される可能性のないリテラルはリテラルの中身が見えたまま型として処理される

```ts
const str = "foobarbaz"; //変更されないので、"foobarbaz"として認識される
const dict = {
  foo: "foo",
  bar: "bar",
  baz: "baz",
} as const;
//こちらも変更されないので { foo: "foo", bar: "bar", baz: "baz" }
const readOnlyStrArray = ["foo", "bar", "baz"] as const;
//こちらも同様、変更されないので["foo", "bar", "baz"]と認識される。
```

ここで、このリテラル値を型システムとして操作可能にしてしまおうという狂った発想が表れた。
それが template literal type。

基本的な構文は template literal のそれと似たような感じで

```ts
type ExampleLiteral<T extends string> = `hi ${T}`;

type HiGuys = ExampleLiteral<"guys">;
//type HiGuys = "hi guys"
```

現在使える template literal に対する組み込みの Utility type は 4 種.

```ts
type Uppercase<StringType>
type Lowercase<StringType>
type Capitalize<StringType>
type Uncapitalize<StringType>
```

詳しい解説は[公式](https://www.typescriptlang.org/docs/handbook/2/template-literal-types.html)や[優秀な解説](https://zenn.dev/lollipop_onl/articles/bd59bf7f97dd305d3ed2)に譲るとして、以下の例を通じて、動的に生成されるプロパティ名に対する型を記述する例を示す。

[play it on playground](https://www.typescriptlang.org/play?#code/PTAEBUE8AcFMGcDGAnAltALqDNalQHYBmsysBieq8oBA9lvHaIgIbSsBGANnhgBassqLGwK0G+YqVAATSAVYBbVIlB1OAK1iIs0ZHTjIMqBKCHnu3AHQAoRHQLxRZIbAAiC5aoAKB6ADyWjpYALygABT6hgDSsJDwAFygzmgEAOYA2gC6AJSgoQB8oADetqAsjs7qWgWlAL4A3OWgRHTIkQ5OWADW8QByynh0RKDR0HEJ+WUVFV3V3HQA7qSTikp44X2QgxvWGHQAMsukAMKs8LARuc2zoEHautaysESEsH6GpDgRLXcamgANH9ZosVsg1kNMgAGbL7OgAVWgRnOl2uoAA1KAwat4utYNZ4NxVFcAIy5YF3CozKkVABurG4AFdYMl0UVQNtdrBKbTKsRUOkmcguLxkkRGZdebSlmgMKLWa1JTyQXdyEyNiKeIqMMgWdLZvUQTcWkaKmQMMLxADmk1bPYqlh5AQHnUULA3J51r5-A8QhFMgAiNp0QOA0CBzisZBhiNRgBegbyzQdTjovGsi3SEWdD2sADE6HRrjdQCBPkYcKAAOSFujVuR0Mz0LCwAAe1CwjmwuBrJXq1esEQATABmUcATlyqaYGazOYUeYAQtGS40y8AK99IDWV8gG7ImzQW6B2531OIcHA+wOh2PJ9P5umCfPc1prCv42uN1vjDvq5+B5HhIrYdtU3ZXng1b9oOI7jlO9ogKAnBMlgrC6EyjLcDuIigEs7Q9PAtggCAmSHAEADi2TJMGRaBsRJFkZR1FxtG9EkcATFUTRCb0QxoCHLAGDVjQkH4KI7RkLo2GIWAAjUPgx6SEwKS6qwgr8BgbRLNGsjWMhqHiSwrDiCenBUNaAgyDpkD6RgsBWKAGz4KMkB0EynL0Es6hWR0Nl2GJXoVgAskJ-B0LI8AADzgKebb2QQkWgK4h4ENhqlpFk2TFOENKZD4UigOc0AiIyqDxrAMWZAQGrmcg2UseyxTHOCbCXFFPiFLaKbzC4Hr2V63iIBWfq6AA6iI-CnJJIRQNe4QxXFCVJSljjpakhBZYUvwVOMkxJBAti5MkQX+KFAgRdF4A5cUNIWlaLCuANXgqMNvrBLoAbWN9e3xPAeTmDQp2GOd4WRTFXW2HaM5OouWjTcgUkYNhc2wLIbpPR4L0+oYo0YBNAgI0jqMBi0tGhrykZsZTvHAtkgP8s4Jozs+mZ0Nmb6aETIQo7gel1t+IBRP4275HWbL5By5P0U+c7swuLrwzN0mQKjel7muLNyxzcNc8ryOq3zH6sF+uQ3EAA)

```ts
// Typescriptの型推論は動的プロパティ名をまったく推論できない
const createDynamicPropObject = (propKeys: string[]) => {
  const obj = {};
  for (const keyName of propKeys) {
    const lowerKeyname = keyName.toLowerCase();
    Object.defineProperty(
      obj,
      lowerKeyname[0].toUpperCase() + lowerKeyname.slice(1),
      {
        value: () => keyName,
        configurable: false,
        writable: false,
        enumerable: true,
      }
    );
  }
  return obj;
};

const dynObj = createDynamicPropObject(["foo", "bar", "baz"]);

console.log(dynObj.Foo()); //Property 'Foo' does not exist on type '{}'.(2339)
console.log(dynObj.Bar()); //Property 'Bar' does not exist on type '{}'.(2339)
console.log(dynObj.Baz()); //Property 'Baz' does not exist on type '{}'.(2339)

// 実際には動作する
////[LOG]: "foo"
////[LOG]: "bar"
////[LOG]: "baz"

// Let's type it correctly

// readonlyであれば、literalとして操作できる
type DynPropMethods<T extends readonly string[]> = {
  [P in Capitalize<T[number]>]: () => Lowercase<P>;
};

const createDynamicPropObjectWithCorrectType = <T extends readonly string[]>(
  propKeys: T
): DynPropMethods<T> => {
  return createDynamicPropObject([...propKeys]) as DynPropMethods<T>;
};

const dynObjCorrectlyTyped = createDynamicPropObjectWithCorrectType([
  "foo",
  "bar",
  "baz",
] as const);

console.log(dynObjCorrectlyTyped.Foo()); //(property) Foo: () => "foo"
console.log(dynObjCorrectlyTyped.Bar());
console.log(dynObjCorrectlyTyped.Baz());
```

かのうせいはむげんだい！

動的なキーを持たせなければこのような使い方をすることはないが、不意に他社のコードに型をつける場合に上記のような型付けが必要になる。

一例として[winston(logger)](https://github.com/winstonjs/winston)は入力値に応じた動的なプロップの`is${logLevel}`というメソッドを持っており、template-literal を使わないと正しく定義できない。
しかも公式の`.d.ts`ファイルの実装が間違っていてこの動的メソッドが型上存在しないため、自前で型を定義し、その型を使うようにする必要がある。

## @ts comments

`//@ts-ignore`など、`@ts`で始まるコメントで typescript コンパイラを黙らせたりできる

- ドキュメントにこれらのコメントの項がない
- [リリースノートで触れられている](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-3-9.html#ts-ignore-or-ts-expect-error)
- [@ts-check は vscode のリファレンスで触れられている](https://code.visualstudio.com/docs/nodejs/working-with-javascript)

### @ts-ignore

- 次の行のコンピレーションエラーを黙らせられる。

### @ts-expect-error

- 次の行のコンピレーションエラーを黙らせられる。
  - 次の行にエラーが発生しない場合逆にエラーになる

### @ts-check

- `.js`ファイル先頭につけると型チェックが有効になる
  - 当然 [allowJs オプション](https://www.typescriptlang.org/tsconfig#allowJs)も必要になる

### @ts-nocheck

- `.js`ファイル先頭につけると型チェックが無効になる

## further reading: 型パズルなど参考になりそうな記事へのリンク

// under construction...

# プラクティス集

typescript をコーディングするときのプラクティス集。

typescript の域に収まらず、javascript の仕様にも踏み込んでいる。

## 複雑な型とテスト

やろうと思えば複雑な型を記述可能だが、これをテストしなくていいの？という話。
型記述はあくまでコード内部の挙動を説明したものでしかないため、ts-jest で typescript で書いたテストが正しく境界値テストを行えていれば問題ない。

jest 内で`@ts-ignore`や`@ts-expect-error`を使うときはしっかりと理由を述べ、レビュワーは型と食い違った挙動を許容していないかチェックしなければならない。

## 型は常に書く

コードは読まれる時間のほうが圧倒的に長い。

## Mutation しない

## DI する。interface を書く。継承しない。

## optional/nullish な number, string について truthy/falsy 評価しない

number の`0`は falsy だし、string の`""`(空 string)は falsy。
同様の理由で`||`(論理的 OR) も使ってはいけない。

代わりに`typeof foo === "undefined"`を使う。`||`(論理的 OR)の代わりに`??`([nullish coalescing operator](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Nullish_coalescing_operator))を使う

[play it on playground](https://www.typescriptlang.org/play?ssl=13&ssc=44&pln=14&pc=44#code/MYewdgzgLgBCAOUCW4CGAbAggJwOYDEBXMYGAXhgApU8BlKbAfgC4ZpskxcAaGG3AHKEAtixhgRAIwCm2AJTkAfDADeAKBgxQkEOmkA6dCFyUAhKf71svKAE940kADM+dBnIDcGreAi6DRibm-ELCNvaOLiEinmre2n56hsbUbtgwAD4ZMABEgUjAGHDYObyWDDCMjLkS6OhIEAAWPhjSEMCcuDmxmgn+ySbRwpnZecYFRSAlZXihldU5tfVNLXrtnd1eAL5eagjIaFh4RCSUAG4gSAAmMAAMvBfXd7EA9ABUagDaADIA8gDiAF1WE4MBBpLxcsQrtInJxpFccjAvn8gSCwRDNDlobD4YjkT8AcDcvlCuhiqUsUsGs1QK11lwkSiiawxrgJuSppSaoQ6jTVm0OozkW8XntECgwBgcARiMBKDlKbdXh9CWiYKD0ODITl2BsCajiZrtVSpLImWriWyORSdRbDazSZNpppbiKxftJdLjnKFU4QCBJDRKQBGABMAGYVcz1QxCJjcnrhTHiXGE4szSUDSzcv7A8GdXmg1mU6xwxHIeX3XFPYcZSd5TkiwWYKEZNh9AJUAJo5bWGmdUmutn1cb0xJhO37TmmwHi9zZ-mS32SeMybbNF2BO6gA)

```ts
const optionalArgFunc = (argStr?: string, argNum?: number) => {
  console.log(!!argStr, typeof argStr);
  console.log(!!argNum, typeof argNum);

  console.log(argStr || "logical or", argStr ?? "nullish coalescing");
  console.log(argNum || "logical or", argNum ?? "nullish coalescing");
};

optionalArgFunc(void 0, void 0);
/*
[LOG]: false,  "undefined" 
[LOG]: false,  "undefined" 
[LOG]: "logical or",  "nullish coalescing" 
[LOG]: "logical or",  "nullish coalescing" 
*/
optionalArgFunc("", 0);
/*
[LOG]: false,  "string" 
[LOG]: false,  "number" 
[LOG]: "logical or",  "" 
[LOG]: "logical or",  0 
*/
optionalArgFunc("foobar", 123);
/*
[LOG]: true,  "string" 
[LOG]: true,  "number" 
[LOG]: "foobar",  "foobar" 
[LOG]: 123,  123 
*/

optionalArgFunc("foobar", Number.NaN);
/*
[LOG]: true,  "string" 
[LOG]: false,  "number" 
[LOG]: "foobar",  "foobar" 
[LOG]: "logical or",  NaN 
*/
```

number 型は NaN の扱いもあるので面倒ですね

## optional な object のメンバに対して "key" in obj オペレータで判定しない

以下の二つは実際は違う挙動だが型上区別がつかない

- 存在しないキーへのアクセスが`undefined`を返す
- キーに`undefined`がセットされている

[playground](https://www.typescriptlang.org/play?#code/JYOwLgpgTgZghgYwgAgPIAczAPYjgGwAUpt0BnZAbwChllSwBlMKAfgC5kyXQBzAblr1MAOQCuAWw7IQkgEbRBdBgCFs2fNLnr8EOCEEBfatQS5uXOBPS6AgpwxZcBYqQoBeKocGnzYS9a6Kg6YOHhEJOTInjTKmMxQnABu2MAAJsgADAA0QgziEsmpGTl5mGoaRelZud4mZiBkGhAAdPjYvAAUAEQMCd3IoAE2ELYAlPzIAPRTANoAMqgA4gC6nPD4ZBC+jc1tHT19LANDZFYjKhPTc4urnCxiEEA)

```ts
interface OptionalProps {
  optStr?: string;
  optNum?: number;
  optBool?: boolean;
}

const sampleA: OptionalProps = {};

const sampleB: OptionalProps = {
  optStr: void 0,
  optNum: void 0,
  optBool: void 0,
};

console.log("optStr" in sampleA); //[LOG]: false
console.log("optStr" in sampleB); //[LOG]: true
```

ある意味意図的にセットされた`undefined`と何もセットしていない状態を切り分けることができるが、`null`を使う方がもっと明示的なのでそちらを使った方がいいだろう。

## optional な object のメンバに対して Object.hasOwnProperty で判定しない

同上

## Enum は使わない

https://zenn.dev/tak_iwamoto/articles/d367f989eb4a33#enum-%E3%81%AF%E4%BD%BF%E3%81%86%E3%81%B9%E3%81%8D%E3%81%A7%E3%81%AA%E3%81%84%E3%81%A8%E3%81%84%E3%81%86%E8%A9%B1

```ts
enum SampleEnum {
  enum1,
  enum2,
}
```

を js に変換すると

```js
"use strict";
var SampleEnum;
(function (SampleEnum) {
  SampleEnum[(SampleEnum["enum1"] = 0)] = "enum1";
  SampleEnum[(SampleEnum["enum2"] = 1)] = "enum2";
})(SampleEnum || (SampleEnum = {}));
```

になる。
これには tree-shake がうまいことかからないので使わない。

[index signature, mapped type](#index-signature-mapped-type)の項のサンプルで行ったようなことを代わりにする。

## private メンバには prefix underscore

[private class field](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes/Private_class_fields)が ES2022 で導入される。
これは、プロパティ名の前`#`をつけることを構文ルールとするもので、フィールド名を見ただけでプライベートかどうかを判別することができる。

javascript には長らく private などのアクセス修飾子が存在しなかったため、公開していないメンバーですよというのをアピールするために、`_`を先頭に着ける文化があった。これは python でも同様である。

そのコンベンションに乗っ取るならば、typescript 上での private メンバには`_`を先頭に着けるのがいいと思う。

# おまけ

## create-react-app like build script

## React/Typescript component sample

React で typescript 使うときのサンプル

children prop の型をはっきり指定できるようになるのでいい感じ。

ref など作成する場合初期値与えなかったら自動的に`MutableRefObject<HTMLInputElement> | undefined`のように推論してくれるので`as MutableRefObject<T>`で黙らせた方がコード書くのは楽。

以下に tagsInput みたいなのを作った時のサンプル。こんな感じで tsx は書いていく。作りかけですが・・・

```tsx
import {
  MouseEventHandler,
  ReactNode,
  MutableRefObject,
  useRef,
  useState,
  useCallback,
  useEffect,
} from "react";
import clsx from "clsx";

import { createStyles, makeStyles, DefaultTheme } from "@material-ui/styles";

import { Tag } from "./tag";

const inputFocusOutKey = ["Enter", "Escape"];

const useStyles = makeStyles<DefaultTheme, TagInputProps>((theme) => {
  return createStyles({
    container: {
      background: "linear-gradient(45deg, #FE6B8B 30%, #FF8E53 90%)",
      border: 0,
      borderRadius: 3,
      boxShadow: "0 3px 5px 2px rgba(255, 105, 135, .3)",
      color: "white",
      width: "100%",
      padding: "0 30px",
      display: "flex",
    },
    tagFieldCOntainer: {
      height: "100%",
      flexGrow: 1,
    },
    displayNone: {
      display: "none",
    },
  });
});

export interface TagInputProps {
  defaultTags?: string[];
  placeholder?: string;
  buttonDisplay?: ReactNode;
  classNames?: {
    container?: string;
    input?: string;
  };
}
export const TagInput = (props: TagInputProps) => {
  // class names
  const classes = useStyles(props);
  // states
  const [tags, setTags] = useState(props.defaultTags ?? ([] as string[]));
  const [inputValue, setInputValue] = useState("");
  const [inputMode, setInputMode] = useState(false);
  // ref
  const inputRef =
    useRef<HTMLInputElement>() as MutableRefObject<HTMLInputElement>;
  const inputValueRef = useRef("");
  // callbacks
  const handleContainerClick: MouseEventHandler<HTMLDivElement> = useCallback(
    (/*e: MouseEvent<HTMLDivElement>*/) => {
      setInputMode(true);
    },
    []
  );
  const handleInputChange = useCallback(
    (e: React.ChangeEvent<HTMLInputElement>) => {
      setInputValue(e.target.value);
    },
    []
  );
  const handleTagDeleteButtonClick = useCallback((tag: string) => {
    setTags((prev) => {
      const newTags = [...prev].filter((prevTag) => prevTag != tag);
      setInputValue(newTags.join(" "));
      return newTags;
    });
  }, []);
  const handleInputBlur = useCallback(() => {
    const tags =
      inputValueRef.current === ""
        ? []
        : inputValueRef.current
            .split(/[\s\t]/)
            .filter((fragment) => !/[\s\t]/.test(fragment));
    setTags(tags);
    setInputMode(false);
  }, []);
  const handleFocusOut = useCallback((e) => {
    if (inputFocusOutKey.includes(e.key)) inputRef.current?.blur();
  }, []);

  // effects
  useEffect(() => {
    if (inputMode) inputRef.current?.focus();
  });
  useEffect(() => {
    inputValueRef.current = inputValue;
  }, [inputValue]);

  return (
    <div
      className={clsx(classes.container, props.classNames?.container)}
      onClick={handleContainerClick}
    >
      <input
        ref={inputRef}
        className={clsx(classes.tagFieldCOntainer, {
          [classes.displayNone]: !inputMode,
        })}
        type="text"
        placeholder={props.placeholder}
        value={inputValue}
        onChange={handleInputChange}
        onBlur={handleInputBlur}
        onKeyDown={handleFocusOut}
      />

      <div
        className={clsx(classes.tagFieldCOntainer, {
          [classes.displayNone]: inputMode,
        })}
      >
        {tags.map((tag) => (
          <Tag onDeleteButtonClick={handleTagDeleteButtonClick}>{tag}</Tag>
        ))}
      </div>

      <button>{props.buttonDisplay ?? "button"}</button>
    </div>
  );
};
```
