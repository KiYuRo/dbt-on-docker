# dbtをDockerで試す

# ディレクトリ構成
```
C:.
├─dbt           # dbtのプロジェクトを格納
│  └─project    # 作成したdbtプロジェクト
└─docker        # docker環境構築関連のファイルとディレクトリを格納
    ├─database  # データベースコンテナ設定用
    │  ├─data   # データベースを永続化するためのディレクトリ
    │  └─init   # コンテナ起動時に初回実行するSQLファイルを格納
    └─dbt       # dbtサーバ用
```

# Docker環境構築
コンテナ起動
```
docker compose up -d
```

dbtコンテナ接続
```
docker exec -it dbt /bin/bash
```

postgresコンテナ内のDB接続
```
docker exec -it database /bin/bash
psql -U root
```

dbtバージョン確認
```
dbt --version
```

```
out:

Core:
  - installed: 1.8.6
  - latest:    1.8.6 - Up to date!

Plugins:
  - postgres: 1.8.2 - Up to date!
```

# dbtプロジェクト作成
プロジェクトを作成．DBの接続情報を入力していく．
```
$ dbt init

04:51:46  Running with dbt=1.8.6
Enter a name for your project (letters, digits, underscore): project

Enter a number: 1
host (hostname for the instance): database
port [5432]: 5432
user (dev username): root
pass (dev password): root
dbname (default database that dbt will build objects in): root
schema (default schema that dbt will build objects in): public
threads (1 or more) [1]: 1
09:31:14  Profile test written to /root/.dbt/profiles.yml using target's profile_template.yml and your supplied values. Run 'dbt debug' to validate the connection.
```

以下のようなディレクトリが作られる
```
$ cd project
$ ls
README.md  analyses  dbt_project.yml  macros  models  seeds  snapshots  tests
```

接続確認
```
$ dbt debug

...
04:53:07  All checks passed!
```

# モデル実行
サンプルのモデルを実行する
```
$ dbt run

...
04:54:16  Completed successfully
04:54:16
04:54:16  Done. PASS=2 WARN=0 ERROR=0 SKIP=0 TOTAL=2
```

テーブルが作られる
```
root=# \dt
              List of relations
 Schema |        Name        | Type  | Owner
--------+--------------------+-------+-------
 public | my_first_dbt_model | table | root
(1 row)
```

# モデルテスト
```
$dbt test

05:22:37  Done. PASS=3 WARN=0 ERROR=1 SKIP=0 TOTAL=4
```
