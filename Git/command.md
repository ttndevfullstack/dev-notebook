Git Commands
============
___

_A list of my commonly used Git commands_

--

### Advanced Usage

| Command | Description |
| ------- | ----------- |
| `git maintenance start` | Enable automatic maintenance |
| `git maintenance run` | Run cleanup tasks immediately |
| `git maintenance stop` | Disable automatic maintenance |
| `git fetch --all && git reset --hard origin/$(git rev-parse --abbrev-ref HEAD)` | Reset current branch from remote branch |

### Branching & Merging

| Command | Description |
| ------- | ----------- |
| `git commit --amend -m "<new_message>"` | Update message of latest commit |
| `git push origin --delete [branch name]` | Delete a remote branch |
| `git checkout -b [branch name] origin/[branch name]` | Clone a remote branch and switch to it |
| `git merge [source branch] [target branch]` | Merge a branch into a target branch |

### Sharing & Updating Projects

| Command | Description |
| ------- | ----------- |
| `git push -u origin [branch name]` | Push changes to remote repository (and remember the branch) |
| `git remote add origin ssh://git@github.com/[username]/[repository-name].git` | Add a remote repository |
| `git remote set-url origin ssh://git@github.com/[username]/[repository-name].git` | Set a repository's origin branch to SSH |
| `git remote remove [remote name] ` | Remove a remote repository |

### Inspection & Comparison

| Command | Description |
| ------- | ----------- |
| `git log` | View changes |
| `git log --summary` | View changes (detailed) |
| `git log --oneline` | View changes (briefly) |
| `git diff [source branch] [target branch]` | Preview changes before merging |
| `git diff [pre_branch name]..[cur_branch name]` | Check changed when pull current branch |

### Stash changed and apply changes

| Command | Description |
| ------- | ----------- |
| `git stash push -m "[stash name]"` | Stash changed with specify name |
| `git stash apply stash@{[index]}` | Apply specify stash |
