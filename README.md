# Automated Git Pull

### Getting Started
First setup git repository environments. A complete environment will contain 5
repositories, as displayed in figure.txt:


##### 1) Create hold repo's
- run git init inside the local hold repo directory to create the necessary .git
files. Set the git remote in the working repo to have access to the local hold
repo.
- run git init --bare inside the server hold repo directory to create the
necessary .git files for a bare git repository. Set the git remote in the server
hold repo to have access to the origin repository.

##### 2) Set up post-receive script hook in origin repo
- Save post-receive in the hooks folder under ..../origin/.git/hooks.
- In post-receive, change the dir variable to equal the correct directory
destination for the dropbox folder. Also make sure the target variable is set
to the directory location of the server hold repo.

##### 3) Set up watcher.c
- Save watcher.c anywhere on the local side.
- Edit watcher.c line 27 and change the watch directory to the directory of the
dropbox file.

##### 4) Set up unpack script
- Save unpack in the same folder as watcher.c
- In unpack, change the dir variable to equal the correct directory destination
for the local hold repo. Also make sure the target variable is set to the
directory location of the dropbox folder.


### Syncing the working repo with origin repo
##### 1) Run ./watcher
- watcher needs to be manually restarted every time dropbox is updated in order
to catch the update and store it to the local hold repo.

##### 2) User pushes to origin
- Once a user pushes to the origin repo, the post-receive hook is triggered.
This tells the server hold repo to pull from origin, tar it, create md5sum and
sending the two files to the dropbox folder, overwriting it in the process.

##### 3) watcher detects change in dropbox
- Once a change in the dropbox folder has occurred, watcher runs unpack.
- unpack copies the dropbox contents (tar'd repo and md5sum) to the local hold
repo. It then creates a new md5sum of the tar'd repo file from the dropbox
folder. If the new md5sum and the md5sum from the dropbox folder match, the
contents of the tar file are extracted into the local hold repo and committed to
the local hold repo's git.

##### 4) Pulling from the local hold repo (updating the working repo)
- Once the local hold repo is updated with the changes that were pushed to the
origin repo, the changes can be reviewed and pulled to the working repo.
- After pulling from the local hold repo be sure to delete the visible files
within the local hold repo.
