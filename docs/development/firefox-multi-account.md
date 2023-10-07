Most Lila features outside of puzzles and play with computer require interaction between multiple users. Anything that facilitates switching between them is useful for developers.

Use Firefox's [Multi-Account Container extension](https://addons.mozilla.org/en-US/firefox/addon/multi-account-containers/) to have multiple active users logged in to a lila instance, all in their own tabs with their own session environments (cookies, local storage, etc), without using private browsing. More importantly, if you set up a few containers dedicated to specific users, you'll never have to log that user in again. Just select the user/container from the menu and let Firefox/cookie/lila handle the rest in a newly created tab.

This sample Multi-Account Container menu associates containers with spamdb users. The sessions in each container persist across tab closure and browser restarts - just like your default session:

<img align="center" width="480" alt="Screen Shot 2022-05-06 at 1 21 07 AM" src="https://user-images.githubusercontent.com/101470903/167077893-339af997-3f66-4d02-a612-97fa559510e7.png">

---

To make the tabs reflect the username, go to the `st.headTitle` block in the `head` tag of the `apply` method in `app/views/base/layout.scala` and make it look like this:

```scala
st.headTitle:
  if ctx.blind then "lichess"
  else if netConfig.isProd then fullTitle | s"$title • lichess.org"
  else ctx.me.so(_.username)
```

Don't check that in by mistake.

To further distinguish them, you can use [`spamdb.py`](../local-setup/db.md) `--user-bg 400` option to create users with a random background image selected from `data/image-links.txt`
