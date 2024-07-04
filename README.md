# compose-sample
Dockerの復習
Docker composeでDjangoのアプリケーションを立ち上げる

以下の要素で構成する
    Webサーバ: nginx
    アプリケーションサーバ: uvicorn
    Webアプリケーション: django
    データベースサーバ: postgreSQL

手順
1. Compose定義ファイルにコンテナを定義
    djangoコンテナ
        "django", "src"ディレクトリを作成
        "django"ディレクトリにDockerfileを作成
        requirements.txtの作成
    作業ディレクトリ直下にdocker-compose.yamlを作成
    docker-compose.yamlがあるディレクトリで一度djangoコンテナを立ち上げる
        $ docker compose run django django-admin startproject config .
        => yaml内のdjangoサービスに基づいてコンテナを起動、コンテナ上で"django-admin startproject config ."を実行
        (Linuxの場合,ファイルの所有者を現在ログインしているユーザに変更: sudo chown -R $USER:$USER .)
    config/settings.pyを編集後、djangoコンテナを起動
        $ docker compose up -d
        起動後、"http://127.0.0.1:8000"にアクセスするとdjangoのアプリが確認できる
    一度コンテナを削除
        $ docker compose down
    
    
    postgreSQLコンテナ
        docker-compose.yamlに追記
        config/settings.py-DATABASESを編集してデータベースへの接続
    docker-compose.yamlに従ってイメージのビルド
        $ docker compose build
    djangoコンテナとdbコンテナを起動
        $ docker compose up -d
    djangoコンテナに入り、マイグレーションを実行
        $ docker compose exec django python manage.py migrate --noinput
        *--noinputを指定することでコマンド実行中に入力を要求しない
    dbコンテナに入り、postgreSQLに接続
        $ docker compose exec db psql -U learner -d testdb
        データベースを確認
            $ \dt
        データベースからでる
            $ \q


    nginxコンテナ
        nginxディレクトリ、nginx/default.confを作成
        djangoのcollectstaticを実行して静的ファイルを指定ディレクトリに集約
            $ docker compose run django python manage.py collectstatic
            => src/collected_staticディレクトリが作成され、その下に静的ファイルが集約される



    *docker-comopose.yamlに誤りがあった場合
        一度コンテナを全て削除する
        $ docker compose down -v
            -vオプションをつっけることでボリュームごと削除する、データベースのデータも消えてしまうことに注意
        もう一度イメージのビルドからやり直す
2. コンテナ用のイメージを作成
3. コンテナを展開