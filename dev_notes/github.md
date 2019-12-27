...menustart

 - [github](#bf215181b5140522137b3d4f6b73544a)
     - [github update fork](#f6b73a9a864f02b2d14ad454c6b09e68)
     - [run html on github](#606e5c37337c2f05305ab4a4a0dc2691)
     - [git status -s ignore file mode](#ecf2b9ae77e1b9272d6716ab8337c37e)
     - [merge specific commit](#a6c7b8bc87e837e643f48e27b843d648)
     - [show file change of a commit](#e35fc6dbd7673d56c0824c31ff378241)
     - [get a file with specific revision](#6f4311248df3ab2115e904e14c7836c9)
     - [git show/diff 乱码问题](#aafd38d2cb2288571bb67fc78e3a18f7)
     - [\[trick\] use git log to display the diffs while searching](#a9df5d1d20b4eb063767169d82151fdc)
     - [how to set up username and passwords for different git repos](#a3aecaf26f7ec612b34f4d9ed6c6532d)
     - [provide username when clone private repos](#366ee47209629dccbab3d2399247ea84)
     - [gitlab: git clone leads to "SSL certificate problem: unable to get local issuer certificate"](#8da880caa0ca98d1c46a028c0da79aac)
     - [Calling git clone using password with special character](#60f96f2175fb84d4839e67f2533a4c10)
     - [Copy branch from another repository](#9af7d00519ec3625b399242404c33af2)
     - [delete a branch locally and remotely](#65804564299051849847b74237b908e7)
     - [ignore system proxy setting](#69a87c5b277c131f12dde6841d30e6bc)

...menuend


<h2 id="bf215181b5140522137b3d4f6b73544a"></h2>


# github 

<h2 id="f6b73a9a864f02b2d14ad454c6b09e68"></h2>


## github update fork

 - 给fork配置远程上游仓库 
    - `git remote add upstream xxxx.git`
 - 同步fork
    - 从上游仓库 fetch 分支和提交点，提交给本地 master，并会被存储在一个本地分支 upstream/master 
        - `git fetch upstream` 
    - 切换到本地主分支(如果不在的话) 
        - `git checkout master` 
    - 把 upstream/master 分支合并到本地 master 上，这样就完成了同步，并且不会丢掉本地修改的内容。
        - `git merge upstream/master` 
    - 提交 
        - `git push origin master`

        
<h2 id="606e5c37337c2f05305ab4a4a0dc2691"></h2>


## run html on github

 - visit `http://rawgit.com/`
 - 输入你需要执行的 html 网页，会给出两个链接
    - https://rawgit.com/mebusy/html5_examples/master/XXXX.html
        - url for production
    - https://cdn.rawgit.com/mebusy/html5_examples/master/XXXX.html
        - url for dev/testing
 - 注意： cdn.rawgit.com 上的文件，第一次访问后会被 永久cache，从而导致 拉取不到最新的文件。
    - 所以一般访问的时候，会指定 某个 commit hash  ， 或使用最新的 HEAD
    - `https://cdn.rawgit.com/mebusy/html5_examples/a12340a5d32b0c760ef138301b067fb1153ef94b/00_marchingSquare.html`
    - or `https://cdn.rawgit.com/mebusy/html5_examples/HEAD/00_marchingSquare.html`


<h2 id="ecf2b9ae77e1b9272d6716ab8337c37e"></h2>


## git status -s ignore file mode

```bash
git config core.filemode false
```

<h2 id="a6c7b8bc87e837e643f48e27b843d648"></h2>


## merge specific commit 

```
git cherry-pick [-n] <commit> 
```

 - `-n` means `no commit `

<h2 id="e35fc6dbd7673d56c0824c31ff378241"></h2>


## show file change of a commit 

```
git diff-tree <commit>
```

<h2 id="6f4311248df3ab2115e904e14c7836c9"></h2>


## get a file with specific revision

```
git show REVISION:filePath > outFilePath
```


<h2 id="aafd38d2cb2288571bb67fc78e3a18f7"></h2>


## git show/diff 乱码问题

```
git diff | less -r
```

 - `-r` means `Output "raw" control characters`.


<h2 id="a9df5d1d20b4eb063767169d82151fdc"></h2>


## [trick] use git log to display the diffs while searching

```
git log -p -- path/to/file
```


<h2 id="a3aecaf26f7ec612b34f4d9ed6c6532d"></h2>


## how to set up username and passwords for different git repos

 1. Using SSH 
    - `ssh-agent`  and `ssh-add`
 2. Using gitcredentials
    - https://git-scm.com/docs/gitcredentials

```
git config --global credential.${remote}.username yourusername
git config --global credential.helper store
```

 - here, `{remote}` is something like `https://github.com`
 - 仓库 pull 下来后， 把上面两条指令 去掉 `--global` 再执行一次， 同时 删除`~/.gitconfig ` 中的 credential 设置，以避免影响其他的仓库冲突

```
$ cat ~/.gitconfig
[credential "https://github.com"]
    username = xxxxx
[credential]
    helper = store
```

<h2 id="366ee47209629dccbab3d2399247ea84"></h2>


## provide username when clone private repos

```
git clone https://username:password@github.com/username/repository.git
```


<h2 id="8da880caa0ca98d1c46a028c0da79aac"></h2>


## gitlab: git clone leads to "SSL certificate problem: unable to get local issuer certificate"

```
git config --global http.sslVerify false
git clone ...
git config --global http.sslVerify true
```


<h2 id="60f96f2175fb84d4839e67f2533a4c10"></h2>


## Calling git clone using password with special character

```
!   #   $    &   '   (   )   *   +   ,   /   :   ;   =   ?   @   [   ]
%21 %23 %24 %26 %27 %28 %29 %2A %2B %2C %2F %3A %3B %3D %3F %40 %5B %5D
```

 - for example

```
$ git clone https://myuser:password%21@github.com/myuser/repo.git
```

<h2 id="9af7d00519ec3625b399242404c33af2"></h2>


## Copy branch from another repository

 1. git clone your repo , so your repo's repo is named `origin`
 2. add another remote repo , give it name `html5`
    - `git remote add html5 ...` 
 3. fetch remote repo data
    - `git fetch html5`
 4. checkout 1 branch `html5/develop`` , to your local branch `develop`
    - `git checkout -b develop html5/develop`
 5. push you local branch `develop` to `origin`
    - `git push --set-upstream origin develop`

<h2 id="65804564299051849847b74237b908e7"></h2>


## delete a branch locally and remotely

```
$ git push --delete <remote_name> <branch_name>
$ git branch -d <branch_name>
```

<h2 id="69a87c5b277c131f12dde6841d30e6bc"></h2>


## ignore system proxy setting 

```
git config --global  --add remote.origin.proxy ""
```

