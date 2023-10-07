# User interface
The mithril or snabbdom UI modules are in `ui/`. Say we work on `ui/round`, the playing UI.

Enable auto-recompile:

```
cd ui/round
yarn run dev --watch
```

To enable auto-recompile for the .scss files:

```
cd ui
yarn run gulp css
```

# Translations

1. New translation keys are added in `translation/source/site.xml` in British English.
1. Then regenerate translation keys for Scala: `bin/trans-dump`
1. Do not touch `translations/dest/`. New translations from crowdin will automatically be applied here.

To make a new translation key available to the client JS, [add the key to the appropriate page's key list](translations.md#using-translations-in-javascript).

If you need to add a new file to `translation/source` (for example to translate a completely new page), you also need to add the name of the file (without the extension):

In `bin/trans-dump`:
[here](https://github.com/lichess-org/lila/blob/master/bin/trans-dump#L10)

In `/build.sbt`:
[here](https://github.com/ornicar/lila/blob/3b38ffdbbedc2730746becbb4a4f076ddfb5274c/build.sbt#L79)

For an in-depth explanation of how the whole translation system works, see [How translations work](translations.md).

# Puzzles

Import [some puzzle data](https://github.com/lichess-org/lila-db-seed).

# Forum

[Create the default categories](https://gist.github.com/kraktus/7a60362b6af362bb1bea0c1b6a212d15).

# Swiss

You need to install and run [bbpPairings](https://github.com/cyanfish/bbpPairings). Mind that Lichess uses [cyanfish](https://github.com/cyanfish) fork.

To set up a swiss tournament, you need to be a team leader. For the Lichess one's to be working properly it's advised to [create the team of the official Lichess swiss](https://gist.github.com/kraktus/92e302ed9fad705cd62d5c30343a5a95).

# Insights

1. [Download insights sample data](https://github.com/lichess-org/lila/files/6807098/insight.bson.zip).
2. run
```
mongorestore --db lichess-insight --collection insight insight.bson
```
3. Register an account named `thibault`.
4. Either play one rated game with it or hack [this function](https://github.com/ornicar/lila/blob/6048f3c4e223357ea99ed84ea4f0a82f251eb2fe/modules/insight/src/main/InsightApi.scala#L44).
5. Access to `http://localhost:9663/insights/thibault/`

# Font icon to SVG

Extract all fonts as SVG
```fontforge -lang=ff -c 'Open($1); SelectWorthOutputting(); foreach Export("svg"); endloop;' lichess.sf```
Where `lichess.sfd` is https://github.com/ornicar/lila/blob/master/public/font/lichess.sfd

# Accessibility

To activate Accessibility mode on lichess, press `tab` then `enter` on the homepage.

Most of the code is located in `ui/nvui`, and it is then loaded as a plugin.