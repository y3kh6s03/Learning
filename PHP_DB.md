# PHP　から　DB操作

## DBからデータ取得
   PDOクラスを利用する
   1. $conn = new PDO($dsn, $user, $pwd);
      $dsnには、接続DB情報を渡す
      $dsn = "mysql:host={$host};dbname={$dbname};";
   2. PDOで取得したデータを変数に格納してクエリを流すことができる
      $pst = $conn->query(DBへのクエリを記述);
   3. $result = $pst->fetchAll();
      このクエリを変数に格納するとfetchAllのメソッドでオブジェクトに変更可能
      fetchAllの引数にPDO::FETCH_ASSOCを渡すと連想配列にできる。
   4. この結果を出力すると結果が確認できる
      ※この結果を
      echo "<pre>"
      print_r($result);
      echo "<pre>"
      と記述すると改行を含めて表示してくれる。
   5. DBからデータを取得したら、$connを初期化する
      PDOでは必要ないが、ライブラリやプログラミング言語によって
      接続が残ったままになり、無駄になることがあるため
      $conn = null;
      このように初期化する必要がある

## DBの値の更新
    PDOのコネクション変数から
    $conn->exec("DBへのクエリを記述");
    記述例
    $result = $conn->exec('update mst_prefs set name = "福島" where id = 5');


## 例外処理
   try-catch-finallyによる実装
   サーバープログラムを実装する上で必要となる
   目的は、クエリの途中でエラーが発生したときに、処理がとまることを防ぐ
   1. try{
      実装したい記述
      }catch(Error階層からクラス継承 $e){
        エラー内容の表示
      }finally{
        処理終了の処理
      };
   2. 関数定義の中で、個別詳細なErrorクラスを継承してエラーを断定的にする
   3. functionを実行するときに汎用的なErrorクラスを継承して、エラーハンドリングを行う
   4. 最終的にErrorのクラスを表示するために
      get_class(catchもしくはfinallyブロックのエラークラスが格納された変数を渡す);