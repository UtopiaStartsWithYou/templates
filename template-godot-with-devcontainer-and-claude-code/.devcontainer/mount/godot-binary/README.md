# How to Mount Godot Binary into the Devcontainer

> **Note:** Manually download the linux Godot binary and place here, then update the `devcontainer.json` to bind mount it into the Docker Container.

## 1. Download Godot Binary

Download the official `Godot_v4.5-stable_linux.x86_64` binary.

https://godotengine.org/
https://godotengine.org/download/linux/


## 2. Place Binary in Mount Directory

Place the binary under `<host project>/.devcontainer/mount/godot-binary/`

Example:
```
.devcontainer/mount/godot-binary/Godot_v4.5-stable_linux.x86_64
```

## 3. Update devcontainer.json

Update `.devcontainer/devcontainer.json`'s `"mounts": [ .. ]` like this:
```json
// .devcontainer/devcontainer.json
{
	"name": "You've Got Packages - Devcontainer",
	"build": {
		"dockerfile": "Dockerfile"
	},	
	// ..
	"mounts": [
		// ..

		// ---------------------------------------------------------------------------------------------------
		// mount Godot binary
		// ---------------------------------------------------------------------------------------------------
		// "source=${localWorkspaceFolder}/.devcontainer/mount/godot-binary/Godot_v4.4.1-stable_linux.x86_64,target=/usr/local/bin/godot,type=bind,consistency=cached",
		"type=bind,source=${localWorkspaceFolder}/.devcontainer/mount/godot-binary/Godot_v4.5-stable_linux.x86_64,target=/usr/local/bin/godot,consistency=cached"
	]
}
```


> **Tip**
> Make sure you bind mount exactly 1 Godot binary to `/usr/local/bin/godot` in the Docker Container.
>
> You'll need to update the path in `devcontainer.json` when you add new Godot binaries to `.devcontainer/mount/godot-binary/`
>
> Comment your unused versions.


> **Customization**
> If you need to test against multiple versions...
> mount `type=bind,source=<godot3 mount>,target=/user/local/bin/godot3`
> and also `type=bind,source=<godot4 mount>,target=/user/local/bin/godot4`


## How It Works

When you use the bind mount config `target=/usr/local/bin/godot`
Dev Container will mount the host machine's:
```
<Host Machine: Your Supercool Project>/.devcontainer/mount/godot-binary/Godot_v4.5-stable_linux.x86_64
```

into the devcontainer where it's accessible in the PATH.

From there you can run the `godot` command (regardless of version) which will run the host machine's binary found at this location inside the container:
```
/usr/local/bin/godot
```

This allows Claude Code (or you!) to simply run `godot --headless -s <path-to-scene-or-script>` from any directory.
