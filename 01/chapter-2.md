Chapter2 Organizing Domain Logic
================================================================================

ドメインロジックには以下のパターンがある:

* Transaction Script
* Domain Model
* Table Module


## Transaction Script

* プレゼンテーションから入力を受け取り、バリデーション、計算をし、データをデータベースに保存し、他のシステムの操作を呼び出す。
  そしてデータをプレゼンテーションに戻し、返信データを構成したりフォーマットするための計算を行う
* これをアクションのスクリプト、あるいはビジネストランザクションと考えることができる
* このようなコードはいくつかのサブルーチンに分かれていて、サブルーチンは複数の
    Transaction Scripts 間で共有されている。
* 例として、販売システムは Transaction Script
    を購入、ショッピングカートへの追加、配送ステータスの表示などに使うだろう


* Transaction Scripts の利点は以下:
    * 完結な手続きモデルでほとんどの開発者が理解できる
    * Row Data Gateway, Table Data Gateway
        を使ったシンプルなデータソースレイヤとうまく動く
    * どのように transaction boundary
        が設定されるか明確である（トランザクション開始と終了。ツールによって裏側で行うことが容易）:W
* Transaction Scripts の欠点は、ドメインロジックが増えると現れてくる
    * いくつかのトランザクションは同じことをするため、コードの重複が発生する


## Domain Models

* 複雑なロジックは Domain Model で扱う
    * バリデーション、計算などは Domain Model のメソッドに委譲する


## Transaction Scripts / Domain Models の比較

* Transaction Scripts はすべての処理を行う。基礎にあるオブジェクトは Table Data
    Gateway のみ
* Domain Models を使った例では、それぞれが振る舞いの一部を次の処理に私、最終的に
  strategy object が結果をつくる
* Domain Models を使うと、複雑な処理を strategy objects を追加することによって
  よく整理された状態にできる


## Table Module

* Table Module は Transactio Scripts と Domain Models の中間に位置する
* テーブルに関するドメインロジックを整理するもの
* Domain Models のようなロジックの構造化や継承、 strategy, OO パターンなどはない
* 利点は SQL の結果を使って動くような GUI 環境の場合に Table Module
    を使って結果を操作するのが簡単な点


## どのようにパターンを選ぶか

* ソフトウェアの複雑さによる
* 複雑なソフトウェアになる場合は Domain Models を使うことを勧めている
    * 複雑さを図るのは難しいので経験のある開発者に判断してもらう*
* Transaction Scripts で始めてから Domain Models
    に変更するのに躊躇しなくてよい。
  ただし、 Domain Models から Transaction Scripts
  に変更するのはデータソースレイヤを変更できないのであればあまり価値はない


## サービスレイヤ

* 2つのドメインレイヤに分けるアプローチはよくあること。サービスレイヤは Domain
    Models や Table Modules にまたがって存在する
  * プレゼンテーションロジックはサービスレイヤを通してドメインと接する。それはアプリケーションの
  API として振る舞う
* サービスレイヤをどのように利用するか
    * 最小の実装は、基礎となるオブジェクトに振る舞いがあり、サービスは facade
        になるパターン。サービスレイヤはユースケースの中心となり、簡単に扱える
        API を提供する
