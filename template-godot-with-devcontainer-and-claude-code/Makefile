# Makefile

.PHONY: git_history_stop_tracking git_history_start_tracking

git_stop_tracking_bash_history:
	@echo "Stopping git tracking of .bash_history..."
	@git update-index --skip-worktree .devcontainer/mount/home/.bash_history
	@echo "✓ .bash_history will no longer appear in git status/diffs"

git_start_tracking_bash_history:
	@echo "Resuming git tracking of .bash_history..."
	@git update-index --no-skip-worktree .devcontainer/mount/home/.bash_history
	@echo "✓ .bash_history is now tracked again"
