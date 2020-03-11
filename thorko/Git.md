Git
========================
git solve merge conflict

```
git reset --hard
git merge -X theirs
git pull
```

git solve your branch is ahead of master 
```
git reset --hard origin/master
git status
git pull origin master
```


### merging
```
git checkout int
git merge master	
```

### rename git branch
```
git checkout <old_name>
# rename
git branch -m <new_name>
# if already pushed to remote repo delete the old branch
git push origin --delete <old_name>
# push new
git push origin -u <new_name>
```

# git submodule - add another repo to yours
```
git submodule add <url>
git submodule init
git submodule update
```
to get the latest updates from the upstream run
```
git submodule update --remote --merge
```



# clone with submodule

```bash
git clone --recurse-submodules https://github.com/mer-qa/libvncserver.git
```

or

```bash
git clone https://github.com/mer-qa/libvncserver.git
git submodule init
git submodule update
```



## git import existing directory

```bash
cd /my/dir
git init
git add .
git commit -m "first commit"
git remote add https://gitlab.com/thorko/monitoring_pipeline.git
git push -u origin master
```

## git ignore

create a .gitignore file and add the files you want to ignore

```
update.sh
```

