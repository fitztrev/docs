- Install [elasticsearch 7.x.x](https://www.elastic.co/guide/en/elasticsearch/reference/7.17/install-elasticsearch.html)

  No special config is necessary. We'll use the default port 9200

- Setup [lila-search](https://github.com/lichess-org/lila-search):

  ```
  git clone https://github.com/lichess-org/lila-search
  cd lila-search
  sbt stage
  ./target/universal/stage/bin/lila-search
  ```

  `sbt run` does not work for lila-search at press time but you can use stage build. Maybe this will be fixed.

- Add the following line to `lila/conf/application.conf`:
  ```
  search.enabled = true
  ```
- Either use [spamdb](https://github.com/lichess-org/lila-db-seed) to add and automatically index games, posts, and teams:
  ```
  spamdb/spamdb.py --es
  ```
  `--es` connects spamdb to elasticsearch (at localhost:9200) and indexes your database.
  You're done, you can skip the rest.
- Or create indices manually (games will only be indexed on completion or computer analysis):

  - Before playing / analyzing any games on lila, you must do this:

    ```
    curl -XPOST http://localhost:9673/mapping/game
    ```

  - Now you can startup lila, login as the lichess user, go to `http://localhost:9663/dev/cli`, and run these commands:

        ```
        forum search reset
        team search reset
        study search reset
        ```

    You should now be able to search.
