# Onboarding (Windows)

The simplest way to run lila on Windows is to simply use the [Docker image](https://github.com/benediktwerner/lichess-docker). It will set up and run all programs and services required for lila inside a container. You can then read the [Linux instructions](https://github.com/ornicar/lila/wiki/Lichess-Development-Onboarding) for instructions on how to build and run lila itself and additional Lichess services like fishnet and for further instructions on seeding your database, gaining admin access, troubleshooting, etc.

An alternative is [enabling the Windows Subsystem for Linux](https://docs.microsoft.com/windows/wsl/install-win10), installing an operating system, and again following the [Linux instructions](https://github.com/ornicar/lila/wiki/Lichess-Development-Onboarding).

If you're running Ubuntu using WSL, then you'll run into problems when installing some of the required packages because `apt-key` is bugged in WSL. But there's a workaround - all `apt-key` commands you come across have to be rewritten in the way as described in this issue comment: https://github.com/Microsoft/WSL/issues/3286#issuecomment-395980867

You may run into some problems with redis and mongodb using WSL. [Follow these instructions for redis](https://redislabs.com/blog/redis-on-windows-10/) and [these instructions for mongodb](https://github.com/michaeltreat/Windows-Subsystem-For-Linux-Setup-Guide/blob/master/readmes/installs/MongoDB.md)
