### bash-functions
#### About
The [bash-functions](/bash-functions) file contains some example functions that can be helpful when working with git. If you have suggestions, additions or fixes, you are encouraged to submit merge requests.

1. `gits [#]` - a function that displays a brief summary of changes and the previous 5 log messages and hash commits. If an optional number is provided, it will display that many log messages.

#### Usage
Copy the contents of `bash-functions` into `~/.bashrc` or copy the file somewhere in your home directory and add something like the following to `~/.bashrc`:

```
if [[ -e "$HOME/.bash_functions" ]]; then
  source "$HOME/.bash_functions"
fi
```

### bash-git_prompt
#### About
The [bash-git_prompt](/bash-git_prompt) file contains some functions and configuration for adding git status information to your current prompt. It is meant as an example, you are encouraged to tweak it to meet your needs and suggest additions, fixes or updates.

#### Usage
You can copy it wholesale into your `~/.bashrc` configuration file, or you can add something like the following to `~/.bashrc` to source a file:

```
if [[ -e "$HOME/.bash_git_prompt" ]]; then
  source "$HOME/.bash_git_prompt"
fi
```

**Note:** If you are using `$PROMPT_COMMAND` in your `~/.bashrc` already, you will need to take some care and adjust the contents of `bash-git_prompt`

#### Symbol meanings:
* `>` - Local branch is ahead of the branch it is tracking
* `<` - Local branch is behind the branch it is tracking
* `!` - Local and remote branches have diverged
* `+` - There is at least 1 local `stash`
* `?` - There are files in the git working directory that are untracked

Example:

![example prompt and gits output](media/example.png)

### git-aliases
#### About
The [git-aliases](/git-aliases) file contains a handful useful git aliases. These can be placed in `~/.gitconfig` (recommended) or within a project's git config (not recommended).

As with everything else, if you have additions, fixes or updates, you are encouraged to share them.

* `co` - Alias for `checkout`, easier and quicker to type
* `last` - Display information about the most recent commit
* `unstage` - Reverse a `git add <file>` with `git unstage <file>`
* `ls` - list all the files currently tracked by git
* `vis` - display a pretty listing of log messages with a graph of the branching
* `today` - (Requires git v1.8+) displays log of commits since yesterday; e.g. today's commits
* `squash` - Does an interactive rebase of the previous # commits; useful for squashing commits
* `stashed` - Display information about stashes
* `history` - Show the history of a file, branch or other ref including commit messages and diff / patch

#### Usage
Copy the aliases from `git-aliases` that you want to use into `~/.gitconfig`
