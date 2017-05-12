### Getting Started with GitLab

1. Generate an SSH keypair for GitLab (optional)
  * `ssh-keygen -t rsa -b 2048 -f ~/.ssh/gitlab-ce`

1. Add your SSH public key to your account on GitLab:
  1. Browse to: https://gitlab.example.com/profile/keys
  1. Click Add SSH Key (top-right)
  1. Set the title to something meaningful
  1. Copy your public key and paste it into the Key field
  1. Click Add Key

1. Add an entry for GitLab in your ~/.ssh/config file (Optional but very convenient)
  * This will allow you to type something like `git clone gitlab-ce:namespace/project.git` without needing to pass a username, fqdn, or ssh identity to git

```
Host  gitlab-ce
  User              git
  Hostname          gitlab.example.com
  IdentityFile      ~/.ssh/gitlab-ce
```

### Cloning a Repository

Once you've set up SSH, cloning a repository hosted on GitLab is pretty simple and straightforward.

The structure of a repository's URI is: $host:$namespace/$project.git (example: gitlab-ce:admins/git-tips.git).

Determine where you want to clone the repository; a directory like ~/projects/ works pretty well, but it really doesn't matter; wherever you are most comfortable working.

  * Change to your projects directory (e.g. `cd ~/projects`)
  * `git clone -o cpbills gitlab-ce:cpbills/git-tips.git`

That's it.

You should now have a copy of the Git Tips repository in ~/projects/git-tips (or wherever, you non-conformist, you...) It is cloned and the only remote repository (simply 'remote' for short) it knows about is named 'admins' (-o <name> gives the remote a name, the default is origin).

### Branch Workflow

In a branch-style workflow, contributors either maintain long-lived personal branches of a project, or create (and destroy) shorter lived branches for bringing in new features or fixing bugs.

Creating a new branch:

```
cd ~/projects/git-tips
git checkout -b foo     # create and checkout the branch 'foo'
```

You can change between branches by typing `git checkout <name_of_branch>`.

```
git checkout foo          # Change to local branch 'foo'
git merge --no-ff master  # Merge changes from 'master into the currently checked out branch; Creates a merge commit
git rebase master         # Rebase current branch on the commit master points to;
                          # e.g. Apply commits from master, then apply current branch commits
```

### Fork Workflow

The fork-style workflow is more complicated than the branch-style workflow because it necessitates working with multiple remotes, but is easier for individual contributors to work with, since you have a long-lived development environment set up.

Forking creates a clone of a particular project in your own personal name-space. This allows you to do your development work out of the way of anyone else, and work on features and bug fixes until they are ready to be shared with others.

The fork-style workflow is useful when there are numerous contributors managing long-lived branches on the main repository.

"Rewriting history" using tools like `git rebase` is a much smaller issue when working with a personal fork.

Forking a repository on GitLab CE:

* Make sure you're logged in to your GitLab CE instance
* Browse to the repository you want to fork
* Click the button in the top right that says 'Fork'
* Select the namespace you want to create the fork in

Getting set up with your fork and multiple remotes:

```
git clone -o foo gitlab-ce:foo/git-tips.git           # Clone your fork
git remote add admins gitlab-ce:admins/git-tips.git   # Add the 'admins' remote
```

You now have two remotes; 'foo' and 'admins'. You can push or pull code using either of them:

```
git push foo master           # Push local branch 'master' to remote 'foo'
git push admins master        # Push local branch 'master' to remote 'admins'
git push admins master:foo    # Push local branch 'master' to remote 'admins', branch 'foo'
git pull admins foo           # Pull changes from remote 'admins', branch 'foo'
git rebase bar admins/master  # Rebase your local branch 'bar' on top of the branch 'master' on admins; changes made to your local branch only
```

Ignore the 'rebase' example above if you're unfamiliar with rebase; it's only there for an example of how you can work with remotes. For more information about git, please read through [Pro Git](https://git-scm.com/book/en/v2).


### Contributing Changes

Whether you're using the fork or branch workflow, contributing your changes is pretty much the same process (at least when using GitLab).

Once you have made changes that you would like to contribute to a project, you will need to notify the maintainers (or "owners") of the project by making a Merge Request, within GitLab's Web UI.

Making a Merge Request will send an email to the person you assign it to, and will show a diff of your branch versus the branch you are asking to merge your changes into. Follow the steps below to make a Merge Request in GitLab:

1. Browse to the project's page on GitLab (e.g. https://gitlab-ce/admins/git-tips.git) OR your forked repository's page
1. Locate and click '+ New Merge Request' near the top right of the page (Subject to change as GitLab evolves)
1. Select your source project and branch (e.g. admins/git-tips and 'foo', or username/git-tips and 'foo')
1. Select your target project and branch (e.g. admins/git-tips and 'prod')
1. Click 'Compare Branches'
1. Describe what your branch does and what testing you've done: don't rely on commit messages.
1. Click 'Submit new merge request'

Now your changes are ready to be reviewed. The project maintainers may have followup questions for you or ask you to make changes.

Make any changes, as needed, and comment on the Merge Request when you are ready for another review. As long as you push your changes to the same branch of your initial Merge Request, you should not need to create another one.

### Bringing in Changes

If you are the maintainer of a project, you may need to respond to Merge Requests and occasionally bring changes in to the 'production' branch from contributor's forks or branches.

How you bring those changes in depends on your preferences; some people prefer merging and merge commits, where others prefer a cleaner / more linear history and using 'rebase'.

If you are responsible for maintaining a project, do yourself a favor and read about the various methods people use for bringing in changes.

Merging is easy, especially within GitLab, because it can essentially all be done through the web interface using Merge Requests. Rebase (in my opinion) provides a clearer linear history of the project, but requires more understanding of git than this simple guide strives to impart.

[Information on rebasing](http://git-scm.com/book/en/v2/Git-Branching-Rebasing)

[Rebase vs. Merge](http://git-scm.com/book/en/v2/Git-Branching-Rebasing#Rebase-vs.-Merge)

[Information on merging](http://git-scm.com/book/en/v2/Git-Branching-Basic-Branching-and-Merging)

### Migrating a Repository from Other Services

The process is fairly straightforward:

* Create a new project on GitLab; either a user or group project
* Clone the project from the original service (if it has not already been cloned somewhere you can work with it)
* Add a remote for the new GitLab project you created
* Push the repository to your new GitLab remote

```
git clone -o source <source_URI> migrate    # (optional) Create a fresh clone
                                            # in the directory 'migrate'


# Checkout all remote branches locally (optional)
for branch in $(git branch -r | grep source | grep -v HEAD); do
  branch="${branch##*/}"
  git branch --track "$branch" source/"$branch"
done

# Where 'namespace' is the user/group, and 'project' is the name of the project on GitLab:
git remote add gitlab-ce gitlab-ce:<namespace>/<project>.git
git push gitlab-ce --all   # Push all local branches to the new remote
git push gitlab-ce --tags  # Push any local tags to GitLab (optional)
```
