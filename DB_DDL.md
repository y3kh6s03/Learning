# DB DDL　データ定義言語
## SQL操作　DBeaver

### DBの作成
1. 作成の時点で制約を行なって管理をすることがベター
   1. not null制約や文字数、一意の値とするなどの制約を行う
   2. チェックの実装はDBでは行わない。PHPで行う
      PHPとDBでチェックすると二重管理になり、無駄
      変更したときに、片方だけ変更するなどのミスにつながる
### 主キー primary key
1. primary keyと設定する
2. primary keyであれば、not null、uniqueが設定されている
3. 複合キーは、記述方法が異なるから注意
4. 複合キーの場合には、uniqueな値では管理できないのでnot nullだけのイメージ
5. 主キーに対して、IDを0〜の数字を付与するるためにauto_incrementがある
   1. インデックス(uniqueやprimary key)をもつ必要がある.もしくは、index(auto_incrementをもたせるkey)このような記述で明示的にもたせる→show index from DB名.テーブル名とすれば確認できる
   2. ひとつのテーブルに対して、ひとつの属性に付与できる　primmary keyを設定するなど
   3. default設定は行えない
### テーブル内容の変更 alter table
以下はすべてalter tableでテーブルを指定して以下を記述する
1. alter tableでdbとtableを指定
2. add columnでcreateのように記述することで追加可能
   1. after 既存のkey名　このように記述すると既存のkeyの下にcolumnを追加できる
   2. first と記述すると最初に追加することができる(基本的にprimary keyをもつcolumnが先頭にするため、使用頻度低い)
3. modify columnとすると既存のcolumnの変更を行うことができる
4. drop column keyを指定　このようにするとcolumnを削除できる
   auto_incrementがついていると削除できない
   その場合はmodify columnで内容を変更してから実行する

###　外部キー
1. 型情報を合わせる必要がある
2. 外部キーに設定するとインデックスが自動付与される
3. cascade 親テーブルの行を削除・更新した場合、子テーブルの一致する行を連動させる
4. restrict 親テーブルに対する削除・更新を拒否する
5. 記述方法
   1. alter table テーブル名
   2. add constraint 制約名
   3. foreign key (対象のキー)
      1. references 親テーブル名(id名)
      2. on update cascade
      3. on delete restrict --- 省略可能
6. 外部キー制約はつける場合とつけない場合がある
   外部キー制約は制約が強いため

### 実践的なテーブル定義
1. トランザクションテーブル 通称エントリーテーブル
   1. アプリから頻繁にデータの挿入・更新するテーブル
   2. 具体的にオーダー情報、顧客情報、請求情報など
   3. テーブル名の先頭にENT、TXN、TRNなどの識別子がつくことが多い


2. マスタテーブル
   1. 参照値を保持するテーブル
   2. アプリから基本的に挿入・変更しない
   3. 具体的に商品一覧・店舗一覧など
   4. テーブルの先頭にMSTの識別子がつくことが多い

3. 論理削除フラグの導入
   レコードの有効性を識別するためのフラグ
   delete_flug=1の場合には無効レコードとして扱う　など
   columnの中に delete_flg int(1) not null default 0などのように

4. 更新日、更新者の導入(updated_at, updated_by)
   アプリのバグ特定に役立つ
   columnを作成して
   1. update_at timestamp default current_timestamp on update current_timestamp
   2. update_by varchar(20) not null