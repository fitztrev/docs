The following instructions outline how to set up your development environment for starting development on Lichess. The instructions are aimed to be agnostic of the platform the stack is installed on, so a working knowledge of the specifics of your GNU/Linux distribution or other such Unix-based operating system is assumed. 

## Getting Help

If you get stuck during the installation process the most suitable place to seek help is the `#lichess-dev-onboarding` channel on Discord (https://discord.gg/lichess). The main developer of Lichess (thibault) can be found there as well as several people who have successfully installed the stack.

You may also find answers to other development-related questions in the [GitHub Wiki](https://github.com/lichess-org/lila/wiki).

## Stream recordings

[![https://www.youtube.com/watch?v=AejdHlL902w](https://img.youtube.com/vi/AejdHlL902w/0.jpg)](https://www.youtube.com/watch?v=AejdHlL902w&list=PLe5ZNOR8Ttm1LlBRmmnccZkQa7XH1K0rK)

## Prerequisites

Before beginning, please make sure you have the following tools installed, using your favourite package manager to install them where applicable.

### Hardware 
* At least 4 GB of RAM
* A CPU with 64-bit architecture.

### Tools and dependency managers
* `git`
* `java` (JDK >= 17, type `java --version` to see if you need to update or use [jenv](https://github.com/jenv/jenv) if you need multiple jdk versions)
* `cs` (cs >= 2, [installing coursier](https://get-coursier.io/docs/cli-installation) will provide sbt as well)
* `node` (node >= 14.3, [install via NVM preferred](https://github.com/nvm-sh/nvm#installing-and-updating)) or ([install node only](https://github.com/nodesource/distributions/blob/master/README.md#installation-instructions))
* `pnpm` (pnpm 8.x, type `npm i -g pnpm` after installing node) or (see the [pnpm homepage](https://pnpm.io/installation))

### Running infrastructure
* `mongodb` (5.1 >= mongo >= 4.2, [instructions](https://docs.mongodb.com/manual/administration/install-on-linux/), [WSL2](https://stackoverflow.com/questions/62495999/installing-mongodb-in-wsl))
  * For WSL2, you might want to manually create the default `/data/db` directory and give ownership permissions to your user (``sudo chown -R `id -un` /data/db``). If `sudo service mongod start` does not work, you may want to open a terminal and run `mongod` as super-user.
* `redis` 

Alternatively, if you have setup [docker-compose](https://docs.docker.com/compose/install/) on your machine, write a `docker-compose.yml` file:

```
version: "3.3"
services:
  redis:
    image: redis:6-alpine
    ports:
      - 6379:6379
  mongo:
    image: mongo:5.0
    restart: always
    container_name: lila_mongo
    ports:
      - 27017:27017
    volumes:
      # use `docker exec -it lila_mongo bash` to get a shell inside mongo
      # container. Directory containing this docker-compose.yml will be mounted
      # as /host inside container so you can import db dumps, etc.
      - .:/host
      - lila_mongo_data:/data/db

volumes:
  lila_mongo_data: {}
```

and spin up a `redis` and `mongodb` instance with:

    docker-compose up

## Installation

### Setup lila

```sh
git clone --recursive https://github.com/lichess-org/lila.git
cd lila
mongosh lichess < bin/mongodb/indexes.js # creates database indexes
# or `mongosh mongodb://localhost:27017/lichess < lila/bin/mongodb/indexes.js` if you use docker
ui/build # builds client. -h for help and -w for incremental watch mode.
./lila # starts the SBT console
```
Once the console has booted, you will see a `lila>` prompt. Type `compile` and sit back. The full compilation takes 5 minutes on GitHub CI servers.

When it's done,  type `run` to start the HTTP server.
Then open http://127.0.0.1:9663 in your browser.

> [Read more about the SBT console commands](https://www.playframework.com/documentation/2.8.x/PlayConsole).

### Setup websockets

If you need websockets (which you probably do):
```sh
git clone https://github.com/lichess-org/lila-ws.git
cd lila-ws
sbt run -Dcsrf.origin=http://localhost:9663
```

### UI dev tip

Run `pnpm add-hooks` to configure the lila git workspace to format staged files with prettier prior to every commit. You may also install a prettier plugin in your code editor to format on save. All source files with the `ts`, `js`, or `json` extensions must be prettified.

Watch client typescript and scss code automatically using `ui/build -w`. Changes you make to source files will be detected, compiled, and available in your browser after a [hard refresh](https://www.google.com/search?q=hard+refresh).  You may also disable asset caching in your browser inspector so that ordinary page reloads will pick up changes as well.

### Recommended: Seed database
You can use https://github.com/lichess-org/lila-db-seed to seed your local database with dummy data.
```
git clone https://github.com/lichess-org/lila-db-seed
```

For users, games, puzzles, teams, forums, blogs, game histories, timelines, activity, and more - [use the `spamdb.py` script to populate your database](db.md) (requires python 3.9+).
```
pip3 install pymongo
python3 lila-db-seed/spamdb/spamdb.py --help
```

Or, you may install game & puzzle data only:
```
cd lila-db-seed
mongorestore dump
```

### Optional: Setup Stockfish analysis

Start a fishnet client for analysis (requires a recent [Rust toolchain](https://rustup.rs/) to build from source, [alternatives](https://github.com/lichess-org/fishnet#readme)):

```sh
git clone --recursive https://github.com/lichess-org/fishnet.git
cd fishnet
cargo run -- --endpoint http://localhost:9663/fishnet/
```

### Optional: Setup "Play with the computer"

lila-fishnet enables playing vs Stockfish (not needed for analysis):

```sh
git clone https://github.com/lichess-org/lila-fishnet.git
cd lila-fishnet
sbt run -Dhttp.port=9665
```

You will also need a client. Start a fishnet client for play against the machine (requires a recent [Rust toolchain](https://rustup.rs/) to build from source, [alternatives](https://github.com/lichess-org/fishnet#readme)):

```sh
git clone --recursive https://github.com/lichess-org/fishnet.git
cd fishnet
cargo run -- --endpoint http://localhost:9665/fishnet/
```

### Optional: Setup Search
[Follow these instructions to enable game, forum, team, and study search on Lila](lila-search.md)

## Development

[Learn more about using `pnpm` and `ui/build` to work on client code](https://github.com/lichess-org/lila/wiki/Lichess-UI-Development)

### Bloop based setup for hacking lila scala code

[Set up bloop for quick builds and IDE features](bloop.md).

### Code formatting

These repositories use [scalafmt](https://scalameta.org/scalafmt/) for Scala and [prettier](https://prettier.io/) for everything else.

Please [install scalafmt for your editor](https://scalameta.org/scalafmt/docs/installation.html), or run `scalafmtAll` in the sbt console before submitting code.

Likewise, pick a plugin for prettier ([coc-prettier](https://github.com/neoclide/coc-prettier) is good for nvim), or type `pnpm format` in the project root.

## Contributing

Collaboration on the website front and back end is managed through the [lila GitHub repo](https://github.com/lichess.org/lila). You may contribute using the GitHub [Pull Request](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/about-pull-requests) mechanism. Assuming you have `git` installed, a GitHub account with [authentication](https://mgimond.github.io/Colby-summer-git-workshop-2021/authenticating-with-github.html) in place, a [fork](https://docs.github.com/en/get-started/quickstart/fork-a-repo) of the lila repo - the following commands will get you started.

```
  git clone --single-branch --filter=blob:none https://github.com/YOUR-USERNAME/lila # get the sources
  # git clone --single-branch --filter=blob:none git@github.com:YOUR-USERNAME/lila.git # operate over SSH
  git remote add upstream https://github.com/lichess-org/lila # add a ref for the parent repo
  git checkout -b YOUR-BRANCH # branches are human readable & dash separated, otherwise your choice
  # make your edits to sources
  git add -A # stages all changes
  git commit -m 'your commit message' # commit staged changes to git on your host
  # you might want to rebase against master and resolve conflict(s) before pushing
  git push origin YOUR-BRANCH # push the state of your local git to your fork.
  # notice that git push's output contains a URL you can visit to create your Pull Request
```
A couple git configs that might ease your git workflow:

- git config [branch.autoSetupMerge](https://git-scm.com/docs/git-branch#Documentation/git-branch.txt-branchautoSetupMerge) inherit
- git config [push.autoSetupRemote](https://git-scm.com/docs/git-push#Documentation/git-push.txt-pushautoSetupRemote) true
- git config [push.default](https://git-scm.com/docs/git-push#Documentation/git-push.txt-pushdefault) current

If you don't want to use the command line, I don't blame you! Consider downloading and getting acquainted with [GitHub Desktop](https://desktop.github.com/) for Windows, Mac, or [Linux](https://github.com/shiftkey/desktop#installation-via-package-manager). It's a basic UI with menus and things to click and will walk you through forking a project for the first time. Most will find it less intimidating than the command line procedures. But if you want even more command line, there is a dedicated [GitHub CLI](https://cli.github.com) for you to try out!

## Other miscellaneous tips

Here are [some hints](../development/ui.md) for working on various parts of the system.

Spamdb creates a `lichess` admin user and a number of mod accounts listed at the top of `lila-db-seed/spamdb/data/uids.txt`.  If you want to make an admin user manually, connect to the lichess db with `mongo lichess` and run
```
db.user4.update({ _id: "your_id" }, {$set: {roles: ["ROLE_SUPER_ADMIN"]}})
```
With `your_id` being the username in lowercase.

Installation and running notes from a new lichess-dev contributor, with detailed installation command line log and tips for running (e.g., in order to access lichess-dev from an outside system):
https://github.com/keaaw/howto/blob/main/lichess-dev.md

## Alternatives

### Docker

[Here](https://github.com/benediktwerner/lichess-docker) is a Docker setup that allows running lila and all services required for it in a Docker container. Running in Docker simplifies setup and guarantees that your development environment will perfectly match that of anyone else who uses that setup, eliminating the "it works on my machine" phenomena. However, it is always more performant to run any project directly on its host machine.

### IntelliJ IDE (instructions need updating)

Here is a guide on how to [set up lila with the IntelliJ IDE](https://github.com/lichess-org/lila/wiki/Lichess-Development-Onboarding-(IntelliJ-on-Linux)).

## Troubleshooting

* ```
  [PrimaryUnavailableException$: MongoError['No primary node is available!']]
  ```
  Make sure mongod is running, check `/var/log/mongo/mongod.log` for errors.
  It might not start if you have too little free space (might need 3GB), or if there is a previous lock file that hasn't been cleaned up (maybe try removing `/var/lib/mongodb/mongod.lock`)

* Can't create games
  ```
  [ERROR] p.c.s.n.PlayDefaultUpstreamHandler Cannot invoke the action
  java.lang.ArrayIndexOutOfBoundsException: 101
  ```
  Check `mongo --version`, and that is satisfies the requirement at the top of this page.

* ```
  java.util.concurrent.TimeoutException: Future timed out after [5 seconds]
  ```
  Check that MongoDB is running. Restart lila, if it was started before MongoDB.
  On OS X, the connection timeout might be needed to be increased initially (5 seconds could be too short for a cold start). See [#6718](https://github.com/lichess-org/lila/issues/6718).

* Mongo error when Lila running
  ```
  [error] reactivemongo.api.Cursor - fails to send request
  reactivemongo.core.errors.DatabaseException$$anon$1: DatabaseException['error processing query: ns=lichess.challenge limit=50Tree: $and
  ```
  or similar excptions due to missing indexes: Run `mongo lichess bin/mongodb/indexes.js` again.

* Mongo error when importing games
  ```
  DatabaseException['cannot insert document because it exceeds 180 levels of nesting' (code = 15)]?
  ```
  In `/etc/mongodb.conf`:
  ```
  setParameter:
    maxBSONDepth: 999
  ```

* `sbt` prints `Killed` and exits

  Most likely there was not enough free RAM to compile lila.