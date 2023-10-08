# Onboarding (MacOS)

Note that development on macOS is not officially supported by the core team, but several community members have done it successfully.

Follow the [Linux setup guide](index.md). Please note the following differences:

It is recommended to use the [homebrew](https://brew.sh/) package manager. Most dependencies can be installed from there.

# Lila

set
`play.server.netty.transport = "jdk"` in `base/application.conf`

# lila-ws

## Apple M1

Remove this line from [build.sbt](https://github.com/ornicar/lila-ws/blob/master/build.sbt):

```diff
-libraryDependencies += "org.reactivemongo"           % "reactivemongo-shaded-native"  % s"$reactivemongoVersion-$os-x86-64"
```

Note that not all features of lila-ws have been thoroughly tested on mac, but it seems to work well enough for testing basic pvp gameplay.
