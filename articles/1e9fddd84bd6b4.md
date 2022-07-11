---
title: "React Docs BETAを読む 【Contextでデータを深くに渡す】"
emoji: "🐡"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["react","tech","frontend"]
published: true
---
[React Docs BETAを読む 【状態の管理】](https://zenn.dev/ryochang122/articles/1f97a79373c892)
[React Docs BETAを読む 【Stateで入力に反応する】](https://zenn.dev/ryochang122/articles/4d71076608ceba)
[React Docs BETAを読む 【状態構造の選択】](https://zenn.dev/ryochang122/articles/e26f9d37227579)
[React Docs BETAを読む 【コンポーネント間で状態を共有する】](https://zenn.dev/ryochang122/articles/79da51c125f0bf)
[React Docs BETAを読む 【状態の保持とリセット】](https://zenn.dev/ryochang122/articles/946db271367c1e)
[React Docs BETAを読む 【状態のロジックをReducerに抽出する】](https://zenn.dev/ryochang122/articles/f0a22530473423)
に引き続き状態管理の章を読み進めていきます。

# Passing Data Deeply with Context
今回はPassing Data Deeply with Contextということで、Contextについての章ですね。
Reactは基本的にpropsを通じて情報の受け渡しをするので、バケツリレーのような状態になることもあります。
経由するコンポーネントやその情報を必要とするコンポーネントが多い場合はContextを使用することで、ツリー内に共有することができます。

## The problem with passing props
propsは明示的に情報をパイプするのに良い手段なんですが、UIツリーのネストが深くなった状態では冗長になりがちです。
また状態を引き上げる際に近接する共通の親が遠くなってしまうなどの弊害があります。
Docsではその状態をProp掘削と表現されていてなんだかおもしろいです。

## Context: an alternative to passing props
Contextを使用すると親コンポーネント下のツリー全体にデータを提供することができます。
Contextを使用するには以下の3Stepを踏むと記載があります。
1. Contextを作成する。
2. データを必要とするコンポーネントからContextを使用する。
3. データを指定するコンポーネントからContextを提供する。

### 1. Create the context
まずはContextを作成するところからです。
まずは```createContext```を使用してContextを作成し、exportします。
Docsの例だと以下
```jsx
import { createContext } from 'react';

export const LevelContext = createContext(1);
```
ここで渡している1はデフォルト値ですね。
数値以外にもオブジェクトを含む任意の値を渡すことができます。

### 2. Use the context
propsで値を受け取っている部分をContextに置き換えます。
```jsx
// Before
export default function Heading({ level, children }) {
  // ...
}

//After
export default function Heading({ children }) {
    const level = useContext(LevelContext);
    // ...
}
```
Contextを使用していますが、提供するコンポーネントがない場合はReactは指定したデフォルト値を使用します。

### 3. Provide the context
ContextのProviderを使用して、Contextを使用するコンポーネントをWrapします。
コンポーネントは最も近接するProviderの値を使用します。

## Using and providing context from the same component
Providerで使用する値にContextの値を使用する事ができます。
Docsの例ではネストされたオブジェクトをネストごとにインクリメントするために使用しています。

## Context passes through intermediate components
Contextを提供するコンポーネントとそれを使用するコンポーネントの間には任意の数のコンポーネントを挿入できます。
> Context lets you write components that “adapt to their surroundings” and display themselves differently depending on where (or, in other words, in which context) they are being rendered.

とDocsにもある通りContextを使用することで、周囲と適応するコンポーネントを書くことができます。
記述する場所によって異なる表示を行うコンポーネントが作成できるということですね。
あとはContextは完全に分離されているので、任意の数のContextを同時に使用してもそれぞれに影響はありません。

## Before you use context
深くネストしているからといってContextを使用する必要があるわけではありません。
まずは以下から始めましょう。
1. propsで渡すことから始める。
2. コンポーネントを抽出して、JSXを子としてそれらに渡します。
childrenにコンポーネントを書いてそこに渡してあげれば不要な中間レイヤーを一つスキップできます。

上記アプローチがうまくいかない場合にContextを検討するのが良いようです。
まずはPropsや通常のネストなどで対応できないか考えるということですかね。

## Use cases for context
Contextのユースケースは以下のような場合です。
* テーマ
* カレントのアカウント情報
* ルーティング
* 状態管理

気をつけないといけないのは、Contextは静的な値に限定されないので、レンダリングで別の値で更新されると使用するコンポーネントを全て更新します。

# まとめ
状態管理の章も終盤に差し掛かってきました。
今回はContextの話で、参画しているプロジェクトだとそのあたりは全てReduxに吸収されていて見かけないHooksの一つです。
ただドキュメントを見る限り全然使える場面も多くあると感じました。
特に状態管理やカレントの情報などロジックを分離するためにはContextにしたほうが良いものもあるきがします。
少し使ってみて問題なければ、取り込んでいきたいですね。



