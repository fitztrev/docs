# Running spamdb - [Download from GitHub](https://github.com/lichess-org/lila-db-seed/)

[python3.9+](https://www.python.org/) and the pymongo module are both required. If you don't have python3, use your package manager or the [downloads page](https://www.python.org/downloads/) to install it, then get pip3 and pymongo with command line:

```
python -m ensurepip --upgrade
# NOTE - On windows it might be "py -m ensurepip --upgrade"
pip3 install pymongo
```

The lila-db-seed/spamdb/spamdb.py script can do a few things.  Usage help:

```
cd spamdb
python3 ./spamdb.py --help
```
Usually, the script will generate a new set of data from inputs in the provided arguments as well as the spamdb/data directory.  This data will be merged into your running mongodb instance at 127.0.0.1:27071 by default.  To customize connection details use the --uri argument.  Set the password for your users with the --password flag (otherwise they will default to "password").  Set the default background in user prefs with --user-bg (default is dark mode, use 400 for transparency).  For other options see spamdb.py --help.  Add, remove, or modify entries to the various .txt files in the data directory if you want to customize fluffy stuff.  

### Do consider editing uids.txt to give the mod users different passwords than the default if your dev instance will be exposed to others.

## Special users:

- lichess - ROLE_SUPER_ADMIN
- admin - ROLE_ADMIN 
- shusher - ROLE_SHUSHER
- cheat-hunter - ROLE_CHEAT_HUNTER
- timeout-mod - ROLE_TIMEOUT_MOD
- puzzle-curator - ROLE_PUZZLE_CURATOR
- api-hog - ROLE_API_HOG   (this guy is useful for api testing, both server and clients)
- troll - marked as troll
- bot - marked as bot
- kid - they're just children, how could you checkmate children?
- wide - 20 W's in visible username, WGM title, and a patron to test css formatting for extremely wide usernames.
- and assorted others, student, coach, see spamdb/modules/user.py

## Normal users:

The normal users have all the data.  This includes notifications, ratings, follows, game histories, activity, timelines, blogs, forums, teams, tournaments.  Their usernames can be found and customized in data/uids.txt.  Specify user/password to set individual passwords

## Caveats:
There are no indices for game or forum search yet.  This will be fixed never/soon.
