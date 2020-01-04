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


## show files changed between 2 commit 

```bash
git diff commit1  commit2 --name-only
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
 4. checkout 1 branch `html5/develop` , to your local branch `develop`
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

# git 国内加速

1. 使用[站长工具](http://tool.chinaz.com/dns) 找到下面两个域名 较快的ip
    - `assets-cdn.github.com`
    - `github.global.ssl.fastly.net`
2. 把查到的ip 加入到 `/etc/hosts`
    - 
    ```bash
    185.199.109.153 assets-cdn.github.com
    185.199.108.153 assets-cdn.github.com
    185.199.110.153 assets-cdn.github.com
    185.199.111.153 assets-cdn.github.com

    69.63.184.14 github.global.ssl.fastly.net
    31.13.82.1 github.global.ssl.fastly.net
    ```
3. 刷新 NDS 缓存
    - 
    ```bash
    sudo dscacheutil -flushcache; sudo killall -HUP mDNSResponder
    ```



