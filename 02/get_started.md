= データベース & ORM =

データベースと ORM の基本をおさらいしてから、クエリビルダのソースコードを読む。

=== 入門 ===

CakePHP 3.4 Cookbook
https://book.cakephp.org/3.0/ja

データベースアクセス & ORM
https://book.cakephp.org/3.0/ja/orm.html

 * テーブルクラス
 * エンティティクラス
 * クエリ

クエリビルダ
https://book.cakephp.org/3.0/ja/orm/query-builder.html

 * クエリはコレクションオブジェクトである
 * クエリが遅延評価される仕組み

データベースの基本
https://book.cakephp.org/3.0/ja/orm/database-basics.html

 * コネクション管理クラス
 * コネクションクラス

=== ソースコードのファイル構成 ===

CakePHP Framework
https://github.com/cakephp/cakephp

```
src/
  Database/   -- A flexible and lightweight Database Library for PHP
                 https://github.com/cakephp/cakephp/blob/master/src/Database/README.md
  Datasource/ -- CakePHP Datasource Library
                 https://github.com/cakephp/cakephp/blob/master/src/Datasource/README.md
  ORM/        -- CakePHP ORM
                 https://github.com/cakephp/cakephp/blob/master/src/ORM/README.md
```

CakePHP Application Skeleton
https://github.com/cakephp/app

```
src/
  Behavior/   -- https://api.cakephp.org/3.4/class-Cake.ORM.Behavior.html
  Entity/     -- https://api.cakephp.org/3.4/class-Cake.ORM.Entity.html
  Table/      -- https://api.cakephp.org/3.4/class-Cake.ORM.Table.html
```
