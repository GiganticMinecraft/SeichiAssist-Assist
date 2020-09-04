# モナド

この項目は型クラスに関するScala研修テキストを読んでいることを前提とします。

## 高階型

モナドの話をする前に、まず**高階型**を紹介します。

これまで見た例では、`Int` や `Any` 等の他に、別の型が適用された形で現れる型がありました。 `Function1[-T1, +R]` や `List[E]` などがこれにあたります。

ここで、 `Function1` のみを考えると、これは「型を二つ取ってはじめて具体的な型となるもの」です。同様に `List` は「型を一つ取ってはじめて具体的な型となるもの」になります。

`Int`や`Any`のように、変数が持ちうる型のことを**プロパーな型**と呼びます。反対に、`List` ように「型をいくつか取ってはじめてプロパーな型となる」ようなものを**高階型**(HKT; **H**igher **K**inded **T**ype)と呼びます。

型が高階型であるかどうかは、型宣言をする際に重要になる場合があります。

例えば、 `List[E]` や `Vector[E]` は `map[R](f: E => R): List[R]` といった `map` メソッドを持ちますが、「とある型に `map` を実装できる」という性質を抜き出す型クラスを考えてみます。

すると(以下のコードは実行できません！)、

```
trait Mappable[Collection] {
  def map[E, R](collection: Collection[E], f: E => R): Collection[R]
}
```

のような `trait` を用意することになります。

しかし、 `Collection[E]` や `Collection[R]` と書くと、プロパーな型である `Collection` に `E` を渡すことになってしまい、意図が伝えられていません。正しくは、次のように書きます。

```Scala
// [_]を付けることで、 Collectionが別の型を一つ受け取ることを言える
trait Mappable[Collection[_]] {
  def map[E, R](collection: Collection[E], f: E => R): Collection[R]
}
```

この時、型引数である `Collection` は**高階である**と言えます。

## モナドとは何か - FlatMapの力

モナドは、少し抽象的になりますが「計算処理の依存関係を扱うための `trait`」です。具体例無しでは分かりづらいため、これから具体例をいくつか見ることにしましょう。

### Option?

TODO

### List?

TODO

### IO?

TODO

## モナド`trait`の定義

モナドは、次のように定義される `trait` です。

```Scala
trait Monad[F[_]] {
  def pure[A](value: A): F[A]

  def flatMap[A, B](fa: F[A], function: A => F[B]): F[B]

  // その他の便利メソッド
  def map[A, B](fa: F[A], function: A => B): F[B] = flatMap(fa, a => pure(function(a)))

  // ...
}
```
