# docker-php-dev-template
DockerでPHP7.2+MySQL8.0+PostgreSQL10環境で開発するテンプレートプロジェクトです。

プロジェクトに応じてApacheのDocumentRootやマウントするディレクトリを変更してください。

初期構成では、ララベルのインストーラも導入されます。

不要であれば、./php/Dockerfileを編集してビルドしてください。


## プリインストールアプリケーション
* adminer(/adminer)
* memcache(/memcached)
  * ID:memcache/PW:password
* mailcacher(localhost:1080)
  * メールのポート番号は1025なので注意

## DB設定
1. mysql.envというファイルを作成
1. 以下の内容をセット
    ```
    MYSQL_ROOT_PASSWORD=<ROOTパスワード>
    MYSQL_USER=<DBユーザ名>
    MYSQL_PASSWORD=<DBパスワード>
    MYSQL_DATABASE=<DB名>
    ```
1. pgsql.envというファイルを作成
1. 以下の内容をセット
    ```
    POSTGRES_PASSWORD=<DBパスワード>
    POSTGRES_USER=<DBユーザ名>
    POSTGRES_DB=<DB名>
    ```
1. adminerを使うなりコマンドなりでお好きにどうぞ。
   なお、初期構成では外部にDBのポートを解放していないので、外部からは繋がりません。
   
   必要に応じ、docker-composeを変更してください。
