### Scala Metals (IDE helper):

1. In VS Code, open this `lila-docker` project and install the [Dev Containers extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers)
2. Cmd+Shift+P > "Dev Containers: Reopen in Container"
3. A new VS Code window will open, attached to the container instead of your host machine
4. File > Open Folder > "/workspaces/lila-docker/repos/lila" (or whichever Scala project you want to work on)
5. Install + Enable the Scala Metals extension (Cmd+Shift+X > "Scala (Metals)")
6. Cmd+Shift+P > "Metals: Import build"

Once the build has been imported, you should have code completion, go to definition, etc when you open a Scala file.
