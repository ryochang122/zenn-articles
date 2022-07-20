---
title: "React Docs BETAを読む 【Manipulating the DOM with Refs】"
emoji: "🐷"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["react","frontend"]
published: true
---

Reactを使っていると、DOM要素にアクセスする必要がある場合があります。
僕の最近作ったものではツールチップなどでDOM要素にアクセスする必要がありました。
他にもフォーカスを当てたりスクロールやサイズの測定などを行う場合などもDOM要素にアクセスする必要があります。
今回の章はそんな時に、Refを使用してDOMを操作することについてです。

## Getting a ref to the node
ReactでDOMノードにアクセスするためには```useRef```を使用します。
```jsx
const myRef = useRef(null);
<div ref={myRef}/>
```
[前の章](https://beta.reactjs.org/learn/referencing-values-with-refs)でもあった通りuseRefは単一のcurrentを持つオブジェクトを返します。
上記のdivに渡しているrefの場合は,DOMノードを作成するときにcurrentにノードへの参照がセットされます。
イベントハンドラからこのオブジェクトからブラウザAPIにアクセスできます。

## Accessing another component’s DOM nodes
独自のコンポーネントにrefを渡した場合はデフォルトでnullが取得されます。
これ自体はReactの仕組みとして他コンポーネントのDOMノードにアクセスできないからです。
Ref自体は限られた場合するのが望ましいため、他コンポーネントのDOMノードを操作するとコードが壊れやすくなるので注意です。
代わりに、DOMノードを公開するコンポーネントはオプトインする必要があります。
参照をそのコンポーネント内の子に一つ渡してあげるという感じです。
```jsx
const MyInput = forwardRef((props, ref) => {
  return <input {...props} ref={ref} />;
});
```
デザインシステムでは、低レベルのコンポーネントでは上記のような参照の転送が一般的のようです。
一方で、フォームやリストなどの高レベルコンポーネントはDOM構造への依存を避けないと通常は公開しません。

## When React attaches the refs
Reactのすべての更新が2つのフェーズに分割されます。
* レンダリング中にReactはコンポーネントを呼び出して、画面に表示されるべきものを把握します。
* コミット中に、Reactは変更をDOMに適用します。

基本的に、レンダリング中に参照にアクセスすることは望ましくありません。
初期値にnullが入っている状態でレンダリング中に参照すると読み取りには早すぎる場合があります。

## Best practices for DOM manipulation with refs
Refはあくまで非常手段なので、ブラウザAPIなどのReact外の動作が必要な場合に使用します。
非破壊的操作の場合は問題ないですが、DOMの変更の場合はReactの動作とコンフリクトを起こす危険性があります。
Reactの管理外でDOMの変更を行うとReactが追跡できずにクラッシュする場合などがあります。
なのでDOM操作はあくまでJSX上で固定になっているなどのReactの管理対象外のものにするべきです。

## まとめ
refを使用したDOM操作についての章でした。
ツールチップの実装時にこの辺りの機能をつかっていたんですが、いまいち腹落ちしてない部分がありました。
タグにRefを渡す構文自体など、基本的な部分を掘り下げずに使用していたので、今回の章でそのあたりがスッキリしました。
