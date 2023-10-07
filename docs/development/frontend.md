## Getting started

Learn about [**`pnpm` here**](https://github.com/pnpm/pnpm#readme). In most cases, it has the same behavior and syntax as both `npm` and `yarn`. Just as you might use `npm install` or `yarn add` to add a package with those tools, either `pnpm install` or `pnpm add` can be used to do the same with `pnpm`.

Building the client involves [**`tsc`**](https://www.typescriptlang.org/docs/handbook/compiler-options.html) to transpile typescript code to javascript, [**`dart-sass`**](https://sass-lang.com/dart-sass) to compile `*.scss` SASS files into CSS, and [**`esbuild`**](https://esbuild.github.io/) to bundle javascript dependencies into individual modules suitable for linkage in an HTML `<script>` tag.

## Using ui/build

You probably won't have to interface with `dart-sass` or `esbuild` directly. The `ui/build` tool does it for you.

`ui/build --help` is enough to get most people started. The source code [can be found here](https://github.com/lichess-org/lila/tree/master/ui/%40build). It uses basic lichess client conventions and information in custom objects in the `package.json` files in each module to determine how to generate assets.

See the `lichess` object defined in [`ui/site/package.json`](https://github.com/lichess-org/lila/blob/master/ui/site/package.json) for the most comprehensive example. Each line in that `bundles` object identifies a source typescript (relative to the package.json) and a destination basename (relative to `<lila>/public/compiled`) that `esbuild` will use to generate the javascript assets downloaded by the client.

`ui/build` will reconcile all `node_modules` with the `pnpm` package store according to parsed `package.json`s each time it is invoked. This behavior can be suppressed with the `--no-install` argument:

`ui/build --no-install`

## Customizing linked pnpm modules

How about when you need to make changes to a module from the `npm` registry such as `chessground`? For that, you can use `pnpm link`.

Clone the repo you want to modify to `<your-local-package>`. Then `cd` to the _using_ module directory - the one containing the `package.json` you want to link to your package. Finally, type `pnpm link <relative-path-to-your-local-package>`. Run `ui/build` again to pick up your changes.

For example, to work on pgn-viewer: First `git clone https://github.com/lichess-org/pgn-viewer` into a directory adjacent to lila root. We'll use the lichess forum code to test our local pgn-viewer. Forum stuff lives in the `ui/site` module and `lichess-pgn-viewer` is linked in that module's `package.json`. So:

```
cd <lila-root>/ui/site
pnpm link ../../../pgn-viewer # relative path to your local package
```

### IMPORTANT

Notice that `pnpm link `actually modifies the `pnpm-lock.yaml` file in lila root. You don't want to check that modification into git. Therefore:

`git restore <lila-root>/pnpm-lock.yaml`

When you're done with your local package changes. Afterwards, `pnpm` & `ui/build` will use the `npm` registry version again.

## Here are some useful links:

- [Symbols you can use on lichess webpages](https://lichess.org/assets/oops/font.html) (defined in `public/font/lichess.sfd`)
- add some links
