# git

- install

        - mac
        included in system
        
        - debian/ubuntu
        sudo apt-get update
        sudo apt-get upgrade
        sudo apt-get install git

        - redhat/centos/oraclelinux
        sudo yum upgrade
        sudo yum install git

        - set global commit name
        git config --global user.name someone
        git config --global user.email someone@example.com
        git config --global credential.username someone
        
        - set local commit name (.git/config, overwrite global)
        git config --local user.name someone
        git config --local user.email someone@example.com
        git config --local credential.username someone

        check configs
        git config --list

        - save/update password
        git config --global credential.helper osxkeychain (save to Keychain)
        Keychain access -> git

        - set editor
        git config --global core.editor "'xxx program' -multiInst -nosession"

- clone

        git clone https://github.com/kyle11235/git somefolder
        
        - clone private
        git clone https://someone@github.com/kyle11235/git

        - clone by ssh
        create ssh keys, private key by default is saved into ~/.ssh/id_rsa
        upload public key to github/project/settings/deploy key
        chmod 400 ~/.ssh/id_rsa
        git clone git@xxx.com:xxx/xxx.git

        - change remote to ssh
        git remote set-url origin git@github.com:kyle11235/xxx.git

        - clone self signed/ignore ssl
        git -c http.sslVerify=false clone https://x.x.x.x/root/xxx.git

- change state

        - file state

        touch -> add -> commit
        untracked -> stated -> unmodified -> modified -> staged

        - track
        git add cm1.txt cm2.txt

        - undo track
        git rm cm1.txt (if it is not committed, it will ask you to use --cached to keep file or -f to delete file)
        git rm --cached cm1.txt
        git rm -f cm1.txt

        - undo modifications - https://git-scm.com/docs/git-checkout
        git checkout -- readme.txt (with -- means file, without -- means switching branch)

        - stage modified file (next to commit)
        git add readme.txt

        - undo staged file - https://git-scm.com/docs/git-reset
        git reset HEAD readme.txt   (HEAD == the current branch, maybe master)
        git reset (this will reset all staged files back to working tree)

        - check status
        git status
        git status -s

        - compare what is in your working directory VS staging area (next to commit)
        git diff (diff can be used to compare modification in different stages/compare branches)

        - compare what is in your staging area VS last commit
        git diff --staged

        - use diff tool
        git difftool --tool-help
        e.g. git difftool --tool=vimdiff, this will start 2 vim windows for each file for you to edit, for you can specify the file: git difftool --tool=vimdiff test.txt

        - commit with -m
        git commit -m “add readme”

        - commit all changes of tracking file directly without staging
        git commit -am “no need to do git add…”

- revert commit

        e.g.
        commit3
        commit2
        commit1

        - undo commit with going back to last 2th commit state, move last 1th commit back to stage (this is usefull when you forget to commit some file)

        git reset --soft HEAD~1  (or  git reset --soft HEAD^   then input   ~1)

        - undo commit with new commit–  https://git-scm.com/docs/git-revert
        use a new commit to revert the specific last 2th commit content, history is still there

        git revert HEAD~1  or  git revert master~1

- rm / move

        - remove a file from working dir and git
        git rm todo.txt (this will delete file from disk, you can use --cached to keep an untracked file)
        git commit -m “remove todo”

        - move/rename file
        git mv README.md README

        - move way2
        mv README.md README
        git rm README.md
        git add README

- log

        git log
        git log --stat (good for code review)
        git log -p -1 (-p prints detailed diff in each commit. -1 limits the output to only the last two entries)

- branching

        - check
        git branch

        - create (Creating a new branch is as quick and simple as writing 41 bytes to a file )
        git branch test

        - checkout
        git checkout test
        
        - checkout to a specific commit
        git checkout -b temp-branch 3a0f9e17a83ceb4a2fe8b431469ea4903347b584

        - stash before merge
        save work before merge, either commit it to a temporary branch or stash it

        git stash
        git stash list

        git stash apply
        git stash apply stash@{0}
        git stash apply --index    (with --index it restages for you)

        git stash drop
        sit stash drop stash@{0}

        - compare before merge
        git diff master  (based on master, what I have changed on the current branch, show changes at the bottom)

        - merge
        git merge (auto commit if no conflict)

        - merge but not commit
        git merge test --no-commit --no-ff (even it's fast forward)

        - manual commit
        git commit -m "good merge"

                - if conflict

                        - not to merge
                        git merge --abort

                        - resolve conflict
                        git add resolved_file
                        git commit -m "comment"

        - check last commit of each branch
        git branch -v

        - check merged branches to current
        git branch --merged

        - delete branch
        git branch -d test

- git remoting

        - git clone
        git clone remote-tracking branch (upstream branch) -> tracking branch (local branch)
        remote repo = origin
        tracking branch = master, its upstream branch = origin/master, only 'clone' setup upstream automatically

        - clone specific remote branch
        git clone -b testing --single-branch $git_url test
        git clone && git checkout test

        - create new branch and push
        git checkout -b test
        git push --set-upstream origin test

        - check
        git remote -v
        git remote show origin (show details)

        - manage
        git remote add pb https://github.com/xxx/hello
        git remote rename pb paul
        git remote remove paul

        - fetch
        git fetch
        git fetch origin
        git fetch origin master:tmp

        - compare after fetch
        git diff tmp

        - merge manually after fetch
        git merge orgin/master

        - pull
        git pull = fetch to origin/master and merge to master automatically

        - push
        git push
        git push origin master = git push origin master:master
        git push origin master:remote_branch

        - delete remote branch
        git push origin --delete test

- tag (commit point)

        - tag
        git tag v0.0.1 -m "v1 tag"
        git tag v0.0.1 commit1_id -m "commit1 tag"

        - check
        git tag
        git show v0.0.1

        - checkout (detached HEAD state, you cannot commit)
        git checkout v0.0.1

        - checkout to branch (you can commit)
        git checkout -b test v0.0.1

        - push
        git push origin v0.0.1

        - delete local
        git tag -d v0.0.1

        - delete remote
        git push origin --delete v0.0.1

- workflow

        - fork the project
        - create a topic branch from master
        - make some commits to improve the project.
        - push this branch to your GitHub project.
        - open a Pull Request on GitHub.
        - discuss, and optionally continue committing.
        - the project owner merges or closes the Pull Request.
