# Bootstrapの利用

1. Bootstrapの利用可能クラスの検索
   デベロッパーツールのsourceから確認可能

   1. sassのマップファイルの削除
      マップファイルがあることでsassファイルとの紐付けをしたファイルが生成されてしまう
      そのためmapファイルの削除とmapファイルの作成を止める
      1. mapファイルの生成停止方法
         vsコードの歯車→セッティング→検索で'live sass compiler'
         Generate mapをfalseに変更する
      2. デベロッパーツールからだいたい予想できる単語で検索を行って使用クラスを検索することができる