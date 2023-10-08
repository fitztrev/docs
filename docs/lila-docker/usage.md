## Usage

### Scala development:

To restart lila (after making changes to any Scala code):

```bash
docker compose restart lila
```

### UI (JS/CSS) development:

To watch for Typescript/SCSS changes and automatically recompile:

```bash
docker compose run --rm ui bash -c "/lila/ui/build -w"
```

### To add translation keys:

After modifying a `translation/source/*.xml` file, run:

```bash
docker compose run --rm ui bash -c "/lila/bin/trans-dump"
```

### Code formatting:

```bash
docker compose run --rm ui bash -c "cd /lila && pnpm install && pnpm run format"
docker compose run --rm ui bash -c "cd /chessground && pnpm install && pnpm run format"
docker compose run --rm ui bash -c "cd /pgn-viewer && pnpm install && pnpm run format"

# sbt scalafmtAll
docker run --rm -v $(pwd)/repos/lila:/lila \
    sbtscala/scala-sbt:eclipse-temurin-focal-17.0.8.1_1_1.9.6_3.3.1 \
    bash -c "cd /lila && sbt scalafmtAll"
```

### Berserk (Python library):

To install the development version of [Berserk](https://github.com/lichess-org/berserk) and run a sample script against your local development site:

```bash
docker run --rm -v $(pwd)/repos/berserk:/berserk -v $(pwd)/scripts:/scripts python:latest \
    bash -c "cd /berserk && pip install -e . && python /scripts/berserk-example.py"
```

### Scalachess:

If you're making changes to the Scalachess library, you can have lila use it instead of the published Maven version:

1. Update the `build.sbt` file in the scalachess repo:

   ```diff
   -  ThisBuild / version           := "15.6.7"
   +  ThisBuild / version           := "my-test-1"  # give it a custom version
   ```

2. Update the `Dependencies.scala` file in the lila repo:

   ```diff
   -  val chess = "org.lichess" %% "scalachess" % "15.6.7"
   +  val chess = "org.lichess" %% "scalachess" % "my-test-1"
   ```

3. Publish the local scalachess changes and restart lila:

   ```bash
   docker compose exec lila bash -c "cd /scalachess && sbt publishLocal"
   docker compose restart lila
   ```

Other Scalachess commands:

```bash
## compile
docker run --rm -v $(pwd)/repos/scalachess:/mnt \
    sbtscala/scala-sbt:eclipse-temurin-focal-17.0.8.1_1_1.9.6_3.3.1 \
    bash -c "cd /mnt && sbt compile"

## test
docker run --rm -v $(pwd)/repos/scalachess:/mnt \
    sbtscala/scala-sbt:eclipse-temurin-focal-17.0.8.1_1_1.9.6_3.3.1 \
    bash -c "cd /mnt && sbt testKit/test"

## package
docker run --rm -v $(pwd)/repos/scalachess:/mnt \
    sbtscala/scala-sbt:eclipse-temurin-focal-17.0.8.1_1_1.9.6_3.3.1 \
    bash -c "cd /mnt && sbt package"
```

### bbpPairings:

```bash
docker build -f docker/bbpPairings.Dockerfile . -t bbppairings
docker run --rm -v $(pwd)/repos:/mnt bbppairings bash -c "\
    git clone https://github.com/cyanfish/bbpPairings \
    && cd bbpPairings \
    && make \
    && chmod +x bbpPairings.exe \
    && cp bbpPairings.exe /mnt"

## verify
./repos/bbpPairings.exe
```

### Developing Chessground locally

By default, your local lila instance will use the version of chessground that is published to npm. If you want to make changes to that library and see them reflected in your local lila instance, you can do the following:

1. Update the `package.json` in the `lila` repo:

   ```diff
   "dependencies": {
   -  "chessground": "^8.3.11",
   +  "chessground": "link:/chessground",
   }
   ```

2. Start the chessground compiler in watch mode:

   ```bash
   docker compose run --rm ui bash -c "cd /chessground && pnpm install && pnpm run compile --watch"
   ```

3. Start the lila ui build in watch mode:

   ```bash
   docker compose run --rm ui bash -c "/lila/ui/build -w"
   ```

Then you can see the updated chessground demo at http://localhost:8080/chessground/demo.html and when you refresh lila, it will use the local copy of chessground.

### Developing PGN Viewer locally

To re-compile the PGN Viewer after making changes:

```bash
docker compose run --rm ui bash -c "cd /pgn-viewer && pnpm run sass-dev && pnpm run bundle-dev"
```

See the changes on the PGN Viewer demo page: http://localhost:8090/
