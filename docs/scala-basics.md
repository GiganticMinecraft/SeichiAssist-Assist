# Scalaの基本

## はじめに

SeichiAssistでは、プログラムのコードを書くために[Scala](https://docs.scala-lang.org/ja/)を使っています。本項では、SeichiAssistで主に使われるScalaの機能等を紹介します。

### 読み方

次のように表示されるコードのひと塊を「コードスニペット(コードの断片)」と呼びます。

```Scala
val variable = 42
```

これらは特に明記が無い限りScalaで書かれたコードを含みます。

また、次のように色が一切付いていないコードは、何らかのエラーにより実行ができません。「誤った例」を示すスニペットです。

```
// エラー: val が vl になってしまっている
vl variable = 42
```

Scalaのコードスニペットは、すべて[Scastie](https://scastie.scala-lang.org/)というオンライン実行環境で試せるものになります。コードを貼り付けてSaveボタンを押せば実行できます。少し変えて動かしてみて、予想通りの結果になるか試してみるのも良いでしょう。

`abc123` のようにフォントが切り替わっている箇所は、コードスニペットにするには短いコードの一部分のみを切り取ってきたことを表します。

「注: 」と書かれている箇所は読むことが推奨されている注釈です。

「補足: 」と書かれている箇所は、疑問に思われるかもしれない箇所や、正確性に欠ける記述を補足するものです。よくわからない場合は飛ばしても構いません。

## 値、変数、型

### 値

Scalaでプログラムを書くにあたって最も基本的なものとも言える存在が「値」です。値を厳密に定義するのはここでは難しいですが、以下のようなものが含まれます：

 - 「整数」値
   - `0`, `-10`, `42`
 - 「文字列」値
   - `"abc"`, `"あいうえお"`, `"Scala"`
 - 「真偽」値
   - `true`, `false`
     - 注: `true` と `false` は、それぞれ何かの条件が「満たされている」、「満たされていない」の意味を持ちます。詳しくはコレクションの節にて説明します。
 - 「小数部分を持つ数」値
   - `0.3`, `-1.5`
     - 補足: 実は、コンピュータの内部的な事情によりこれらの値は私たちが思ったように正確に表されているとは限りません。 `0.1 + 0.2` はしばしば `0.3` とは違う値を返してきます！詳しくは、「浮動小数点数　誤差」などと検索してみてください。
 - 「時刻」値
   - 2020年9月3日14時15分
     - 注: これをこのままプログラムに書いても動きません！プログラムは、この特定の時刻を指し示す、より具体的な値(例えば1970年1月1日 00:00:00 UTCからの経過秒数など)を扱います。

### 変数

Scalaでは `1 + 3`(加算), `2 * 21`(掛け算)のように整数値の計算をすることができます。 

計算をした結果を後で別の計算や処理に使いたいという状況はよく発生するのですが、Scalaでは「変数」と呼ばれる仕組みでこれを実現できます。変数を作成する(補足: "宣言する"と言います)には、 `val ` の後に「変数の名前」を書き、変数が持っておいて欲しい結果を ` = ` で繋ぎます。

```Scala
val firstValue = 3
val secondValue = firstValue * firstValue + 1
```

これで、 `secondValue` は `10` を持つことになります。Scastieは式の値を表示してくれるので、

```Scala
val firstValue = 3
val secondValue = firstValue * firstValue + 1

secondValue
```

と入力すると、 `secondValue` の隣に `10: Int` という表示が出ます。

### 型

さて、上で挙げた値達は、何らかの「種類」のようなものに分けられるように見えます。「値、変数や式がどんな種類であると分かっているか」の情報そのものが「型」と呼ばれるものです。

例えば`0`は`Int`(注: integer; 整数)という型であることがすぐに分かりますし、`"abc"`は`String`という型になります。Scalaには`Int`のように既に用意された型もあれば、自分で型を作ることもできます。

変数の型は明示的に書くことができます。

```Scala
val integerValue: Int = 42
```

型は、予期しない値がプログラムのとある箇所から別の箇所へ渡されてしまうことを防ぐ役割があります。以下のコードは、「`integerExpected`は`Int`であることが期待されているが、`String`である`"42"`が入ろうとしている」という理由で実行できません(Scastieに入れてみると、「型エラー」が出ることでしょう)

```
// 型エラー: "42" は Int ではない
val integerExpected: Int = "42"
```

Scalaの特別な型として `Any` 型というものがあります。これは「どのような種類の値になるか一切わからない」ことを示す型であり、何らかの複雑な理由で適切な種類分けができない時には `Any` が書かれます。

```Scala
// Stringという、より情報量がある型を付けるのが便利だが、
// 「何が入ってるかわからない」と「しらを切る」こともできる
val lessKnownValue: Any = "42"
```

補足: ここで言う型は「**コードを見るだけで分かる**、値、変数や式がどのような種類であるかの情報」であり、プログラムが実行されるより遥か前に分かっているものです。コードがコンピュータによって実行しやすい形式にすることを「コンパイルする」、コンパイルするプログラムのことを「コンパイラ」と呼びます。コンパイラは式の型を算出し、変数に本当にその式の値を入れるのが**安全かどうか**を検査します。この仕組みは __型システム__ と呼ばれ、プログラムを走らせたときに値にくっつけられる「値がどのような種類であるか」という情報である「型タグ」とは区別されます。例えば、Scastieで表示された `secondValue [10: Int]` のような表示は「型タグ」によるものです。

## オブジェクト

Scalaの値には「オブジェクト」というものがあります。オブジェクトは複数の値を一つの値として束ね持ったものです。例えば、先程の「時刻」などは、オブジェクトで表現されています。

オブジェクトは、`new` キーワードにより作成でき、束ねる値として「プロパティ」を持つことができます。

`new` で作っただけのオブジェクトを変数に入れようとする時、何もしないならば型に `Any` を指定するしかありません。例えば、次のコードは動きません。

```
// 新しいオブジェクトを作成し、myObjectに持たせる
val myObject: Any = new MyObjectType {
  val integerProperty = 123
  
  val stringProperty = "abc"
}

// エラー：AnyにはintegerPropertyが無い
// Scalaは、myObjectがどんな形をしているのか何もわからない
myObject.integerProperty
```

これだとプロパティにアクセスすることができないため、先に `trait` で型を定義する必要があります。オブジェクトがとある `trait` の型を持つことは明示的に宣言する必要があり、 `new` の直後にtrait名を書きます。

次のコードはオブジェクトの型を定義し、定義した型を持つオブジェクトを `new` で作成し `myObject` に持たせ、最後に `integerProperty` を `myObject.integerProperty` と取り出しています。 `//` から始まる箇所はコード上無視される「コメント」です。

```Scala
// 型を定義する
trait MyObjectType {
  val integerProperty: Int
  
  val stringProperty: String
}

// 新しいオブジェクトを作成し、myObjectに持たせる
val myObject: MyObjectType = new MyObjectType {
  val integerProperty = 123
  
  val stringProperty = "abc"
}

// プロパティの値を取り出す
myObject.integerProperty
```

オブジェクトは、プロパティの他に「メソッド」と呼ばれる、プロパティ等を加工する計算をあわせ持つことができます。メソッドは「引数(ひきすう)」と呼ばれる入力を受け取ることができます。 `def` の後にメソッド名、続いて引数のリスト、計算結果の型と書くことで定義できます。例えば、次の `trait` はプロパティ`integerProperty`を一つ、プロパティへ乗算するメソッドを一つ持ちます。

```Scala
trait ObjectWithAMethod {
  val integerProperty: Int

  // メソッド定義
  def multiplyPropertyBy(x: Int): Int = integerProperty * x
}
```

メソッドを「呼び出し」て計算をさせたい時は、プロパティを取り出す際のように `.` にメソッド名を繋げ、渡したい引数を括弧の中に書きます。

```Scala
trait ObjectWithAMethod {
  val integerProperty: Int

  def multiplyPropertyBy(x: Int): Int = integerProperty * x
}

val myObjectWithAMethod = new ObjectWithAMethod {
    val integerProperty: Int = 21

    // multiplyPropertyByは再度ここに書く必要はない
}

myObjectWithAMethod.multiplyPropertyBy(2)
// -> 42: Int
```

## 関数、`Function` trait、関数オブジェクト

<!-- TODO「対応させる」は一般的な言い回しではない -->
関数(英: function)とは値を別の値へ対応させるものを指す抽象的な概念です。例えば、数学で使われる関数 _succ_ は、自然数に +1 した別の自然数を対応付け、 2 = _succ_(1) のような式を成り立たせます。

関数は受け取れる値の型(引数型)、値を渡した(注: 値を渡すことを適用; applyとも言います)結果出てくる値の型(結果型)、値の具体的な対応付けの三つの情報からなります。

Scalaでは関数を扱うための型として、 `apply` メソッドを持つ `Function1[T1, R]` という `trait` が[用意されて](https://www.scala-lang.org/api/current/scala/Function1.html)います(宣言に書かれた `-T1` や `+R` の`+, -`の意味は多相性の項目で説明しますが、とりあえず`+`と`-`は無視できます)。 `T1` の部分が引数型、 `R` の部分が結果(**R**esult)型になります。

次のコードは実際に `Function1[Int, Int]` の値を作成します。

```Scala
val intSquareFunction: Function1[Int, Int] = new Function1[Int, Int] {
  def apply(x: Int): Int = x * x
}

intSquareFunction.apply(3) // -> 9: Int
```

### 糖衣構文; syntax sugar

先程のコードは関数を表すオブジェクトを定義して関数へ数値を適用するだけであるのに、コードとしてそこそこ長いものになってしまいました。

先程と「全く同じ内容」を短く書くために、Scalaはそれ専用の構文を用意しています。

```Scala
val intSquareFunction: Int => Int = (x => x * x)

intSquareFunction(3) // -> 9: Int
```

まず、 `Int => Int` は `Function1[Int, Int]` と同義です。

`(x => x * x)` は「ラムダ式」と呼ばれる言語機能を使用しています。 `Function1` のように、各オブジェクトが定義しなければならないメソッドが一つしかない `trait` を SAM (**S**ingle **A**bstract **M**ethod; 単一抽象メソッド) trait  と呼びますが、SAM trait の値を短く記述するためにラムダ式が使えます。

```Scala
trait LinearMultiplication {
  def multiplyBySomeFactor(x: Int): Int
}

val f: LinearMultiplication = (x => x * 3)

f.multiplyBySomeFactor(-1) // -3: Int
f.multiplyBySomeFactor(4)  // 12: Int
```

最後の行では `intSquareFunction.apply(3)` が `intSquareFunction(3)` になりましたが、これはScalaが `apply` メソッドを特別に扱う機能を利用しています。`intSquareFunction(3)` のように、「オブジェクトそのものを関数のように呼び出す」コードは、そのオブジェクトの `apply` メソッドを呼び出す意図であると解釈されます。

このように、そのまま書くと見づらくなったりするコードを短く簡潔に書くための構文を「糖衣構文(英: syntax sugar)」と呼びます。

## 部分型と `extends`

> 「値、変数や式がどんな種類であると分かっているか」の情報そのものが「型」と呼ばれる

「型」の項目ではこのように型を説明し、`Int` や `String` に対して、 `Any` が「何もわかっていない」ことを指し示す型であると説明しました。つまり、「変数 `x` が `Int` であることを知っている」ならば「変数 `x` が `Any` であることを知っている」と言えそうです。

このような時、 `Int` は `Any` の**部分型である**と言うことにします。逆に、`Any` は `Int` の部分型ではありません。ある値が `Any` だからと言って、それが `Int` とは限らないからです。

Scalaの部分型関係には様々なものがありますが、代表的なものが、 `trait A` が別の `trait B` の部分型になっているものです。これは、 `trait A` を宣言する時に `trait A extends B` のように書くことで実現できます。

次の例では、`Int` に対して `Int` を返す関数の型 `Function1[Int, Int]` の部分型として、入力に比例する出力を返す関数である `LinearIntFunction` を定義しています。

```Scala
trait LinearIntFunction extends Function1[Int, Int] {
  // 比例係数
  val coefficient: Int

  override def apply(x: Int): Int = coefficient * x
}

// LinearIntFunction は Function1[Int, Int] の部分型なので、
// myLinearFunction: Function1[Int, Int] = ...
// と書いても大丈夫
val myLinearFunction: LinearIntFunction = new LinearIntFunction {
  val coefficient: Int = 3

  // Function1[Int, Int] が実装すべき apply は LinearIntFunction がすでに定義しているため、
  // 新しくオブジェクトを作る際には書かなくて良い
}

myLinearFunction(0)  // -> 0
myLinearFunction(3)  // -> 9
myLinearFunction(-5) // -> -15
```

上のコードでの `Function1[Int, Int]` を `LinearIntFunction` の「親trait」と呼びます。対して、`LinearIntFunction` を「子trait」と呼びます。

子traitが `extends` を指定したとき、親traitのメソッドを実装しておくことができます。上の例では、 `LinearIntFunction` は `Function1[Int, Int]` の `apply` メソッドを実装しています。これにより、新しく `LinearIntFunction` のオブジェクトを定義するときには `coefficient` を指定するだけで良くなります。

`override` は、親traitのメソッドを子traitの時点で実装してしまう意図を伝えるためのものです。ですから、親traitに宣言されていないメソッドを `override` しようとするとエラーになります。例えば、以下のコードはエラーが出て実行させてくれません。

```
trait LinearIntFunction extends Function1[Int, Int] {
  // 比例係数
  val coefficient: Int

  // エラー: applyのlが抜けている！
  override def appy(x: Int): Int = coefficient * x
}
```

## コレクション

複数の(型が同じ)値を大量に扱いたいことがあります。マインクラフトの例で言えば、オンラインのプレーヤー全員に対して同じことをしたいであるとか、幾つかのブロックに対して同じことをしたい、などでしょうか。

「オンラインのプレーヤー全員」や「幾つかのブロック」のように、**ものの集まりを表す**値を**コレクション**と呼びます。コレクションの中に入っているものを、そのコレクションの**要素**と呼びます。

Scalaは

 - `Vector[E]` など、「一列に並んだ」データ
 - `Map[K, V]` など、「KからVへの対応を表す」データ
 - `Set[E]` など、「並び順を気にしない集まりである」データ

などを標準で提供しています。これらは幾つかのtraitを親に持っていて、それぞれのtraitに様々なメソッドが定義されています。

### Vector

「一列に並んだデータ」で万能である `Vector[E]` の例を見てみましょう。`E` の部分には、 `Function1` で見たように具体的な型を入れます。 `Vector` の値を作るためには、 `Vector` に続き、オブジェクトが持つべきデータをカンマ(`,`)区切りで括弧の中に書きます。

```Scala
val myCollection: Vector[Int] = Vector(1, 2, 3, 4)

myCollection // Vector(1, 2, 3, 4): Vector[Int]
```

`Vector` は、要素すべてを「一律に」変換することができます。

```Scala
val myCollection: Vector[Int] = Vector(1, 2, 3, 4)

myCollection // Vector(1, 2, 3, 4): Vector[Int]

myCollection.map(n => n * 2) // Vector(2, 4, 6, 8): Vector[Int]
myCollection.map(n => n * n) // Vector(1, 4, 9, 16): Vector[Int]
```

### Map

`Map[K, V]` は、 `K` の値に `V` の値を対応させるようなコレクションです。例を見てみましょう。

```Scala
val myMap: Map[Int, String] = Map(
  (0, "zero"),
  (1, "one"),
  (2, "two")
)

myMap(0) // zero: String
myMap(1) // one : String
myMap(2) // two : String
```

注: 上の例で、`myMap(3)` のように対応を書いていない `Int` から `String` を持ってこようとするとエラーとなります。

### Setと性能特性

`Set[E]` は値の集まりではあるものの、`Vector` のように順序を持っていません。 `Set[E]` は `Vector` のように `.map` メソッドを持ち、 `.contains` メソッドによって値が入っているか入っていないかを調べることができます。

`.contains` メソッドは、 **真偽値** である `Boolean` を返してきます。例えば `.contains(1)` が `true` であるならば `Set` が `1` を含んでおり、 `.contains(1)` が `false` であるならば `Set` は `1` を含んでいません。

```Scala
val myIntSet: Set[Int] = Set(1, 3, 5)

myIntSet.map(n => n * 2) // Set(2, 6, 10): Set[Int]
myIntSet.map(n => -n)    // Set(-1, -3, -5): Set[Int]

myIntSet.contains(3)  // true: Boolean
myIntSet.contains(-1) // false: Boolean
```

さて、上のコードで `Set` を `Vector` に変えてみてください。`.map` した結果が今度は `Vector` になりますが、`Vector` も `.contains` メソッドを持っていますから、結果は同じになります。

なぜわざわざ `Set` を使うのかと言えば、「`Set` の方が `.contains` が速い」からです。`Vector` の `.contains` は、最初から最後まで要素を一つずつ見ていって、探しているものが見つかれば `true` を返し、最後まで探しても見つからなければ `false` を返すようになっています。これでは、コレクションのサイズが大きくなればなるほど「最後まで見ていく」作業は遅くなることでしょう。 対して、 `Set` は(補足: ハッシュテーブルなどの)巧妙な仕組みを利用することにより、コレクションのサイズがとても大きくなっても殆ど一瞬で `.contains` が答えを出してくれるようになっています。

ですから、`.contains` を何万回何億回と呼ばないとならないような場合では、 `Vector` などではなく `Set` を使えば処理に掛かる時間が大幅に減ることでしょう。

このように、同じ操作をする時にもコレクションにより処理時間が異なることがあります。とあるコレクションのサイズが大きくなった時、どのくらい操作に時間が掛かるようになるか、という指標を**性能特性**と呼びます。Scalaのコレクションの性能特性を詳しく知りたい場合、[公式ページの表](https://docs.scala-lang.org/ja/overviews/collections/performance-characteristics.html)を見てみると良いでしょう。参考までに、 `Vector` の `.contains` のような時間の増え方を「線形」、 `Set` の `.contains` のように掛かる時間が増えない場合を「定数」と呼びます。

## `case class`

この節と次節(`sealed`と直和)においては、Scalaの公式ドキュメントの次の2つのページも参考になるかもしれません。

 - [ケースクラス](https://docs.scala-lang.org/ja/tour/case-classes.html)
 - [パターンマッチング](https://docs.scala-lang.org/ja/tour/pattern-matching.html)

ここまでは、 `trait` により型を定義して、その型に合致するオブジェクトを `new` していました。値を入れてオブジェクトを作るだけなのに数行使うことになり、プロパティ名も毎回書かなければオブジェクトを作成できませんでした。

ただ単に値が入ったようなオブジェクトを作成するのは、 `case class` を使えば少し簡単に実現できます。 `case class` は `trait` のように型を定義するキーワードです。例えば、三次元空間の座標を `Int` の三つ組で表すオブジェクトの型 `ThreeDimensionalPoint` は、

```Scala
case class ThreeDimensionalPoint(x: Int, y: Int, z: Int)
```

のように宣言できます。ここで、 `x`、 `y`、 `z` は `ThreeDimensionalPoint` であるオブジェクトのプロパティになります。`x`などはこの `case class` のオブジェクトを作る際に渡すことになるため、**パラメータ**とも呼ばれます。`case class`の定義で、パラメータが並んでいる箇所を**パラメータリスト**と呼びます。そして、このように定義された `case class` のオブジェクトは、

```Scala
case class ThreeDimensionalPoint(x: Int, y: Int, z: Int)

val point = ThreeDimensionalPoint(1, 0, -3)

point.x // 1 : Int
point.y // 0 : Int
point.z // -3: Int
```

のように、コレクションを作成したときと同じようにオブジェクトが持つべき値を渡します。ここで値を渡す順番は、 `case class` のパラメータリストと同じ順番になります(例えば上の例では、`0`は`y`に対応します)。

## `sealed`と直和

TOOD: sealedで部分型階層を制限できること、case classでパターンマッチができることを書く

## 多相性、多相的な定義

TODO: コレクションの例に触れ、すべての型に対して動く定義について触れる
TODO: 多相関数がいかに制限的かについて触れ、シグネチャから挙動が予測できることを書く
