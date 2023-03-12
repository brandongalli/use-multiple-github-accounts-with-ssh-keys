# Easiest way to use multiple github accounts with ssh keys
Here are the steps to use multiple github accounts on same machine.
Let's assume that we have 2 github accounts.
- github.com/mygit
- github.com/companygit

### 1. Clear all cached ssh keys
```
ssh-add -D
```


### 2. Generate ssh keys for both `mygit` and `companygit` and add them to your github accounts.
```
ssh-keygen -t rsa -C "id_rsa_mygit"
ssh-keygen -t rsa -C "id_rsa_companygit"
```
NOTE: Replace `"id_rsa_mygit"` and `"id_rsa_companygit"` with your own words.
(e.g. ssh-keygen -t rsa -C "id_rsa_orkiv")


### 3. Add ssh keys to the authentication agent.
```
ssh-add ~/.ssh/id_rsa_mygit
ssh-add ~/.ssh/id_rsa_companygit
```

### 4. Configure ssh aliases.
```
touch ~/.ssh/config
```
Open up `~/.ssh/config` file with command-line editor.
(e.g. `sudo nano ~/.ssh/config`)

Add the following configuration.
```
Host github.com-mygit
	HostName github.com
	User git
	IdentityFile ~/.ssh/id_rsa_mygit

Host github.com
	HostName github.com
	User git
	IdentityFile ~/.ssh/id_rsa_companygit
```
NOTE: For the 2nd account, the Host is `github.com`. Why?

When I use another name(e.g. `Host github.com-companygit`), the first Host(for `mygit`) was invoked for the other hosts and getting the following error.
```
ERROR: Permission to [companygit/test.git] denied to [mygit].
fatal: Could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists.
```
After changing the Host as `github.com` for the second account, the error was gone.


### 5. Enjoy yourself.
