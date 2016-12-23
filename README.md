# git skills
github学习安装操作的全部过程--培训资料：http://jingyan.baidu.com/article/4853e1e57c936f1909f7269a.html；
github怎么删除代码仓库：http://jingyan.baidu.com/article/215817f788f0c21eda1423f2.html


GitHub操作大全:

创建版本库:
    版本库又名仓库，英文名repository，你可以简单理解成一个目录，这个目录里面的所有文件都可以被Git管理起来，每个文件的修改、删除，Git都能跟踪，以便任何时刻都可以追踪历史，或者在将来某个时刻可以“还原”。
    所以，创建一个版本库非常简单，首先，选择一个合适的地方，创建一个空目录：
    $ mkdir learngit
    $ cd learngit
    $ pwd
    /Users/michael/learngit

    pwd命令用于显示当前目录。在我的Mac上，这个仓库位于/Users/michael/learngit。
    如果你使用Windows系统，为了避免遇到各种莫名其妙的问题，请确保目录名（包括父目录）不包含中文。
    第二步，通过git init命令把这个目录变成Git可以管理的仓库：
    $ git init
    Initialized empty Git repository in /Users/michael/learngit/.git/
              瞬间Git就把仓库建好了，而且告诉你是一个空的仓库（empty Git repository），细心的读者可以发现当前目录下多了一个.git的目录，这个目录是Git来跟踪管理版本库的，没事千万不要手动修改这个目录里面的文件，不然改乱了，就把Git仓库给破坏了。
              如果你没有看到.git目录，那是因为这个目录默认是隐藏的，用ls -ah命令就可以看见。
     

把文件添加到版本库:

    一定要放到learngit目录下（子目录也行），因为这是一个Git仓库，放到其他地方Git再厉害也找不到这个文件。
    第一步，用命令git add告诉Git，把文件添加到仓库：
    $ git add readme.txt
              执行上面的命令，没有任何显示，这就对了，Unix的哲学是“没有消息就是好消息”，说明添加成功。
    第二步，用命令git commit告诉Git，把文件提交到仓库：
    $ git commit -m "wrote a readme file"
    [master (root-commit) cb926e7] wrote a readme file
     1 file changed, 2 insertions(+)
     create mode 100644 readme.txt
    -m后面输入的是本次提交的说明，可以输入任意内容，当然最好是有意义的，这样你就能从历史记录里方便地找到改动记录。

              为什么Git添加文件需要add，commit一共两步呢？因为commit可以一次提交很多文件，所以你可以多次add不同的文件，比如：
    $ git add file1.txt
    $ git add file2.txt file3.txt
    $ git commit -m "add 3 files."

工作区和暂存区:

    git status命令可以让我们时刻掌握仓库当前的状态，

    git diff：顾名思义就是查看difference;查看修改的内容

    git log命令显示从最近到最远的提交日志， 如果嫌输出信息太多，看得眼花缭乱的，可以试试加上--pretty=oneline参数


              在Git中，用HEAD表示当前版本，上一个版本就是HEAD^，上上一个版本就是HEAD^^，当然往上100个版本写100个^比较容易数不过来，所以写成HEAD~100。
    $ git reset --hard HEAD^
    HEAD is now at ea34578 add distributed
              现在，我们要把当前版本“append GPL”回退到上一个版本“add distributed”，就可以使用git reset命令：
    再想恢复到append GPL，就必须找到append GPL的commit id。Git提供了一个命令git reflog用来记录你的每一次命令：
    $ git reflog
    ea34578 HEAD@{0}: reset: moving to HEAD^
    3628164 HEAD@{1}: commit: append GPL
    ea34578 HEAD@{2}: commit: add distributed
    cb926e7 HEAD@{3}: commit (initial): wrote a readme file
    $ git reset --hard 3628164
    HEAD is now at 3628164 append GPL

