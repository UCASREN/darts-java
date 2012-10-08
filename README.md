darts-java: Double-ARray Trie System Java implementation.
=========================================================

概要
----

Taku Kudo さんの Double Array Trie の C++ 実装 Darts http://www.chasen.org/~taku/software/darts/ を
MURAWAKI Yugo さんが Java ポーティングしたバージョン http://nlp.ist.i.kyoto-u.ac.jp/member/murawaki/misc/index.html 
をベースとし、より Java らしいインタフェースに変更し、また性能面も改善した Darts の Java 実装です。


元実装 (Java ポーティング版) からの改善点
-----------------------------------------

 * インタフェースの改善
   * より Java らしいインタフェースで、かつ手軽に利用できるインタフェースとしました。

 * ヒープ消費量の改善
   * Trie 構築時、および構築後のヒープ消費量が少なくなるようにしました。

 * 実行速度の改善
   * Trie の構築や exact match での検索、common prefix での検索それぞれについて処理時間を削減しました。


Benchmark
---------

### テストデータ

Ubuntu 12 の /usr/share/dict/words (916 KB、99171 語) をテストデータとして利用しています。

### 測定方法

 * ヒープ消費量
   * DoubleArrayTrie#build() の呼び出し前後それぞれにおいて、ヒープ消費量を
     Runtime#totalMemory() / Runtime#freeMemory() にて計測し、その差分をとることで
     構築後のヒープ消費量を算出しています。

 * build() 処理時間
   * ファイルから読み込んだテストデータを build() で 50 回処理し、その平均と標準偏差を算出しています。

 * exactMatchSearch() 処理時間
   * テストデータをもとに構築された Trie に対して、テストデータの語句すべてを
     順に exact match 検索する処理を 50 回実施し、その平均と標準偏差を算出しています。

 * commonPrefixSearch() 処理時間
   * exactMatchSearch() と同様に、テストデータをもとに構築された Trie に対して、
     テストデータの語句すべてを順に common prefix 検索する処理を 50 回実施し、その平均と標準偏差を算出しています。
   * 元実装では、commonPrefixSearch() の結果を同メソッドに渡した int 配列で受け取るインタフェースと
     なっているため、検索結果の個数を求めるためと、実際の結果を受け取るためのそれぞれ１回ずつ
     （合計２回）、commonPrefixSearch() を呼び出しています。

### 測定結果

```
                              original     imploved
====================================================
ヒープ消費量 [byte]         62,287,864   16,780,160
----------------------------------------------------
build() [msec]                  165.68        64.26
  (標準偏差)                    (82.87)       (6.74)
----------------------------------------------------
exactMatchSearch() [msec]        10.88         6.24
  (標準偏差)                     (7.21)       (7.73)
----------------------------------------------------
commonPrefixSearch() [msec]      17.18        14.04
  (標準偏差)                     (4.68)       (4.75)
----------------------------------------------------
```


License
-------

LGPL と修正 BSD のデュアルライセンスです。
各ライセンスの詳細は LGPL ファイル、BSD ファイルをご覧ください。
