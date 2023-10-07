# Lichess Development Documentation

[Lichess.org](https://lichess.org) is an open-source chess server. This documentation is for developers who want to contribute to the project.

## Code + Architecture

The main Lichess code is written in [Scala](https://www.scala-lang.org/) and Typescript. Some of the supporting services are written in [Rust](https://www.rust-lang.org/).

For a full list of repositories and to see an architecture diagram, see [lichess.org/source](https://lichess.org/source).

!!! info "What is lila?"

    Lichess is primarily written in the Scala programming language. The name for the main Lichess codebase is "lila", which stands for "**_Li_**chess in Sca**_la_**".

## Ways you can contribute

- For non-code ways you can contribute, see [lichess.org/help/contribute](https://lichess.org/help/contribute)
- See Github issues that we tagged as ["good first issue"](https://github.com/orgs/lichess-org/projects/4)
- See what others are working on or offer help in the `#lichess-dev-onboarding` channel on the [Lichess Discord](https://discord.gg/lichess)

## Setting up a development environment

There are 3 ways you can run a development environment, with increasing complexity:

1. [lila-gitpod](lila-gitpod/index.md) - Will create a development environment in a remote workspace. This doesn't require any setup on your local machine. Try this to get started quickly and see what a development environment looks like on a fresh install.
2. [lila-docker](lila-docker/index.md) - Run Lichess locally in Docker containers.
3. Install the dependencies and run all the services on your local machine. See [Local Setup](local-setup/index.md) for instructions.

## Competence development program

Lichess would like to support its contributors in their competence development by covering costs of relevant training materials and activities. This is a small way to further empower contributors who have given their time to Lichess and to enable or improve additional contributions to Lichess in the future. For more information, including how to apply, check [Competence Development for Lichess contributors](https://lichess.org/page/competence-development).