管理修改
    阅读: 343364
    现在，假定你已经完全掌握了暂存区的概念。下面，我们要讨论的就是，为什么Git比其他版本控制系统设计得优秀，因为Git跟踪并管理的是修改，而非文件。
    你会问，什么是修改？比如你新增了一行，这就是一个修改，删除了一行，也是一个修改，更改了某些字符，也是一个修改，删了一些又加了一些，也是一个修改，甚至创建一个新文件，也算一个修改。
    为什么说Git管理的是修改，而不是文件呢？我们还是做实验。第一步，对readme.txt做一个修改，比如加一行内容：
    $ cat readme.txt
    Git is a distributed version control system.
    Git is free software distributed under the GPL.
    Git has a mutable index called stage.
    Git tracks changes.

    然后，添加：
    $ git add readme.txt
    $ git status
    # On branch master# Changes to be committed:#   (use "git reset HEAD <file>..." to unstage)##       modified:   readme.txt#
    然后，再修改readme.txt：
    $ cat readme.txt 
    Git is a distributed version control system.
    Git is free software distributed under the GPL.
    Git has a mutable index called stage.
    Git tracks changes of files.

    提交：
    $ git commit -m "git tracks changes"
    [master d4f25b6] git tracks changes
     1 file changed, 1 insertion(+)

    提交后，再看看状态：
    $ git status
    # On branch master# Changes not staged for commit:#   (use "git add <file>..." to update what will be committed)#   (use "git checkout -- <file>..." to discard changes in working directory)##       modified:   readme.txt#
    no changes added to commit (use "git add" and/or "git commit -a")

    咦，怎么第二次的修改没有被提交？
    别激动，我们回顾一下操作过程：
    第一次修改 -> git add -> 第二次修改 -> git commit
    你看，我们前面讲了，Git管理的是修改，当你用git add命令后，在工作区的第一次修改被放入暂存区，准备提交，但是，在工作区的第二次修改并没有放入暂存区，所以，git commit只负责把暂存区的修改提交了，也就是第一次的修改被提交了，第二次的修改不会被提交。
    提交后，用git diff HEAD -- readme.txt命令可以查看工作区和版本库里面最新版本的区别：
    $ git diff HEAD -- readme.txt 
    diff --git a/readme.txt b/readme.txt
    index 76d770f..a9c5755 100644
    --- a/readme.txt
    +++ b/readme.txt
    @@ -1,4 +1,4 @@
     Git is a distributed version control system.
     Git is free software distributed under the GPL.
     Git has a mutable index called stage.
    -Git tracks changes.
    +Git tracks changes of files.

    可见，第二次修改确实没有被提交。
    那怎么提交第二次修改呢？你可以继续git add再git commit，也可以别着急提交第一次修改，先git add第二次修改，再git commit，就相当于把两次修改合并后一块提交了：

    第一次修改 -> git add -> 第二次修改 -> git add -> git commit;


撤销修改：
    命令git checkout -- readme.txt意思就是，把readme.txt文件在工作区的修改全部撤销，这里有两种情况：
    一种是readme.txt自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；
    一种是readme.txt已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。
    总之，就是让这个文件回到最近一次git commit或git add时的状态。
    git checkout -- file命令中的--很重要，没有--，就变成了“切换到另一个分支”的命令，我们在后面的分支管理中会再次遇到git checkout命令。

删除文件：
     一般情况下，你通常直接在文件管理器中把没用的文件删了，或者用rm命令删了：
    另一种情况是删错了，因为版本库里还有呢，所以可以很轻松地把误删的文件恢复到最新版本：
    $ git checkout -- test.txt



添加远程仓库:
    $ git remote add origin git@github.com:michaelliao/learngit.git
    请千万注意，把上面的michaelliao替换成你自己的GitHub账户名

    下一步，就可以把本地库的所有内容推送到远程库上：
    $ git push -u origin master
    Counting objects: 19, done.
    Delta compression using up to 4 threads.
    Compressing objects: 100% (19/19), done.
    Writing objects: 100% (19/19), 13.73 KiB, done.
    Total 23 (delta 6), reused 0 (delta 0)
    To git@github.com:michaelliao/learngit.git
     * [new branch]      master -> master
    Branch master set up to track remote branch master from origin.
    把本地库的内容推送到远程，用git push命令，实际上是把当前分支master推送到远程。

    由于远程库是空的，我们第一次推送master分支时，加上了-u参数，Git不但会把本地的master分支内容推送的远程新的master分支，还会把本地的master分支和远程的master分支关联起来，在以后的推送或者拉取时就可以简化命令。
    从现在起，只要本地作了提交，就可以通过命令：
    $ git push origin master


