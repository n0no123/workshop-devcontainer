# workshop-devcontainer

# Part 0: Setup
## Step 0: Star this repository

## Step 1: Installation
### Docker
Go to [Docker's website](https://docs.docker.com/engine/install/#server) and follow the instruction specific to your platform.

:exclamation: Don't forget the post-installation steps. :exclamation:

### VSCode

Go to [VSCode](https://code.visualstudio.com/Download) and follow the instruction specific to your platform.

Install the [Devcontainer extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers)

## Step 2: Basic commands & samples
Open VSCode's command palette and start typing *try a*, then select **>Dev Containers: Try a Dev Container Sample**
>
*You can open the Command Palette using* <kbd>ctrl</kbd> + <kbd>⇧ shift</kbd> + <kbd>P</kbd>

You can then quit the container using the **>Dev Containers: Reopen folder locally** command.

# Part 1: Container configuration

To create a devcontainer all you'll need is config file named **devcontainer.json**.

## Step 01:
For starters let's try creating one using our newly installed extension.<br>
Open the command palet on VSCode using <kbd>ctrl</kbd> + <kbd>⇧ shift</kbd> + <kbd>P</kbd> and start typing *Dev Containers*, you should now see every command related to the DevContainer extension.

For now we'll focus on the **>Dev Containers: Add Dev Container Configuration Files**.

- Start by selecting a base template (we'll use debian for now, but feel to try out the others later)<br>
  - *note that most of those template come already bundled with some tools, in our case with git*
 - Select a version (*bullseye*)
 - Select some additional features like [fish](https://fishshell.com/).

Once this is done, you'll see a .devcontainer/devcontainer.json added to your folder.

## Step 02: Run it
Using the **>Dev Containers: Open folder in Container** command, build and run the dev container.

## Step 03: Add some features
Dev Containers are easily customizable and come with a wide range of already configured features.

You can either use the **>DevContainers: Configure Container Features** command or go to https://containers.dev/features and copy and paste the desired feature within the feature field.

Either way, it should look something like that:

```json
	"features": {
		"ghcr.io/meaningful-ooo/devcontainer-features/fish:1": {}
	}
```

## Step 04: Rebuild
To apply the changes you've made to your container you'll need to rebuild it.<br>
VsCode will prompt you to do it everytime you make modification to the json file.<br>
Or you can use the **>DevContainers: Rebuild Container** command.

## Step 05: VsCode Config
You can customize your vscode instance inside the container, either by adding extensions or changing the settings of VsCode.

For that, we'll use the customizations -> vscode fields.

Like so:
```json
	"customizations": {
	    "vscode": {}
	}
```

### Step 05-1: Extensions
Firstly, let's add some extensions, for that you will need to get the ids of the extensions you would like to add.

*Go to the extension tab, and right click on any extension, then click on Copy Extension ID*

```json
	"customizations": {
		"vscode": {
			"extensions": [
				"aaron-bond.better-comments"
			],
		}
	}
```

### Step 05-2: Settings

You can also change the settings of VsCode inside the container, doing-so only affects the settings of your container, it will not change your local configuration.

For exemple, if we have [fish](https://fishshell.com/) installed, you could make it the default shell when creating a new integrated terminal with the following snippet:

*Go to settings, for that you can use <kbd>ctrl</kbd> + <kbd>,</kbd> then select the setting you wish to override in the container, then click on more action or use <kbd>⇧ shift</kbd> + <kbd>F9</kbd> and click on Copy Settings as JSON.*

You can then add it to the devcontainer.json like that:

```json
	"customizations": {
		"vscode": {
            "extensions": [
				...
			],
			"settings": {
				"terminal.integrated.defaultProfile.linux": "fish"
			}
		}
	}
```

# Part 2: Dockerfile
## Step 00: Dockerfile
Create a file called **Dockerfile** in the same folder as your **devcontainer.json** file.

## Step 01: The Base Image
Using the FROM statement, you can specify a base image for Docker to use, for this workshop we'll go with Debian.

```Dockerfile
FROM debian:latest
```

## Step 02: Adding a non root user

To make it easier to manage, we will add a user inside our container.

```Dockerfile
ARG USERNAME=user-name-goes-here
ARG USER_UID=1000
ARG USER_GID=$USER_UID

RUN groupadd --gid $USER_GID $USERNAME \
    && useradd --uid $USER_UID --gid $USER_GID -m $USERNAME

USER $USERNAME
```

## Step 03: add root privilege
Sometimes, you need to have root privileges

```Dockerfile
ARG USERNAME=user-name-goes-here
ARG USER_UID=1000
ARG USER_GID=$USER_UID

RUN groupadd --gid $USER_GID $USERNAME \
    && useradd --uid $USER_UID --gid $USER_GID -m $USERNAME
    && apt-get update \
    && apt-get install -y sudo \
    && echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME \
    && chmod 0440 /etc/sudoers.d/$USERNAME

USER $USERNAME
```

## Step 04: Add the package you need
You don't want to have to manually reinstall your packages everytime you rebuild the container.

To prevent that from happening you can add a RUN Instruction to your Dockerfile with the following :
```Dockerfile
RUN sudo apt-get update && apt-get install -y your package list
```

## Step 05: Update devcontainer.json
Lastly, you need to tell your dev container to use the image from your Dockerfile, go to to your devcontainer.json file and replace the image instruction with the following:

```json
    "build": {
        "dockerfile": "Dockerfile",
        "context": ".."
    },
```

# Resources
https://containers.dev/

https://github.com/devcontainers

https://code.visualstudio.com/docs/devcontainers/containers
