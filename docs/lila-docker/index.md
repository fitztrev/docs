# lila-docker

Lichess local development using Docker Compose, for developing on Mac or Linux.

The only requirements for running on your local machine are `git` and Docker Desktop. All the other dependencies (Scala, MongoDB, Node.js, etc) are installed and run in Docker containers.

## Instructions

1. Install [Docker Desktop](https://www.docker.com/products/docker-desktop/) and have it running

1. Clone this repo:

   ```bash
   git clone https://github.com/fitztrev/lila-docker
   ```

1. Start the services

   ```bash
   cd lila-docker
   ./lila-docker start
   ```

   Starting new services may take 5-10 minutes. Some services will start before others and you may see errors in the logs until everything comes online.

   Lila will be the last service to complete, at which point you can visit http://localhost:8080/ to see the site.

## URLs

When the services start, you can visit the following URLs:

| Service            | URL                                                                                        |
| ------------------ | ------------------------------------------------------------------------------------------ |
| Main lila instance | [http://localhost:8080/](http://localhost:8080/)                                           |
| Chessground demo   | [http://localhost:8080/chessground/demo.html](http://localhost:8080/chessground/demo.html) |
| Mongodb manager    | [http://localhost:8081/](http://localhost:8081/)                                           |
| API docs           | [http://localhost:8089/](http://localhost:8089/)                                           |
| PGN Viewer         | [http://localhost:8090/](http://localhost:8090/)                                           |
| Email inbox        | [http://localhost:8025/](http://localhost:8025/)                                           |

Depending on which optional services you start:

| Service               | URL                                                                                                                  |
| --------------------- | -------------------------------------------------------------------------------------------------------------------- |
| lila-gif              | [http://localhost:6175/image.gif?fen=4k3/6KP/8/8/8/8/7p/8](http://localhost:6175/image.gif?fen=4k3/6KP/8/8/8/8/7p/8) |
| Picfit                | [http://localhost:3001/healthcheck](http://localhost:3001/healthcheck)                                               |
| Elasticsearch manager | [http://localhost:5601/](http://localhost:5601/)                                                                     |

### Stopping

To stop the containers, for later resuming via `./lila-docker start`:

```bash
./lila-docker stop
```

To remove the containers:

```bash
./lila-docker down
```