从远程库克隆:

    首先，登陆GitHub，创建一个新的仓库，名字叫gitskills：
    我们勾选Initialize this repository with a README，这样GitHub会自动为我们创建一个README.md文件。创建完毕后，可以看到README.md文件：     
    现在，远程库已经准备好了，下一步是用命令git clone克隆一个本地库：
    方法一：
    $ git clone git@github.com:michaelliao/gitskills.git
    Cloning into 'gitskills'...
    remote: Counting objects: 3, done.
    remote: Total 3 (delta 0), reused 0 (delta 0)
    Receiving objects: 100% (3/3), done.

    $ cd gitskills
    $ ls
    README.md
    方法二：
    注意把Git库的地址换成你自己的，然后进入gitskills目录看看，已经有README.md文件了。
              GitHub给出的地址不止一个，还可以用https://github.com/michaelliao/gitskills.git这样的地址。实际上，Git支持多种协议，默认的git://使用ssh，但也可以使用https等其他协议。



创建与合并分支:
    首先，我们创建dev分支，然后切换到dev分支：
    $ git checkout -b dev
    Switched to a new branch 'dev'

               可以，用git branch命令查看当前分支：
    $ git branch
    * dev
      master
           然后，我们就可以在dev分支上正常提交，比如对readme.txt做个修改，加上一行：
                       ➜  test git:(dev) ✗  vi 1.txt
    然后提交：
    $ git add readme.txt 
    $ git commit -m "branch test"
    [dev fec145a] branch test
     1 file changed, 1 insertion(+)

    现在，dev分支的工作完成，我们就可以切换回master分支：
    $ git checkout master
    Switched to branch 'master'

    切换回master分支后，再查看一个readme.txt文件，刚才添加的内容不见了！因为那个提交是在dev分支上，而master分支此刻的提交点并没有变：
    现在，我们把dev分支的工作成果合并到master分支上：
    $ git merge dev
    Updating d17efd8..fec145a
    Fast-forward
     readme.txt |    1 +
     1 file changed, 1 insertion(+)

    git merge命令用于合并指定分支到当前分支。合并后，再查看readme.txt的内容，就可以看到，和dev分支的最新提交是完全一样的。
    合并完成后，就可以放心地删除dev分支了：
    $ git branch -d dev
    Deleted branch dev (was fec145a).

    删除后，查看branch，就只剩下master分支了：
    $ git branch
    * master


解决冲突：

    分支管理策略:
        合并分支时，加上--no-ff参数就可以用普通模式合并，合并后的历史有分支，能看出来曾经做过合并，而fast forward合并就看不出来曾经做过合并。
        分支策略在实际开发中，我们应该按照几个基本原则进行分支管理：
        首先，master分支应该是非常稳定的，也就是仅用来发布新版本，平时不能在上面干活；
        那在哪干活呢？干活都在dev分支上，也就是说，dev分支是不稳定的，比如1.0版本发布时，再把dev分支合并到master上，在master分支发布1.0版本；
        你和你的小伙伴们每个人都在dev分支上干活，每个人都有自己的分支，时不时地往dev分支上合并就可以了。
        所以，团队合作的分支看起来就像这样：


多人协作:

     当你从远程仓库克隆时，实际上Git自动把本地的master分支和远程的master分支对应起来了，并且，远程仓库的默认名称是origin。

    用git remote -v显示更详细的信息：
    $ git remote -v
    origin  git@github.com:michaelliao/learngit.git (fetch)
    origin  git@github.com:michaelliao/learngit.git (push)

    上面显示了可以抓取和推送的origin的地址。如果没有推送权限，就看不到push的地址。
    推送分支:推送分支，就是把该分支上的所有本地提交推送到远程库。推送时，要指定本地分支，这样，Git就会把该分支推送到远程库对应的远程分支上：
    $ git push origin master

    如果要推送其他分支，比如dev，就改成：
    $ git push origin dev

    但是，并不是一定要把本地分支往远程推送，那么，哪些分支需要推送，哪些不需要呢？
      * master分支是主分支，因此要时刻与远程同步；
      * dev分支是开发分支，团队所有成员都需要在上面工作，所以也需要与远程同步；
      * bug分支只用于在本地修复bug，就没必要推到远程了，除非老板要看看你每周到底修复了几个bug；
      * feature分支是否推到远程，取决于你是否和你的小伙伴合作在上面开发。


