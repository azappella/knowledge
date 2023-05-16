# podman

## install podman on mac os

```
brew install podman
```

Next, create and start your first Podman machine:

```
podman machine init
podman machine start
```
You can then verify the installation information using:
```
podman info
```

## Output from above

```
  ~ podman machine start
Starting machine "podman-machine-default"
Waiting for VM ...
Mounting volume... /Users:/Users
Mounting volume... /private:/private
Mounting volume... /var/folders:/var/folders

This machine is currently configured in rootless mode. If your containers
require root permissions (e.g. ports < 1024), or if you run into compatibility
issues with non-podman clients, you can switch using the following command:

	podman machine set --rootful

API forwarding listening on: /Users/a/.local/share/containers/podman/machine/qemu/podman.sock

The system helper service is not installed; the default Docker API socket
address can't be used by podman. If you would like to install it run the
following commands:

	sudo /opt/homebrew/Cellar/podman/4.5.0/bin/podman-mac-helper install
	podman machine stop; podman machine start

You can still connect Docker API clients by setting DOCKER_HOST using the
following command in your terminal session:

	export DOCKER_HOST='unix:///Users/a/.local/share/containers/podman/machine/qemu/podman.sock'

Machine "podman-machine-default" started successfully
➜  ~ sudo /opt/homebrew/Cellar/podman/4.5.0/bin/podman-mac-helper install
Password:
➜  ~ podman machine stop; podman machine start
Waiting for VM to exit...
Machine "podman-machine-default" stopped successfully
Starting machine "podman-machine-default"
Waiting for VM ...
Mounting volume... /Users:/Users
Mounting volume... /private:/private
Mounting volume... /var/folders:/var/folders

This machine is currently configured in rootless mode. If your containers
require root permissions (e.g. ports < 1024), or if you run into compatibility
issues with non-podman clients, you can switch using the following command:

	podman machine set --rootful

API forwarding listening on: /var/run/docker.sock
Docker API clients default to this address. You do not need to set DOCKER_HOST.

Machine "podman-machine-default" started successfully
```