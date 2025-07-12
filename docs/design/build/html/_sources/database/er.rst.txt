データベース全体ER図
============================================

**最終更新**: [ 更新日を記載 ]

概要
--------------------------------------------

このページでは、システム全体のデータベース構造をER図で表現し、テーブル間の関係性を明確に示します。

全体ER図
--------------------------------------------

.. mermaid::

   erDiagram
       users {
           UUID id PK "ユーザーID"
           VARCHAR email UK "メールアドレス"
           VARCHAR password_hash "パスワードハッシュ"
           VARCHAR status "ステータス"
           TIMESTAMP last_login_at "最終ログイン日時"
           TIMESTAMP created_at "作成日時"
           TIMESTAMP updated_at "更新日時"
       }

       user_profiles {
           UUID user_id FK "ユーザーID"
           VARCHAR first_name "名"
           VARCHAR last_name "姓"
           VARCHAR postal_code "郵便番号"
           VARCHAR prefecture "都道府県"
           VARCHAR city "市区町村"
           VARCHAR street_address "番地以降"
           VARCHAR phone "電話番号"
           TIMESTAMP created_at "作成日時"
           TIMESTAMP updated_at "更新日時"
       }

       products {
           UUID id PK "商品ID"
           VARCHAR name "商品名"
           DECIMAL price "価格"
           INTEGER stock "在庫数"
           VARCHAR status "ステータス"
           TIMESTAMP created_at "作成日時"
           TIMESTAMP updated_at "更新日時"
       }

       shopping_carts {
           UUID id PK "カートID"
           UUID user_id FK "ユーザーID"
           TIMESTAMP expires_at "有効期限"
           TIMESTAMP created_at "作成日時"
           TIMESTAMP updated_at "更新日時"
       }

       cart_items {
           UUID cart_id FK "カートID"
           UUID product_id FK "商品ID"
           INTEGER quantity "数量"
           DECIMAL unit_price "単価"
           DECIMAL subtotal "小計"
           TIMESTAMP created_at "作成日時"
           TIMESTAMP updated_at "更新日時"
       }

       users ||--o{ shopping_carts : "has"
       users ||--|| user_profiles : "has"
       shopping_carts ||--o{ cart_items : "contains"
       cart_items }o--|| products : "references"

テーブル一覧
--------------------------------------------

.. list-table::
   :header-rows: 1

   * - テーブル名
     - 論理名
     - 主要用途
     - 詳細設計書
   * - users
     - ユーザー
     - ユーザー情報を管理
     - :doc:`users`
   * - user_profiles
     - ユーザープロファイル
     - ユーザーの詳細情報を管理
     - :doc:`user_profiles`
   * - products
     - 商品
     - 商品マスタ情報を管理
     - :doc:`products`
   * - shopping_carts
     - ショッピングカート
     - カート機能を管理
     - :doc:`shopping_carts`
   * - cart_items
     - カート商品明細
     - カート内の商品明細を管理
     - :doc:`cart_items`

主要リレーションシップ
--------------------------------------------

**現在実装済み**:

.. list-table::
   :header-rows: 1

   * - 親テーブル
     - 子テーブル
     - 関係性
     - 外部キー
     - カーディナリティ
   * - users
     - shopping_carts
     - ユーザーとカートの関係
     - shopping_carts.user_id
     - 1:多（1人のユーザーが複数のカートを持つ）
   * - users
     - user_profiles
     - ユーザーとプロファイルの関係
     - user_profiles.user_id
     - 1:1（1人のユーザーが1つのプロファイルを持つ）
   * - shopping_carts
     - cart_items
     - カートと商品明細の関係
     - cart_items.cart_id
     - 1:多（1つのカートが複数の商品明細を持つ）
   * - cart_items
     - products
     - 商品明細と商品の関係
     - cart_items.product_id
     - 1:多（1つの商品が複数の商品明細で使用）

**将来実装予定**:

.. list-table::
   :header-rows: 1

   * - 親テーブル
     - 子テーブル
     - 関係性
     - 外部キー
     - カーディナリティ
   * - users
     - orders
     - 会員と注文の関係
     - orders.member_id
     - 1:多（1人の会員が複数の注文を持つ）
   * - orders
     - order_items
     - 注文と注文明細の関係
     - order_items.order_id
     - 1:多（1つの注文が複数の明細を持つ）
   * - products
     - order_items
     - 商品と注文明細の関係
     - order_items.product_id
     - 1:多（1つの商品が複数の明細で使用）

データベース制約サマリー
--------------------------------------------

**一意制約**:

- [ テーブル名.カラム名 ]: [ 制約の説明 ]

**外部キー制約**:

- [ 子テーブル.外部キー ] → [ 親テーブル.主キー ]: [ 制約の説明 ]

**チェック制約**:

- [ テーブル名.カラム名 ]: [ 許可される値の説明 ]

インデックス戦略
--------------------------------------------

**高頻度検索用インデックス**:

.. list-table::
   :header-rows: 1

   * - インデックス名
     - 対象テーブル
     - 対象カラム
     - 用途
   * - [ インデックス名 ]
     - [ テーブル名 ]
     - [ カラム名 ]
     - [ 用途説明 ]

**複合インデックス**:

.. list-table::
   :header-rows: 1

   * - インデックス名
     - 対象テーブル
     - 対象カラム
     - 用途
   * - [ インデックス名 ]
     - [ テーブル名 ]
     - [ カラム名, カラム名 ]
     - [ 用途説明 ]

拡張予定
--------------------------------------------

**フェーズ2（商品・注文機能）**:

- **商品テーブル**: 商品マスタ情報の管理
- **注文テーブル**: 注文情報の管理  
- **注文明細テーブル**: 注文商品詳細の管理
- **カート機能**: 一時的な商品保持

**フェーズ3（決済・配送機能）**:

- **決済テーブル**: 決済履歴の管理
- **配送テーブル**: 配送状況管理
- **配送先テーブル**: 複数配送先の管理

**フェーズ4（拡張機能）**:

- **レビューテーブル**: 商品レビュー機能
- **ポイントテーブル**: ポイント制度
- **クーポンテーブル**: 割引クーポン機能
- **お気に入りテーブル**: ウィッシュリスト機能

関連ドキュメント
--------------------------------------------

- :doc:`template`: データベーステーブル設計テンプレート