操作标签:
    如果标签打错了，也可以删除：
    $ git tag -d v0.1Deleted tag 'v0.1' (was e078af9)

    因为创建的标签都只存储在本地，不会自动推送到远程。所以，打错的标签可以在本地安全删除。
    如果要推送某个标签到远程，使用命令git push origin <tagname>：
    $ git push origin v1.0Total 0 (delta 0), reused 0 (delta 0)
    To git@github.com:michaelliao/learngit.git
     * [new tag]         v1.0 -> v1.0
    或者，一次性推送全部尚未推送到远程的本地标签：
    $ git push origin --tags
    Counting objects: 1, done.
    Writing objects: 100% (1/1), 554 bytes, done.
    Total 1 (delta 0), reused 0 (delta 0)
    To git@github.com:michaelliao/learngit.git
     * [new tag]         v0.2 -> v0.2
     * [new tag]         v0.9 -> v0.9
    如果标签已经推送到远程，要删除远程标签就麻烦一点，先从本地删除：
    $ git tag -d v0.9Deleted tag 'v0.9' (was 6224937)

    然后，从远程删除。删除命令也是push，但是格式如下：
    $ git push origin :refs/tags/v0.9To git@github.com:michaelliao/learngit.git
     - [deleted]         v0.9
    要看看是否真的从远程库删除了标签，可以登陆GitHub查看。



小结
    初始化一个Git仓库，使用git init命令。
    添加文件到Git仓库，分两步：

      * 第一步，使用命令git add <file>，注意，可反复多次使用，添加多个文件；
      * 第二步，使用命令git commit，完成。
      * 要随时掌握工作区的状态，使用git status命令。
      * 如果git status告诉你有文件被修改过，用git diff可以查看修改内容
      * HEAD指向的版本就是当前版本，因此，Git允许我们在版本的历史之间穿梭，使用命令git reset --hard commit_id。
      * 穿梭前，用git log可以查看提交历史，以便确定要回退到哪个版本。
      * 要重返未来，用git reflog查看命令历史，以便确定要回到未来的哪个版本。

    又到了小结时间。
    场景1：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令git checkout -- file。(注意空格)
    场景2：当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令git reset HEAD file，就回到了场景1，第二步按场景1操作。
    $ git reset HEAD readme.txt
    Unstaged changes after reset:
    M       readme.txt
    场景3：已经提交了不合适的修改到版本库时，想要撤销本次提交，参考版本回退一节，不过前提是没有推送到远程库。


    要关联一个远程库，使用命令git remote add origin git@server-name:path/repo-name.git；
    关联后，使用命令git push -u origin master第一次推送master分支的所有内容；

    此后，每次本地提交后，只要有必要，就可以使用命令git push origin master推送最新修改； 拉：    git puLL origin master
    分布式版本系统的最大好处之一是在本地工作完全不需要考虑远程库的存在，也就是有没有联网都可以正常工作，而SVN在没有联网的时候是拒绝干活的！当有网络的时候，再把本地提交推送一下就完成了同步，真是太方便了！

    分支与合并：
              Git鼓励大量使用分支：
    查看分支：git branch
    创建分支：git branch <name>
    切换分支：git checkout <name>
    创建+切换分支：git checkout -b <name>
    合并某分支到当前分支：git merge <name>
    删除分支：git branch -d <name>


    创建标签:

      * 命令git tag <name>用于新建一个标签，默认为HEAD，也可以指定一个commit id；

      * git tag -a <tagname> -m "blablabla..."可以指定标签信息；

      * git tag -s <tagname> -m "blablabla..."可以用PGP签名标签；

    命令git tag可以查看所有标签。











