# workshop-devcontainer

# Part 0: Setup & Exemple
## Step 1: Installation
### Docker
Go to [Docker's website](https://docs.docker.com/engine/install/#server) and follow the instruction specific to your platform.

:exclamation: Don't forget the post-installation steps. :exclamation:

### VSCode

Go to [VSCode](https://code.visualstudio.com/Download) and follow the instruction specific to your platform.

Install the [Devcontainer extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers)

## Step 2: Basic commands & samples

# Part 1: Container configuration

To create a devcontainer all you'll need is config file named devcontainer.json.

## Step 01:
For starters let's try creating one using our newly installed extension.
Open the command palet on VSCode using ctrl+shift+P and start typing Dev Containers, you should see every command related to the DevContainer extension listes here.
For more info on each of those command go there.
For now we'll focus on *Dev Container Configuration Files*.
- Start by selecting a base template (we'll use alpine for now, but feel to try them out later)
  - note that most of those template come already bundled with some tools, in our case with git
 - Select a version
 - Select some additional features like [fish](https://fishshell.com/) for example. -> nothing
Once this is done, you'll see a .devcontainer/devcontainer.json added to your folder.
It comes with one of Microsoft default more info here image https://github.com/devcontainers/template-starter

## Step 02: Run it
Using the Dev Containers: Open folder in container command build the dev container and you're good to go.

## Step 03: Add some features
You can either use the DevContainer Configure ContainerFeatures or go to https://containers.dev/features and add it to the "feature" tag.

## Step 04: Rebuild
To apply the changes you've made to your container you'll need to rebuild it. VsCode will prompt you to do it everytime you make modification to the json file.
Or you can use the Rebuild or Rebuild without cache commands

## Step 05: VsCode Config

### Step 05.1: Extensions
You your vscode instance inside the container using
Let's add some extensions using 
"customizations": {
		"vscode": {
			"settings": {
        "fish": {
            "path": "fish"
        },
      },
			"extensions": ["streetsidesoftware.code-spell-checker", "aaron-bond.better-comments"],
		}
}

### Step 05.2: Settings
Let's add some settings using 
"customizations": {
		"vscode": {
			"settings": {
        "fish": {
            "path": "fish"
        },
      },
			"extensions": ["streetsidesoftware.code-spell-checker", "aaron-bond.better-comments"],
		}
}

# Part 2: Dockerfile


Step XX: Update devcontainer.json
# Resources
https://containers.dev/

https://github.com/devcontainers

https://code.visualstudio.com/docs/devcontainers/containers
