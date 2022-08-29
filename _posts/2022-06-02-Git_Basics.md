---
layout: post
title: "Git Basics"
date:   2022-06-02
tags: [others]
comments: false
author: keyinwu
---

## Git Intro

<img src="https://github.com/keyinwu/blog/raw/main/images/Others/git_command.jpeg" width="70%"/>

## Configuration

### Name & Email

```git
$ git config --global user.name {username}
$ git config --global user.email {email}  
```

### SSH Key

Enter “~/.ssh” directory, execute:

  ```git
  $ ssh-keygen -t rsa -C "your.email@example.com" -b 4096
  ```

Add SSH key on the github website.


## Git Operations

### Pull

1. Create a new folder as the local workspace, enter the folder and execute:

    ```git
    $ git init
    ```

2. Connect the local repo with the corresponding branch using ssh address:
   
   ```git
    $ git remote add origin {ssh-address}
   ```

3. Pull the branch to the local:

   ```git
   $ git pull origin {remote-branch}:{local-branch}
   # for example:
   $ git pull origin local-origin
   ```

4. Done.

### Push

1. Check the status of files in the local:

   ```git
   $ git status
   ```

2. Add modified file to staging area:

   ```git
   $ git add -A .            # add all files
   $ git add {folder/file}   # add specific file(s)
   ```

3. Commit to the local repository:

   ```git
   $ git commit -m "{comments}"
   ```

4. Synchronize the previous version and rebase, in order to make sure there will not be conflicts when merging the branch with the remote origin. If there are conflicts in the rebase process, they should be solved.

   ```
   $ git fetch origin {origin-branch}
   $ git rebase origin/{origin-branch}
   ```

5. Push to the personal remote branch:

   ```
   $ git push origin {local-branch}:{remote-dev-branch}
   ```

6. Submit a merge request to merge with the remote origin.
