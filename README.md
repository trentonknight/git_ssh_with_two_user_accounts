## Git ssh with two user accounts

First create two different ssh keys. For example for your username alpha github account:

```bash
ssh-keygen -t ed25519 -f alpha
```
Now for your second work account with the username bravo:

```bash
ssh-keygen -t ed25519 -f bravo
```
## Provide public key to hosts
* [Adding new ssh key to your github account](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account)
* [Add an SSH key to your GitLab account](https://docs.gitlab.com/ee/user/ssh.html)

Assuming you have xclip installed on Linux:

```bash
xclip -sel clip < ~/.ssh/alpha.pub
```
See the gitlab tutorial URL listed above for more robust details. Now add your newly created public keys to whomever is hosting your repository such as GitHub, Gitlab or other. 

## Edit ssh config file
* [how-to-use-a-different-private-ssh-key-for-git-shell-commands](https://www.howtogeek.com/devops/how-to-use-a-different-private-ssh-key-for-git-shell-commands/)
* [Use different accounts on a single GitLab instance](https://docs.gitlab.com/ee/user/ssh.html)

```bash
Host personal
	Hostname github.com
	IdentityFile ~/.ssh/alpha
	PreferredAuthentications publickey
	IdentitiesOnly yes

Host work
	Hostname github.com
	IdentityFile ~/.ssh/bravo
	PreferredAuthentications publickey
	IdentitiesOnly yes
```

Or using the Gitlab example:

```bash

# User1 Account Identity
Host alpha.gitlab.com
  Hostname gitlab.com
  PreferredAuthentications publickey
  IdentityFile ~/.ssh/alpha

# User2 Account Identity
Host bravo.gitlab.com
  Hostname gitlab.com
  PreferredAuthentications publickey
  IdentityFile ~/.ssh/bravo
```

Now you can clone the repository:

```bash
git clone git@alpha.gitlab.com:gitlab-org/gitlab.git
```

Or update a previously-cloned repo:

```bash
git remote set-url origin git@bravo.gitlab.com:gitlab-org/gitlab.git
```	
Or on Github Example 

```bash
git remote remove origin
```

```bash
 git remote add origin git@alpha:trentonknight/git_ssh_with_two_user_accounts.git

```



### Origin issues

If you recieve the following error after changes

```bash
   main ⇡1 git push                                                  ✔ 
ERROR: Permission to gitlab-org/gitlab.git denied to alpha.
fatal: Could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists.
```
Remove the origin add recreate using the correct ssh key as follows:

```bash
   main ⇡1    git remote remove origin
```

Add remote
```bash
   main    git remote add origin git@alpha:gitlab-org/gitlab.git
                                 
```
Push back to main or choosen branch
```bash
   main    git push --set-upstream origin main
```








