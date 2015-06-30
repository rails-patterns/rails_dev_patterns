公然のSQL
=========

コンテクスト
------------

ActiveRecordを使ってデータベースにアクセスしようとしている。

問題
----

意図通りのデータベースクエリを発行する方法が分からずに時間を浪費したり、無意識のうちに高負荷のクエリを発行したりする。

問題の原因
----------

ActiveRecordを使うと実際のSQLに比べて短いRubyコードでデータベースクエリを生成できる。また、実際のSQLの語順や、データベースの種類に依存せずに記述できるなどのメリットもある。

しかし、より複雑なクエリを生成するためにRubyコードも複雑になることは避けられない。また、ActiveRecordでは特定のデータベースに固有の最適化手法を利用できない場合もある。

フォース
--------

* コードの読みやすさや互換性のために、ActiveRecordを通じてSQLを構築するのは望ましい。
* 複数のデータベースの全ての機能を単一のライブラリでサポートすることはできない。

解決方法
--------

実際に発行されるSQLを確認する。期待したSQLにならないときは、SQLのハードコーディングも検討する。

例
--

#### 1. UNION

ActiveRecordでSQLのUNIONを記述する方法は少なくとも[2011年3月から議論されている](https://github.com/rails/rails/issues/939)が、[2015年6月の時点でも解決していない](https://github.com/rails/arel/pull/320)。よって、自分で書けるようにしておく必要がある。

#### 2. N+1問題

「N+1問題」は、無駄なデータベースクエリを発生させる[典型的な問題である](http://guides.rubyonrails.org/active_record_querying.html#eager-loading-associations)。例えば、下のようにBookとAuthorと対応するテーブルが定義されているとき、

```ruby
class Author < ActiveRecord::Base
end

class Book < ActiveRecord::Base
  belongs_to :author
end
```

```ruby
create_table :authors do |t|
  t.string :name
end

create_table :books do |t|
  t.string :name
  t.references :author, index: true
end
```

次のようなコードは合計11回のクエリを発生する。

```ruby
# コントローラ
@books = Book.last(10)
```

```ruby
# ビュー
<% @books.each do |book| %>
  <tr>
    <td><%= book.name %></td>
    <td><%= author.name %></td>
  </tr>
<% end %>
```

実際に発行されるクエリは下の通り。

    SELECT  `books`.* FROM `books`  ORDER BY `books`.`id` DESC LIMIT 10
    SELECT  `authors`.* FROM `authors` WHERE `authors`.`id` = 1 LIMIT 1
    SELECT  `authors`.* FROM `authors` WHERE `authors`.`id` = 2 LIMIT 1
    …
    SELECT  `authors`.* FROM `authors` WHERE `authors`.`id` = 9 LIMIT 1
    SELECT  `authors`.* FROM `authors` WHERE `authors`.`id` = 10 LIMIT 1

コントローラのコードを以下のように修正することで、クエリは2回になる。

```ruby
@books = Book.includes(:author).last(10)
```

修正後のクエリは以下の通り。

    SELECT  `books`.* FROM `books`  ORDER BY `books`.`id` DESC LIMIT 10
    SELECT `authors`.* FROM `authors` WHERE `authors`.`id` IN (10, 9, 8, 7, 6, 5, 4, 3, 2, 1)

結果
----

データベースクエリに起因する問題により早く気付けるようになり、リリース後に障害が発生するリスクを下げる。
