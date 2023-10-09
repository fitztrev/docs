# Onboarding (MacOS)

Follow the [Linux setup guide](index.md). Please note the following differences:

Consider using the [homebrew](https://brew.sh/) package manager to install most lila dependencies, but install coursier and scala using [these instructions](https://i.imgur.com/3MQ6zr7.jpg). Run `cs update` from time to time to keep your sbt tools up to date.

# Lila

set
`play.server.netty.transport = "jdk"` in `base/application.conf`

# lila-ws

## Apple M1

Remove this line from [build.sbt](https://github.com/ornicar/lila-ws/blob/master/build.sbt):

```diff
-libraryDependencies += "org.reactivemongo"           % "reactivemongo-shaded-native"  % s"$reactivemongoVersion-$os-x86-64"
```
