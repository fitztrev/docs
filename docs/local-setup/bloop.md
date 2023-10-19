# Bloop for hacking lila scala code

### NOTE - Much of this section is out of date. Sbt is now a metals build server, and many prefer it to bloop.

[bloop](https://scalacenter.github.io/bloop/) is a build server. It's the best way to work on lila.

Install it with your package manager and create a userland systemd service for it.

```
systemctl --user enable bloop
systemctl --user start bloop
```

After being set up (`sbt bloopInstall`),
bloop can be used to build lila (`bloop compile lila`)
and run it (`bloop run lila -m lila.app.Lila -c /path/to/lila/.bloop`).

I like to run my local lila as [a systemd service](https://github.com/ornicar/dotfiles/blob/master/systemd/lila.service),
and to restart it [by pressing F1](https://github.com/ornicar/dotfiles/blob/bdd905c72db104f8aea354c535c74f46377a3604/i3/config#L38).
I also have [a service notifying me](https://github.com/ornicar/dotfiles/blob/master/systemd/lila-watch.service)
when [the restart completes or fail](https://github.com/ornicar/dotfiles/blob/master/scripts/lila-watch).

You can interface bloop with your text editor using [scala metals](https://scalameta.org/metals/).
You gain full IDE support: incremental compilation, warning and error reports, jump to definition, global renaming, etc.
This supports vim, emacs, atom, visual studio, sublime and more. I highly recommend using it.

For details about my setup and vim configs, checkout my [dotfiles repository](https://github.com/ornicar/dotfiles).

![My vim setup for hacking lila scala code](https://i.imgur.com/wVGKrjM.png)

bloop will also happily compile and run [scalachess](https://github.com/ornicar/scalachess),
[lila-ws](https://github.com/ornicar/lila-ws), [lila-fishnet](https://github.com/ornicar/lila-fishnet)
and probably any scala project.

## Known issues

- bloop doesn't know about translation xmls. When one is modified, you need to do `bloop clean i18n` from the project root and restart lila to pick it up.
- bloop doesn't watch and recompile playframework routes. Until we find a way to fix that (can you help?), the workaround after editing `conf/routes` is to run `sbt playRoutes`.
- The bloop CLI [does not currently install via homebrew](https://github.com/scalacenter/bloop/issues/1467) on M1 Macs. However, you should still be able to install bloop (non-CLI) by installing Metals with VSCode. Note that you will not be able to run bloop commands from the terminal.
