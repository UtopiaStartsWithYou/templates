# Godot Dev Container Template with Claude Code

## Purpose
A template repository for Godot development in VS Code Dev Containers with:

- **Persistent Claude Code** — authentication and installation survive container rebuilds
- **Mounted Godot binary** — run `godot --headless` from within the container to run `.gd` and `.tscn` files

---

## ⚠️ Security Notice

**Never commit or share your `<project>/.claude/` directory.** It contains authentication tokens.

```gitignore
# .gitignore
.claude/mount/

#------------------------------------------------------------------------------
# ⚠️ NEVER COMMIT THESE -- They contain auth tokens / passwords / user ids! ⚠️
#------------------------------------------------------------------------------
.claude/mount/home/(dot)claude/.claude.json
.claude/mount/home/(dot)claude/.claude.json.backup
.claude/mount/home/(dot)claude/.credentials.json
#------------------------------------------------------------------------------
# ⚠️ NEVER COMMIT THESE -- They contain auth tokens / passwords / user ids! ⚠️
#------------------------------------------------------------------------------

```

---

## Requirements
- `git`
- `VS Code`
- `Docker`

---

## Full Guide

**Godot Setup**
[.devcontainer/mount/godot-binary/README.md](.devcontainer/mount/godot-binary/README.md)

**Claude Setup**
[.claude/mount/home/(dot)claude/HOWTO.md](.claude/mount/home/(dot)claude/HOWTO.md)

---

## Quick Start

1. **Copy Template Directory**
- In your host machine, copy this template folder and rename it for your project
2. **Initialize git**:
   ```bash
   git init
   ```
3. **Download Godot** 
- Place the Linux binary (e.g. `Godot_v4.5-stable_linux.x86_64`) in `.devcontainer/mount/godot-binary/`
- Update `devcontainer.json`'s `"mount"` key

4. **Open this project in a Dev Container (uses Docker)**
   - Open this folder in VS Code
   - Click bottom-most left-most `><` button ➡️ "Reopen in Container"
5. **Install Claude Code**
   - Once container is built, run these commands in bash(inside the container):
   ```bash
   cd ~/.claude
   mkdir local
   cd local
   npm install @anthropic-ai/claude-code
   cd ..
   ln -s ~/.claude/local/node_modules/.bin/claude ~/.claude/local/claude
   ```
6. **Reload shell**:
   ```bash
   source ~/.bashrc
   ```
7. **Done!** Run `claude` to start.

> **Verify**
> Once you're all setup, try running
> - `godot --headless -s /workspaces/template-godot-with-devcontainer-and-claude-code/test_godot.gd`
>
> Replace `template-godot-with-devcontainer-and-claude-code` with your host folder name if you already renamed the template folder to something like `my-super-cool-project/`

---

## How It Works

This template uses Docker bind mounts to persist data outside the container.

### Devcontainer Setup

| Host | Purpose |
|------|---------|
| `.devcontainer/devcontainer.json` | Container + bind mount configuration |
| `.claude/mount/home/(dot)bashrc` | Shell config (`CLAUDE_CONFIG_DIR`, PATH) |

### Claude Code Persistence

| Host | Container |
|------|-----------|
| `.claude/mount/home/(dot)claude/` | `~/.claude/` |

This preserves across rebuilds:
- Authentication/login session
- Local Claude Code installation (`~/.claude/local/`)
- Chats and config files


### Godot Binary

| Host | Container |
|------|-----------|
| `.devcontainer/mount/godot-binary/Godot_v4.5-stable_linux.x86_64` | `/usr/local/bin/godot` |

Update the mount path in `devcontainer.json` when upgrading Godot versions:

```json
"mounts": [
    // ... other mounts ...
    "type=bind,source=${localWorkspaceFolder}/.devcontainer/mount/godot-binary/Godot_v4.5-stable_linux.x86_64,target=/usr/local/bin/godot,consistency=cached"
]
```
