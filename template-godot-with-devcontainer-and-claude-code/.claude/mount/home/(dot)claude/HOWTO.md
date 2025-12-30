
**Claude Code -- Dev Container Setup -- Persistent Login**

> **Warning:** Never share this custom `<host project>/.claude/mount/` , and add it to your `.gitignore`  
>
> It contains your Claude Code login information & authentication tokens.

# TLDR

<details>
    <summary>
    Short Guide.
    </summary>

1) copy paste repo to destination in host machine, give it a new name for your project
2) `git init` to initialize the folder into a git repo
3) spin up Dev Container in VS Code
4) run the following to install Claude Code


**Install Recipe**

```bash
cd ~/.claude
mkdir local
cd local
npm install @anthropic-ai/claude-code
cd ..
ln -s ~/.claude/local/node_modules/.bin/claude ~/.claude/local/claude
```


5) Then run `source ~/.bashrc` or <br> start a new terminal and you're good to go!

Even if you rebuild your container, as long as you don't touch the files in your host `<host project>/.claude/` directory, your Claude Code installation and login will survive rebuilds.

Be careful not to share your login tokens!

</details>

---

# How To Setup Claude Code for Devcontainer

## 1. Setup Persistent Storage Folder

We'll setup a folder in the host machine to store our Claude Code install and settings. That way if we rebuild or restart our Dev Container / Docker Image, we won't have to re-login every time.

**How it works:**

Host machine repo:
```
<host project>/.claude/mount/home/(dot)claude/
```

becomes this in the Debian container (via a Docker "bind mount"):
```
~/.claude/
```

**Configure via:**
```
<host project>/.devcontainer/devcontainer.json > "mounts" key
```

## 2. Configure ~/.bashrc

**2a)** Ensure this `.bashrc` in the host is bind mounted to `~/.bashrc`

**2b)** Ensure it sets:
```bash
export CLAUDE_CONFIG_DIR=/home/<your main login user>/.claude
```

## 3. Reopen Project in Dev Container

Reopen your VS Code project in a Dev Container (it'll automatically build / spin up a container if needed)

## 4. Run Installation Commands


**What it does**
This block will install claude to `<host project>/.claude/mount/home/(dot)claude/local` -- that way it survives subsequent Docker Container builds:

**Persistent Claude Code Install** 
Run these bash commands inside the container:

```bash
cd ~/.claude
mkdir local
cd local
npm install @anthropic-ai/claude-code
cd ..
ln -s ~/.claude/local/node_modules/.bin/claude ~/.claude/local/claude
```

## 5. Reload and Finish

Reload your terminal or run `source ~/.bashrc`, and you're good to go!

Even if you rebuild the container, you'll still be logged in.
