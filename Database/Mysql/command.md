# MySQL Commands

---

_A list of my commonly used Mysql commands_

--

### Check parameters

| Command                                                                                                | Description                                                                              |
| ------------------------------------------------------------------------------------------------------ | ---------------------------------------------------------------------------------------- |
| `show global status like '%innodb_buffer_pool_read_requests%';`                                        | Check total number read request to Buffer Pool (Cache)                                   |
| `show global status like '%innodb_buffer_pool_reads%';`                                                | Check total number read request to Disk (Not found in Buffer Cache of Memory)            |
| `(innodb_buffer_pool_read_requests - innodb_buffer_pool_reads) * 100/innodb_buffer_pool_read_requests` | Calculate Buffer Cache Hit Ratio (% < 90) => Read from disk > 10% => Need to Optimize    |
| `show global status like '%Open_tables%';`                                                             | Check total table is opened in Cache                                                     |
| `show global status like '%Opened_tables%';`                                                           | Check total table is opened                                                              |
| `(Open_tables / Opened_tables) * 100`                                                                  | Calculate Table Cache Hit Ratio (% < 80) => Need to Optimize                             |
| `show variables like 'table_open_cache';`                                                              | Check parameter max table open in cache                                                  |
| `show global status like '%Open_table_definitions%';`                                                  | Check total table definition is requested in Cache                                       |
| `show global status like '%Opened_table_definitions%';`                                                | Check total table definition is requested                                                |
| `(Open_table_definitions / Opened_table_definitions) * 100`                                            | Calculate Table Definition Cache Hit Ratio (% < 80) => Need to Optimize                  |
| `show variables like 'table_definition_cache';`                                                        | Check parameter max table open in cache                                                  |
| `show global status like '%Created_tmp_disk_tables%';`                                                 | Check total number temp table is request to Disk                                         |
| `show global status like '%Created_tmp_tables%';`                                                      | Check total number temp table is request                                                 |
| `(Created_tmp_disk_tables - Created_tmp_tables) * 100/Created_tmp_disk_tables`                         | Calculate Temporary Table Hit Ratio (% < 80) => Read from disk > 20% => Need to Optimize |

### Branching & Merging

| Command                                              | Description                                             |
| ---------------------------------------------------- | ------------------------------------------------------- |
| `git branch`                                         | List branches (the asterisk denotes the current branch) |
| `git branch -a`                                      | List all branches (local and remote)                    |
| `git branch [branch name]`                           | Create a new branch                                     |
| `git branch -d [branch name]`                        | Delete a branch                                         |
| `git push origin --delete [branch name]`             | Delete a remote branch                                  |
| `git checkout -b [branch name]`                      | Create a new branch and switch to it                    |
| `git checkout -b [branch name] origin/[branch name]` | Clone a remote branch and switch to it                  |
| `git branch -m [old branch name] [new branch name]`  | Rename a local branch                                   |
| `git checkout [branch name]`                         | Switch to a branch                                      |
| `git checkout -`                                     | Switch to the branch last checked out                   |
| `git checkout -- [file-name.txt]`                    | Discard changes to a file                               |
| `git merge [branch name]`                            | Merge a branch into the active branch                   |
| `git merge [source branch] [target branch]`          | Merge a branch into a target branch                     |
| `git stash`                                          | Stash changes in a dirty working directory              |
| `git stash clear`                                    | Remove all stashed entries                              |
| `git stash pop`                                      | Apply latest stash to working directory                 |

### Sharing & Updating Projects

| Command                                                                           | Description                                                 |
| --------------------------------------------------------------------------------- | ----------------------------------------------------------- |
| `git push origin [branch name]`                                                   | Push a branch to your remote repository                     |
| `git push -u origin [branch name]`                                                | Push changes to remote repository (and remember the branch) |
| `git push`                                                                        | Push changes to remote repository (remembered branch)       |
| `git push origin --delete [branch name]`                                          | Delete a remote branch                                      |
| `git pull`                                                                        | Update local repository to the newest commit                |
| `git pull origin [branch name]`                                                   | Pull changes from remote repository                         |
| `git remote add origin ssh://git@github.com/[username]/[repository-name].git`     | Add a remote repository                                     |
| `git remote set-url origin ssh://git@github.com/[username]/[repository-name].git` | Set a repository's origin branch to SSH                     |
| `git remote remove [remote name] `                                                | Remove a remote repository                                  |

### Inspection & Comparison

| Command                                         | Description                            |
| ----------------------------------------------- | -------------------------------------- |
| `git log`                                       | View changes                           |
| `git log --summary`                             | View changes (detailed)                |
| `git log --oneline`                             | View changes (briefly)                 |
| `git diff [source branch] [target branch]`      | Preview changes before merging         |
| `git diff [pre_branch name]..[cur_branch name]` | Check changed when pull current branch |

### Stash changed and apply changes

| Command                            | Description                     |
| ---------------------------------- | ------------------------------- |
| `git stash list`                   | List all stash                  |
| `git stash`                        | Stash changed with random name  |
| `git stash apply pop`              | Apply last stash                |
| `git stash push -m "[stash name]"` | Stash changed with specify name |
| `git stash apply stash@{[index]}`  | Apply specify stash             |
