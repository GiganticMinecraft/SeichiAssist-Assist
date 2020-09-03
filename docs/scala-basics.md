# Scalaの基本

## はじめに

SeichiAssistでは、プログラムのコードを書くために[Scala](https://docs.scala-lang.org/ja/)を使っています。本項では、SeichiAssistで主に使われるScalaの機能等を紹介します。

### 読み方

次のように書かれる「コードスニペット」は、(特に明記が無い限りScala)で書かれたコードを含みます：

```Scala
val variable = 42
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
   - `"abc"`, `"あいうえお"`, `Scala`
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

これで、 `secondValue` は `10` を持つことになります。Scastieは最後の式の値を表示してくれるので、

```Scala
val firstValue = 3
val secondValue = firstValue * firstValue + 1

secondValue
```

と入力すると、 `secondValue` の隣に `10: Int` という表示が出ます。

### 型

さて、上で挙げた値達は、何らかの「種類」のようなものに分けられるように見えます。「値、変数や式がどんな種類であると分かっているか」の情報そのものが「型」と呼ばれるものです。

例えば`0`は`Int`(注: integer; 整数)という型であることがすぐに分かりますし、`"abc"`は`String`という型になります。Scalaには`Int`のように既に用意された型もあれば、自分で型を作ることもできます。

変数の型は明示的に書くことができます：

```Scala
val integerValue: Int = 42
```

型は、予期しない値がプログラムのとある箇所から別の箇所へ渡されてしまうことを防ぐ役割があります。以下のコードは、「`integerExpected`は`Int`であることが期待されているが、`String`である`"42"`が入ろうとしている」という理由で実行できません(Scastieに入れてみると、「型エラー」が出ることでしょう)

```
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

Scalaの値には「オブジェクト」というものがあります。オブジェクトは複数の値を一つの値として束ねたものです。

オブジェクトは、`new` キーワードにより作成でき、束ねる値として「プロパティ」を持つことができます。

`new` で作っただけのオブジェクトを変数に入れようとする時、何もしないならば型に `Any` を指定するしかありません。これだとプロパティに安全にアクセスすることができない(補足: 実はできるが実行が遅い)ため、先に `trait` として型を定義する必要があります。オブジェクトがとある `trait` の型を持つことは明示的に宣言する必要があり、 `new` の直後にtrait名を書きます。

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

先程のコードは関数を定義してそれを呼び出すだけであるのに、コードとしてそこそこ長いものになってしまいました。

先程と「全く同じ内容」を短く書くために、Scalaはそれ専用の構文を用意しています。

```Scala
val intSquareFunction: Int => Int = (x => x * x)

intSquareFunction(3) // -> 9: Int
```

まず、 `Int => Int` は `Function1[Int, Int]` と同義です。

`(x => x * x)` は「ラムダ式」と呼ばれる言語機能を使用しています。 `Function1` のように、各オブジェクトが定義しなければならないメソッドが一つしかない `trait` を SAM (**S**ingle **A**bstract **M**ethod; 単一抽象メソッド) trait  と呼びますが、SAM trait の値を短く記述するためにラムダ式が使えます：

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

## `class`、`class`による型



## コレクション

## 抽象クラス

## 多相性、多相的な定義
