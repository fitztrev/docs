# lila-gitpod

You can use [Gitpod](https://gitpod.io) (a free, online, VS Code-like IDE) for creating a development site of [Lichess.org](https://lichess.org). With a single click, it will launch a workspace and automatically:

- Clone the necessary Lichess repositories
- Install all the dependencies
- Seed your database with test data
- Start a development site

![lila-on-gitpod](https://user-images.githubusercontent.com/271432/183785811-dc00e385-f13f-4226-9654-93b6465c75cb.png)

## What do you need?

1. A [Github](https://github.com) account
2. A web browser (like Chrome or Firefox)

## Getting Started

Click here to start a workspace:

[![Open in Gitpod](https://gitpod.io/button/open-in-gitpod.svg)](https://gitpod.io/#https://github.com/lichess-org/lila-gitpod)

It will take about 8-10 minutes. You can watch the progress in the terminal windows that automatically open.

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

## Stopping Your Workspace

Done for the day or make a mistake and want to start over? No problem.

    gp stop

Or just leave it and it'll shut down when it's been idle for 30 minutes.
