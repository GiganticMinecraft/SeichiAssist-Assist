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

モナドは、少し抽象的になりますが「計算処理の依存関係を扱うためのオブジェクト」です。具体例無しでは分かりづらいため、これから具体例をいくつか見ることにしましょう。

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
  def map[A, B](fa: F[A], function: A => B): F[B] = flatMap(fa, (a: A) => pure(function(a)))

  // ...
}
```

型引数に現れる `F` は、「文脈」などと呼ばれます。`F[A]`は、何らかの「評価」を行うことで `A` の値を「計算結果」として出してくる、プログラムのような値の型です。

`Monad[F]`は、`F`による計算の実行順序を制御するためのオブジェクトです。`pure` は「何もせずに、渡した`A`の値をすぐに返す」ようなプログラムを定義します。`flatMap` は「`fa`を実行し、その結果を `function` に入れることで得た`F[B]`を実行する」プログラムを定義します。

### IOモナド - プログラムそのもの

`IO` は「値付きプログラム」そのものでした。実際、 `Monad[IO]` を定義することはすぐにできます。

```Scala
object MonadIO extends Monad[IO] {
  def pure[A](value: A): F[A] = IO.pure(value)

  def flatMap[A, B](fa: F[A], function: A => F[B]): F[B] = fa.flatMap(function)
}
```

では、 `Option` や `List` を文脈とする `Monad` を作れるでしょうか？

### Optionモナド - 途中終了するかもしれない計算

### Listモナド - 複数の計算を全部試す計算

## なぜモナドを使うのか

TODO
