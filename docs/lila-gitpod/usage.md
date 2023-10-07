## Your Development Site

To get the URL for your development site, in a new terminal, type:

    gp url 8080

Once lila is running, your dev site will be available. You can check the status on the PORTS tab above your terminal.

## Test accounts

Your database is preloaded with a number of test accounts:

    lichess  /  password   ROLE_SUPER_ADMIN
    admin    /  password   ROLE_ADMIN
    bobby    /  password
    mary     /  password

To see all other test accounts (admins, titled accounts, flagged accounts, bots, etc):

    mongosh lichess --quiet --file /workspace/lila-gitpod/scripts/mongodb/users.js

## Navigating the code

The main Lichess code repositories have automatically been added to your workspace. You can find them at:

    ls /workspace

To open them in your workspace:

    open -r /workspace/lila      # open main lila repository
    open -r /workspace/lila-ws   # open websocket repository

## Recompiling lila

When you edit a scala file, you'll have to restart `bloop`. Go to the `lila` terminal, type `Ctrl+c` to stop the running process, then rerun:

    bloop run lila -m lila.app.Lila

Bloop will detect any changed files, recompile them, then start lila.

## Recompiling frontend assets

If you're working on TypeScript or SCSS files, `./ui/build -w` will automatically detect the changes and recompile the assets. You can see the output in the `ui` terminal.

## Updating Routes

If you edit the `conf/routes` file, you'll need to update the route cache. Bloop will not automatically detect changes to this file.

Stop lila (see [Recompiling lila](#recompiling-lila) above), then run:

    cd /workspace/lila
    ./lila playRoutes

## Stopping Your Workspace

Done for the day or make a mistake and want to start over? No problem.

    gp stop

Or just leave it and it'll shut down when it's been idle for 30 minutes.
