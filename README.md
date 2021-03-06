# Git教程 
    参考:
    https://www.liaoxuefeng.com/wiki/896043488029600/897884457270432
## 1.Windows安装Git
    1.下载Git ftp://mi@banchengxuezhang.top/D盘（软件）/我的下载/Git-2.25.1-64-bit.exe
    2.安装完成后，在开始菜单里找到“Git”->“Git Bash”，蹦出一个类似命令行窗口的东西，就说明Git安装成功！
    3.安装完成后设置用户名和邮箱
    命令:
        git config --global user.name "xxx"
        git config --global user.email "xxx@xx.com"
    注意:--global 这个参数表示全局,设置了这个参数表示这台机器所有git仓库都会使用这个配置,当然也可以对某个仓库指定不同的用户名和Email地址。
## 2.创建版本库
    1. 首先选择一个合适的地方创建一个空目录(项目目录):
        命令如下:
        mkdir xxx 创建目录
        cd xxx 打开并进入目录
        pwd 查看当前目录
        注意:目录名,包括父目录,最好不要含中文
    2.初始化目录,让它变成Git仓库
        git init
        瞬间Git就把仓库建好了，而且告诉你是一个空的仓库（empty Git repository），细心的读者可以发现当前目录下多了一个.git的目录，这个目录是Git来跟踪管理版本库的，没事千万不要手动修改这个目录里面的文件，不然改乱了，就把Git仓库给破坏了。
        如果你没有看到.git目录，那是因为这个目录默认是隐藏的，用ls -ah命令就可以看见。      
    3.在Git目录中创建文件
        touch README.md
    4.将文件添加到Git仓库
        git add README.md
        git add * 提交目录下所有文件
        git add . 提交目录下所有文件
    5.将文件的修改进行提交
         git commit -m "xxx"
    -m "xxx" xxx写的是本次提交的一些说明,这样方便查看改动记录
     git commit命令执行成功后会告诉你，1 file changed：1个文件被改动（我们新添加的README.md文件）；2 insertions：插入了x行内容（README.md有x行内容）。
    小结
        现在总结一下今天学的两点内容：

        初始化一个Git仓库，使用git init命令。

        添加文件到Git仓库，分两步：

        使用命令git add <file>，注意，可反复多次使用，添加多个文件；
        使用命令git commit -m <message>，完成。
## 3.查看修改内容并提交
    1.首先我们进行工作,对README.md文件进行修改,通过git status 查看修改状态.
    git status 可以看出xxx文件被修改过了,但是没有提交的修改
    2.如果要看具体修改的内容,可以通过git diff 看看具体修改了什么内容,diff即不同,通过这个命令我们可以看出
    -xxx 删了什么
    +xxx添加了什么内容
    3.提交修改和提交新文件一样是两步
        1.git add xxx
        2.git commit -m "xxx"
    小结
        要随时掌握工作区的状态，使用git status命令。

        如果git status告诉你有文件被修改过，用git diff可以查看修改内容。 
## 4.版本回退(时光机)
    1.查看版本的历史记录
        命令:git log 可以看见提交id 提交人 提交时间 提交说明
        git log --pretty=oneline 不显示提交人和提交时间 只显示id和提交说明
        显示结果中,HEAD表示当前版本 上一个版本就是HEAD^ 上上个版本就是HEAD^^ 再往前写成HEAD~100
    2.版本回退命令:
        git reset --hard HEAD^ 回退到上一个版本
        版本回退,内容还原了,使用git log 已经看不见被退掉的版本了
        这个时候 翻记录找到要回退版本的那个id 如 c7cc4cbxxx
        使用 git reset --hard c7cc4cbxxx 回到指定的版本(包括log看不见的,已经被回退了的版本)
    3.Git的版本回退速度非常快，因为Git在内部有个指向当前版本的HEAD指针，当你回退版本的时候，Git仅仅是把HEAD从指向要退到的版本.
    4.通过 git reflog 查看每一次命令 ,找到那个想返回的的版本的id
    小结
        现在总结一下：

        HEAD指向的版本就是当前版本，因此，Git允许我们在版本的历史之间穿梭，使用命令git reset --hard commit_id。

        穿梭前，用git log可以查看提交历史，以便确定要回退到哪个版本。

        要重返未来，用git reflog查看命令历史，以便确定要回到未来的哪个版本。
## 5.工作区和暂存区
    工作区（Working Directory）
    就是你在电脑里能看到的目录，比如我的learngit文件夹就是一个工作区
    版本库（Repository）
    工作区有一个隐藏目录.git，这个不算工作区，而是Git的版本库。
    前面讲了我们把文件往Git版本库里添加的时候，是分两步执行的：
        第一步是用git add把文件添加进去，实际上就是把文件修改添加到暂存区；
        第二步是用git commit提交更改，实际上就是把暂存区的所有内容提交到当前分支。
        因为我们创建Git版本库时，Git自动为我们创建了唯一一个master分支，所以，现在，git commit就是往master分支上提交更改。
    你可以简单理解为，需要提交的文件修改通通放到暂存区，然后，一次性提交暂存区的所有修改。
    实践出真知:
        1.修改README.md文件
        2.文件夹下新建LICENSE文本 内容随便写
        3.使用git status 查看状态
        Git非常清楚地告诉我们，README.md被修改了，而LICENSE还从来没有被添加过，所以它的状态是Untracked。
    现在，使用两次命令git add，把README.md和LICENSE.txt都添加后，用git status再查看一下：
                    new file:   LICENSE.txt
	                modified:   README.md
    所以，git add命令实际上就是把要提交的所有修改放到暂存区（Stage），然后，执行git commit就可以一次性把暂存区的所有修改提交到分支。
    一旦提交后，如果你又没有对工作区做任何修改，那么工作区就是“干净”的：
        nothing to commit, working tree clean
## 6.管理修改
    现在，假定你已经完全掌握了暂存区的概念。下面，我们要讨论的就是，为什么Git比其他版本控制系统设计得优秀，因为Git跟踪并管理的是修改，而非文件。
    命令:
        cat README.md 查看文件内容
        git diff HEAD -- README.md 命令可以查看工作区和版本库里面最新版本的区别
    每次修改，如果不用git add到暂存区，那就不会加入到commit中。
    小结
        现在，你又理解了Git是如何跟踪修改的，每次修改，如果不用git add到暂存区，那就不会加入到commit中。
## 7.撤销修改
    当你修改之后,发现改出问题了,那么这个时候,你需要撤销掉你的修改
    使用命令
    1.git checkout -- README.md 可以把工作区的修改全部撤销,这里有两种情况
        一种是README.md自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；
        一种是README.md已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。
    总之，就是让这个文件回到最近一次git commit或git add时的状态。
    注意:命令中的--很重要，没有--，就变成了“切换到另一个分支”的命令，我们在后面的分支管理中会再次遇到git checkout命令,注意--之后要空格隔开
    2.当你add后还没有commit,你可以撤销add
    使用git reset HEAD README.md 可以把暂存区的修改撤销掉,重新放回工作区
    git reset命令既可以回退版本，也可以把暂存区的修改回退到工作区。当我们用HEAD时，表示最新的版本。
    小结
        又到了小结时间。

        场景1：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令git checkout -- file。

        场景2：当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令git reset HEAD <file>，就回到了场景1，第二步按场景1操作。

        场景3：已经提交了不合适的修改到版本库时，想要撤销本次提交，参考版本回退一节，不过前提是没有推送到远程库。
## 8.删除文件
    在Git中，删除也是一个修改操作，我们实战一下，先添加一个新文件test.txt到Git并且提交：
        touch test.txt
        git add test.txt
        git commit -m "提交新文件test"
    一般情况下，你通常直接在文件管理器中把没用的文件删了，或者用rm命令删了：
        rm test.txt
    这个时候，Git知道你删除了文件，因此，工作区和版本库就不一致了，git status命令会立刻告诉你哪些文件被删除了：
        git status
        1.现在你有两个选择，一是确实要从版本库中删除该文件，那就用命令git rm删掉，并且git commit：
            git rm test.txt
            git commit -m "xxx"
        2.另一种情况是删错了，因为版本库里还有呢，所以可以很轻松地把误删的文件恢复到最新版本：
        git checkout --test.txt
    小结
        命令git rm用于删除一个文件。如果一个文件已经被提交到版本库，那么你永远不用担心误删，但是要小心，你只能恢复文件到最新版本，你会丢失最近一次提交后你修改的内容。
## 9.添加远程库-获取ssh秘钥
    本章开始介绍Git的杀手级功能之一：远程仓库。
    完全可以自己搭建一台运行Git的服务器，不过现阶段，为了学Git先搭个服务器绝对是小题大作。好在这个世界上有个叫GitHub的神奇的网站，从名字就可以看出，这个网站就是提供Git仓库托管服务的，所以，只要注册一个GitHub账号，就可以免费获得Git远程仓库。

    在继续阅读后续内容前，请自行注册GitHub账号。由于你的本地Git仓库和GitHub仓库之间的传输是通过SSH加密的，所以，需要一点设置：
        第1步：创建SSH Key。在用户主目录下，看看有没有.ssh目录，如果有，再看看这个目录下有没有id_rsa和id_rsa.pub这两个文件，如果已经有了，可直接跳到下一步。如果没有，打开Shell（Windows下打开Git Bash），

        创建SSH Key：
        ssh-keygen -t rsa -C "xxx@example.com"

        你需要把邮件地址换成你自己的邮件地址，然后一路回车，使用默认值即可，由于这个Key也不是用于军事目的，所以也无需设置密码。     
        如果一切顺利的话，可以在用户主目录里找到.ssh目录，里面有id_rsa和id_rsa.pub两个文件，这两个就是SSH Key的秘钥对，id_rsa是私钥，不能泄露出去，id_rsa.pub是公钥，可以放心地告诉任何人。
        第2步：登陆GitHub，打开“Account settings”，“SSH Keys”页面：
        然后，点“Add SSH Key”，填上任意Title，在Key文本框里粘贴id_rsa.pub文件的内容：
        点“Add Key”，你就应该看到已经添加的Key：
        为什么GitHub需要SSH Key呢？因为GitHub需要识别出你推送的提交确实是你推送的，而不是别人冒充的，而Git支持SSH协议，所以，GitHub只要知道了你的公钥，就可以确认只有你自己才能推送。

    当然，GitHub允许你添加多个Key。假定你有若干电脑，你一会儿在公司提交，一会儿在家里提交，只要把每台电脑的Key都添加到GitHub，就可以在每台电脑上往GitHub推送了。

    最后友情提示，在GitHub上免费托管的Git仓库，任何人都可以看到喔（但只有你自己才能改）。所以，不要把敏感信息放进去。

    如果你不想让别人看到Git库，有两个办法，一个是交点保护费，让GitHub把公开的仓库变成私有的，这样别人就看不见了（不可读更不可写）。另一个办法是自己动手，搭一个Git服务器，因为是你自己的Git服务器，所以别人也是看不见的。这个方法我们后面会讲到的，相当简单，公司内部开发必备。

    确保你拥有一个GitHub账号后，我们就即将开始远程仓库的学习。
    小结
        “有了远程仓库，妈妈再也不用担心我的硬盘了。”——Git点读机
## 10.添加远程仓库-创建远程目录
    现在的情景是，你已经在本地创建了一个Git仓库后，又想在GitHub创建一个Git仓库，并且让这两个仓库进行远程同步，这样，GitHub上的仓库既可以作为备份，又可以让其他人通过该仓库来协作，真是一举多得。
        首先，登陆GitHub，然后，在右上角找到“Create a new repo”按钮，创建一个新的仓库：
        在Repository name填入learngit，其他保持默认设置，点击“Create repository”按钮，就成功地创建了一个新的Git仓库：
        目前，在GitHub上的这个learngit仓库还是空的，GitHub告诉我们，可以从这个仓库克隆出新的仓库，也可以把一个已有的本地仓库与之关联，然后，把本地仓库的内容推送到GitHub仓库。

        现在，我们根据GitHub的提示，在本地的learngit仓库下运行命令
## 11.绑定并推送到远程仓库
    git remote add origin git@github.com:banchengxuezhang/learngit.git
    请千万注意，把上面的banchengxuezhang替换成你自己的GitHub账户名，否则，你在本地关联的就是我的远程库，关联没有问题，但是你以后推送是推不上去的，因为你的SSH Key公钥不在我的账户列表中。

    添加后，远程库的名字就是origin，这是Git默认的叫法，也可以改成别的，但是origin这个名字一看就知道是远程库。

    下一步，就可以把本地库的所有内容推送到远程库上：
        git push -u origin master
    
    把本地库的内容推送到远程，用git push命令，实际上是把当前分支master推送到远程。

    由于远程库是空的，我们第一次推送master分支时，加上了-u参数，Git不但会把本地的master分支内容推送的远程新的master分支，还会把本地的master分支和远程的master分支关联起来，在以后的推送或者拉取时就可以简化命令。
    推送成功后，可以立刻在GitHub页面中看到远程库的内容已经和本地一模一样：

        从现在起，只要本地作了提交，就可以通过命令：
        git push origin master
     把本地master分支的最新修改推送至GitHub，现在，你就拥有了真正的分布式版本库！
     SSH警告
    当你第一次使用Git的clone或者push命令连接GitHub时，会得到一个警告：

    这是因为Git使用SSH连接，而SSH连接在第一次验证GitHub服务器的Key时，需要你确认GitHub的Key的指纹信息是否真的来自GitHub的服务器，输入yes回车即可。
    Git会输出一个警告，告诉你已经把GitHub的Key添加到本机的一个信任列表里了：
    Warning: Permanently added 'github.com,13.250.177.223' (RSA) to the list of known hosts.
## 12.解绑远程仓库(删除和远程仓库的关联)与总结
    如果添加的时候地址写错了，或者就是想删除远程库，可以用git remote rm <name>命令。使用前，建议先用git remote -v查看远程库信息：
    然后，根据名字删除，比如删除origin：
    git remote rm origin
    此处的“删除”其实是解除了本地和远程的绑定关系，并不是物理上删除了远程库。远程库本身并没有任何改动。要真正删除远程库，需要登录到GitHub，在后台页面找到删除按钮再删除。
    小结
        要关联一个远程库，使用命令
            git remote add origin git@server-name:path/repo-name.git；如:git remote add origin git@github.com:banchengxuezhang/learngit.git
        关联一个远程库时必须给远程库指定一个名字，origin是默认习惯命名；
        关联后，使用命令
            git push -u origin master
        第一次推送master分支的所有内容；
        此后，每次本地提交后，只要有必要，就可以使用命令
            git push origin master推送最新修改；
        分布式版本系统的最大好处之一是在本地工作完全不需要考虑远程库的存在，也就是有没有联网都可以正常工作，而SVN在没有联网的时候是拒绝干活的！当有网络的时候，再把本地提交推送一下就完成了同步，真是太方便了！
## 13.从远程仓库克隆
    上次我们讲了先有本地库，后有远程库的时候，如何关联远程库。
    现在，假设我们从零开发，那么最好的方式是先创建远程库，然后，从远程库克隆。
    首先，登陆GitHub，创建一个新的仓库，名字叫gitskills：
    我们勾选Initialize this repository with a README，这样GitHub会自动为我们创建一个README.md文件。创建完毕后，可以看到README.md文件：
    现在，远程库已经准备好了，下一步是用命令git clone克隆一个本地库：
        git clone git@github.com:banchengxuezhang/gitskills.git
    你也许还注意到，GitHub给出的地址不止一个，还可以用https://github.com/michaelliao/gitskills.git这样的地址。实际上，Git支持多种协议，默认的git://使用ssh，但也可以使用https等其他协议。
    小结
        要克隆一个仓库，首先必须知道仓库的地址，然后使用git clone命令克隆。
        Git支持多种协议，包括https，但ssh协议速度最快。
# 分支管理
    分支在实际中有什么用呢？假设你准备开发一个新功能，但是需要两周才能完成，第一周你写了50%的代码，如果立刻提交，由于代码还没写完，不完整的代码库会导致别人不能干活了。如果等代码全部写完再一次提交，又存在丢失每天进度的巨大风险。
    现在有了分支，就不用怕了。你创建了一个属于你自己的分支，别人看不到，还继续在原来的分支上正常工作，而你在自己的分支上干活，想提交就提交，直到开发完毕后，再一次性合并到原来的分支上，这样，既安全，又不影响别人工作。
    其他版本控制系统如SVN等都有分支管理，但是用过之后你会发现，这些版本控制系统创建和切换分支比蜗牛还慢，简直让人无法忍受，结果分支功能成了摆设，大家都不去用。
    但Git的分支是与众不同的，无论创建、切换和删除分支，Git在1秒钟之内就能完成！无论你的版本库是1个文件还是1万个文件。
## 1.创建与合并分支
    首先我们创建dev分支,然后切换到dev分支
        git checkout -b dev
    git checkout命令加上-b参数表示创建并切换，相当于以下两条命令：
        git branch dev
        git checkout dev
    然后，用git branch命令查看当前分支：
    git branch命令会列出所有分支，当前分支前面会标一个*号。
    然后，我们就可以在dev分支上正常提交，比如对README.md做个修改，加上一行,并且提交
        git add README.md 
        git commit -m "xxx"
    现在，dev分支的工作完成，我们就可以切换回master分支：
        git checkout master
    切换回master分支后，再查看一个README.md文件，刚才添加的内容不见了！因为那个提交是在dev分支上，而master分支此刻的提交点并没有变：
    现在，我们把dev分支的工作成果合并到master分支上：
        git merge dev
    git merge命令用于合并指定分支到当前分支。合并后，再查看README.md的内容，就可以看到，和dev分支的最新提交是完全一样的。
    注意到上面的Fast-forward信息，Git告诉我们，这次合并是“快进模式”，也就是直接把master指向dev的当前提交，所以合并速度非常快。
    当然，也不是每次合并都能Fast-forward，我们后面会讲其他方式的合并。
    合并完成后，就可以放心地删除dev分支了：
            git branch -d dev
    因为创建、合并和删除分支非常快，所以Git鼓励你使用分支完成某个任务，合并后再删掉分支，这和直接在master分支上工作效果是一样的，但过程更安全。
    我们注意到切换分支使用git checkout <branch>，而前面讲过的撤销修改则是git checkout -- <file>，同一个命令，有两种作用，确实有点令人迷惑。
    实际上，切换分支这个动作，用switch更科学。因此，最新版本的Git提供了新的git switch命令来切换分支：
    创建并切换到新的dev分支，可以使用：
        git switch -c dev
    直接切换到已有的master分支，可以使用：
        git switch master
    使用新的git switch命令，比git checkout要更容易理解。
    小结
        Git鼓励大量使用分支：
        查看分支：git branch
        创建分支：git branch <name>
        切换分支：git checkout <name>或者git switch <name>
        创建+切换分支：git checkout -b <name>或者git switch -c <name>
        合并某分支到当前分支：git merge <name>
        删除分支：git branch -d <name>
## 2.解决冲突
    人生不如意之事十之八九，合并分支往往也不是一帆风顺的。
    准备新的feature1分支，继续我们的新分支开发：
        git switch -c feature1
    写一些新的东西  
    Git用<<<<<<<，=======，>>>>>>>标记出不同分支的内容，我们修改如下后保存：
    选择你要的内容留下,再次提交
        git add . 
        git commit-m "解决冲突"
    现在，master分支和feature1分支又变得一样了.
    用git log --graph --pretty=oneline --abbrev-commit可以查看分支的合并情况
    最后，删除feature1分支：
        git branch -d feature1
    小结
        当Git无法自动合并分支时，就必须首先解决冲突。解决冲突后，再提交，合并完成。
        解决冲突就是把Git合并失败的文件手动编辑为我们希望的内容，再提交。
        用git log --graph命令可以看到分支合并图。
## 3.分支管理策略
    通常，合并分支时，如果可能，Git会用Fast forward模式，但这种模式下，删除分支后，会丢掉分支信息。
    如果要强制禁用Fast forward模式，Git就会在merge时生成一个新的commit，这样，从分支历史上就可以看出分支信息。
    下面我们实战一下--no-ff方式的git merge：
    首先，仍然创建并切换dev分支：
        git switch -c dev
    在该分支下修改LICENSE.txt文件,并提交一个新的commit
        git add LICENSE.txt
        git commit -m "在dev分支下修改LICENSE.txt"
    现在我们切回master分支
        git switch master
    准备合并dev分支,并且加上--no-ff禁用Fast forward模式
        git merge --no-ff -m "合并dev分支,禁用fast forward" dev
    因为本次合并要创建一个新的commit，所以加上-m参数，把commit描述写进去。
    合并后，我们用git log看看分支历史：
    git log --graph --pretty=oneline --abbrev-commit
    分支策略
    在实际开发中，我们应该按照几个基本原则进行分支管理：
    首先，master分支应该是非常稳定的，也就是仅用来发布新版本，平时不能在上面干活；
    那在哪干活呢？干活都在dev分支上，也就是说，dev分支是不稳定的，到某个时候，比如1.0版本发布时，再把dev分支合并到master上，在master分支发布1.0版本；
    你和你的小伙伴们每个人都在dev分支上干活，每个人都有自己的分支，时不时地往dev分支上合并就可以了。
    小结
        Git分支十分强大，在团队开发中应该充分应用。
        合并分支时，加上--no-ff参数就可以用普通模式合并，合并后的历史有分支，能看出来曾经做过合并，而fast forward合并就看不出来曾经做过合并。
## 4.Bug分支
    软件开发中，bug就像家常便饭一样。有了bug就需要修复，在Git中，由于分支是如此的强大，所以，每个bug都可以通过一个新的临时分支来修复，修复后，合并分支，然后将临时分支删除。
    当你接到一个修复一个代号101的bug的任务时，很自然地，你想创建一个分支issue-101来修复它，但是，等等，当前正在dev上进行的工作还没有提交：
        git status 查看当前状态
    并不是你不想提交，而是工作只进行到一半，还没法提交，预计完成还需1天时间。但是，必须在两个小时内修复该bug，怎么办？
    幸好，Git还提供了一个stash功能，可以把当前工作现场“储藏”起来，等以后恢复现场后继续工作：
        git stash 暂存状态 回到上一个commit版本
        git status
    现在，用git status查看工作区，就是干净的（除非有没有被Git管理的文件），因此可以放心地创建分支来修复bug。
    首先确定要在哪个分支上修复bug，假定需要在master分支上修复，就从master创建临时分支：
        git switch master
        git switch -c issue-101
    现在修复bug，修改LICENSE.txt内容然后提交
        git add .
        git commit -m "在issue-101分支上修复bug"
    切换回master分支并合并分支修复bug
        git switch master
        git merge --no-ff -m "合并issue-101分支,修复bug" issue-101
    合并完成后 删除issue-101分支
        git branch -d issue-101
    太棒了，原计划两个小时的bug修复只花了5分钟！现在，是时候接着回到dev分支干活了！
        git switch dev
        git status
    工作区是干净的，刚才的工作现场存到哪去了？用git stash list命令看看：
        git stash list
    工作现场还在，Git把stash内容存在某个地方了，但是需要恢复一下，有两个办法：
    一是用git stash apply恢复，但是恢复后，stash内容并不删除，你需要用git stash drop来删除；
    另一种方式是用git stash pop，恢复的同时把stash内容也删了：
        git stash pop
    再用git stash list查看，就看不到任何stash内容了：
        git stash list
    你可以多次stash，恢复的时候，先用git stash list查看，然后恢复指定的stash，用命令：
        git stash apply stash@{0}
    同样的bug，要在dev上修复，我们只需要把0dd39f4  在issue-101上修复bug 101分支这个提交所做的修改“复制”到dev分支。注意：我们只想复制4c805e2 fix bug 101这个提交所做的修改，并不是把整个master分支merge过来。
    为了方便操作，Git专门提供了一个cherry-pick命令，让我们能复制一个特定的提交到当前分支：
        git branch
        git cherry-pick 0dd39f4
    Git自动给dev分支做了一次提交，注意这次提交的commit是0dd39f4，它并不同于master的71850c1，因为这两个commit只是改动相同，但确实是两个不同的commit。用git cherry-pick，我们就不需要在dev分支上手动再把修bug的过程重复一遍。
    有些聪明的童鞋会想了，既然可以在master分支上修复bug后，在dev分支上可以“重放”这个修复过程，那么直接在dev分支上修复bug，然后在master分支上“重放”行不行？当然可以，不过你仍然需要git stash命令保存现场，才能从dev分支切换到master分支。
    小结
        修复bug时，我们会通过创建新的bug分支进行修复，然后合并，最后删除；
        当手头工作没有完成时，先把工作现场git stash一下，然后去修复bug，修复后，再git stash pop，回到工作现场；
        在master分支上修复的bug，想要合并到当前dev分支，可以用git cherry-pick <commit>命令，把bug提交的修改“复制”到当前分支，避免重复劳动。
## 5.Feature分支
    软件开发中，总有无穷无尽的新的功能要不断添加进来。
    添加一个新功能时，你肯定不希望因为一些实验性质的代码，把主分支搞乱了，所以，每添加一个新功能，最好新建一个feature分支，在上面开发，完成后，合并，最后，删除该feature分支。
    现在，你终于接到了一个新任务：开发代号为Vulcan的新功能，该功能计划用于下一代星际飞船。
    于是准备开发：
        git switch -c feature-Vulcan
    五分钟后开发完毕
        git add README.md
        git commit -m "新功能开发完毕"
    切回dev 准备合并
        git switch dev
    一切顺利的话，feature分支和bug分支是类似的，合并，然后删除。
    但是！
    就在此时，接到上级命令，因经费不足，新功能必须取消！
    虽然白干了，但是这个包含机密资料的分支还是必须就地销毁：
        git branch -d feature-vulcan
    销毁失败。Git友情提醒，feature-vulcan分支还没有被合并，如果删除，将丢失掉修改，如果要强行删除，需要使用大写的-D参数。。
    现在我们强行删除
         git branch -D feature-vulcan
    小结
    开发一个新feature，最好新建一个分支；
    如果要丢弃一个没有被合并过的分支，可以通过git branch -D <name>强行删除。
## 6.多人协作
    当你从远程仓库克隆时，实际上Git自动把本地的master分支和远程的master分支对应起来了，并且，远程仓库的默认名称是origin。
    要查看远程库的信息，用git remote：
        git remote
    或者，用git remote -v显示更详细的信息
        git remote -v
    上面显示了可以抓取和推送的origin的地址。如果没有推送权限，就看不到push的地址。
    推送分支
    推送分支，就是把该分支上的所有本地提交推送到远程库。推送时，要指定本地分支，这样，Git就会把该分支推送到远程库对应的远程分支上：
    git push originXXX master
    如果要推送其他分支，比如dev，就改成：
    git push originXXX dev
    但是，并不是一定要把本地分支往远程推送，那么，哪些分支需要推送，哪些不需要呢？
        master分支是主分支，因此要时刻与远程同步；
        dev分支是开发分支，团队所有成员都需要在上面工作，所以也需要与远程同步；
        bug分支只用于在本地修复bug，就没必要推到远程了，除非老板要看看你每周到底修复了几个bug；
        feature分支是否推到远程，取决于你是否和你的小伙伴合作在上面开发。
    总之，就是在Git中，分支完全可以在本地自己藏着玩，是否推送，视你的心情而定！
    抓取分支
    多人协作时，大家都会往master和dev分支上推送各自的修改。
    现在，模拟一个你的小伙伴，可以在另一台电脑（注意要把SSH Key添加到GitHub）或者同一台电脑的另一个目录下克隆：
    当你的小伙伴从远程库clone时，默认情况下，你的小伙伴只能看到本地的master分支。不信可以用git branch命令看看：
      小伙伴:  
        git clone git@github.com:banchengxuezhang/learngit.git
        git branch
    现在，你的小伙伴要在dev分支上开发，就必须创建远程origin的dev分支到本地，于是他用这个命令创建本地dev分支：
        小伙伴:
            git checkout -b dev origin/dev
    现在，他就可以在dev上继续修改，然后，时不时地把dev分支push到远程：
        小伙伴
            git add LICENSE.txt
            git commit -m "在dev分支上做了一些修改"
            git push origin dev 推送修改到dev分支上
    你的小伙伴已经向origin/dev分支推送了他的提交，而碰巧你也对同样的文件作了修改，并试图推送：
        我:
            git add LICENSE.txt
            git commit -m "我在dev分支上做了一些修改"
            git push origin dev 推送修改到dev分支上
    推送失败，因为你的小伙伴的最新提交和你试图推送的提交有冲突，解决办法也很简单，Git已经提示我们，先用git pull把最新的提交从origin/dev抓下来，然后，在本地合并，解决冲突，再推送：
        git pull
    git pull也失败了，原因是没有指定本地dev分支与远程origin/dev分支的链接，根据提示，设置dev和origin/dev的链接：
        git branch --set-upstream-to=origin/dev dev
    再pull
        git pull
    这回git pull成功，但是合并有冲突，需要手动解决，解决的方法和分支管理中的解决冲突完全一样。解决后，提交，再push：
        git add LICENSE.txt
        git commit -m "解决冲突"
        git push origin dev
    因此，多人协作的工作模式通常是这样：
    1.首先，可以试图用git push origin <branch-name>推送自己的修改；
    2.如果推送失败，则因为远程分支比你的本地更新，需要先用git pull试图合并；
    3.如果合并有冲突，则解决冲突，并在本地提交；
    没有冲突或者解决掉冲突后，再用git push origin <branch-name>推送就能成功！
    4.如果git pull提示no tracking information，则说明本地分支和远程分支的链接关系没有创建，用命令git branch --set-upstream-to <branch-name> origin/<branch-name>。
    这就是多人协作的工作模式，一旦熟悉了，就非常简单。
    小结
        查看远程库信息，使用git remote -v；
        本地新建的分支如果不推送到远程，对其他人就是不可见的；
        从本地推送分支，使用git push origin branch-name，如果推送失败，先用git pull抓取远程的新提交；
        在本地创建和远程分支对应的分支，使用git checkout -b branch-name origin/branch-name，本地和远程分支的名称最好一致；
        建立本地分支和远程分支的关联，使用git branch --set-upstream branch-name origin/branch-name；
        从远程抓取分支，使用git pull，如果有冲突，要先处理冲突。
## 7.Rebase
    在上一节我们看到了，多人在同一个分支上协作时，很容易出现冲突。即使没有冲突，后push的童鞋不得不先pull，在本地合并，然后才能push成功。
    https://www.liaoxuefeng.com/wiki/896043488029600/1216289527823648
    小结
        rebase操作可以把本地未push的分叉提交历史整理成直线；
        rebase的目的是使得我们在查看历史提交的变化时更容易，因为分叉的提交需要三方对比。
# 标签管理
    发布一个版本时，我们通常先在版本库中打一个标签（tag），这样，就唯一确定了打标签时刻的版本。将来无论什么时候，取某个标签的版本，就是把那个打标签的时刻的历史版本取出来。所以，标签也是版本库的一个快照。
    Git的标签虽然是版本库的快照，但其实它就是指向某个commit的指针（跟分支很像对不对？但是分支可以移动，标签不能移动），所以，创建和删除标签都是瞬间完成的。
    Git有commit，为什么还要引入tag？
    “请把上周一的那个版本打包发布，commit号是6a5819e...”
    “一串乱七八糟的数字不好找！”
    如果换一个办法：
    “请把上周一的那个版本打包发布，版本号是v1.2”
    “好的，按照tag v1.2查找commit就行！”
    所以，tag就是一个让人容易记住的有意义的名字，它跟某个commit绑在一起。
## 1.创建标签            
    在Git中打标签非常简单，首先，切换到需要打标签的分支上：
        git switch master
    然后，敲命令git tag <name>就可以打一个新标签：
        git tag v1.0
    可以用命令git tag查看所有标签：
        git tag
    默认标签是打在最新提交的commit上的。有时候，如果忘了打标签，比如，现在已经是周五了，但应该在周一打的标签没有打，怎么办？
        git log --pretty=oneline --abbrev-commit
    方法是找到历史提交的commit id，然后打上就可以了：
    比方说要对"提交修改"这次提交打标签，它对应的commit id是b5eb443 ，敲入命令： git tag v0.9 b5eb443 
    再用命令git tag查看标签：
        git tag
    注意，标签不是按时间顺序列出，而是按字母排序的。可以用git show <tagname>查看标签信息：
    可以看到，v0.9确实打在add merge这次提交上。
    还可以创建带有说明的标签，用-a指定标签名，-m指定说明文字：
        git tag -a v0.1 -m "version 0.1 released" 1094adb
    用命令git show <tagname>可以看到说明文字
        git show v0.1
    注意：标签总是和某个commit挂钩。如果这个commit既出现在master分支，又出现在dev分支，那么在这两个分支上都可以看到这个标签。
    小结
        命令git tag <tagname>用于新建一个标签，默认为HEAD，也可以指定一个commit id；
        命令git tag -a <tagname> -m "blablabla..."可以指定标签信息；
        命令git tag可以查看所有标签。
## 2.操作标签
    如果标签打错了,也可以删除
        git tag -d v0.1
    因为创建的标签都只存储在本地，不会自动推送到远程。所以，打错的标签可以在本地安全删除。
    如果要推送某个标签到远程，使用命令git push origin <tagname>：
        git push origin v0.1
    或者，一次性推送全部尚未推送到远程的本地标签：
        git push origin --tags
    如果标签已经推送到远程，要删除远程标签就麻烦一点，先从本地删除：
        git tag -d v0.09
    然后，从远程删除。删除命令也是push，但是格式如下：
        git push origin :refs/tags/v0.09
    要看看是否真的从远程库删除了标签，可以登陆GitHub查看。
    小结
        命令git push origin <tagname>可以推送一个本地标签；
        命令git push origin --tags可以推送全部未推送过的本地标签；
        命令git tag -d <tagname>可以删除一个本地标签；
        命令git push origin :refs/tags/<tagname>可以删除一个远程标签。
# 使用GitHub
    https://www.liaoxuefeng.com/wiki/896043488029600/900937935629664
    我们一直用GitHub作为免费的远程仓库，如果是个人的开源项目，放到GitHub上是完全没有问题的。其实GitHub还是一个开源协作社区，通过GitHub，既可以让别人参与你的开源项目，也可以参与别人的开源项目。
    在GitHub出现以前，开源项目开源容易，但让广大人民群众参与进来比较困难，因为要参与，就要提交代码，而给每个想提交代码的群众都开一个账号那是不现实的，因此，群众也仅限于报个bug，即使能改掉bug，也只能把diff文件用邮件发过去，很不方便。
    但是在GitHub上，利用Git极其强大的克隆和分支功能，广大人民群众真正可以第一次自由参与各种开源项目了。
    如何参与一个开源项目呢？比如人气极高的bootstrap项目，这是一个非常强大的CSS框架，你可以访问它的项目主页https://github.com/twbs/bootstrap，点“Fork”就在自己的账号下克隆了一个bootstrap仓库，然后，从自己的账号下clone：
        git clone git@github.com:banchengxuezhang/bootstrap.git
    一定要从自己的账号下clone仓库，这样你才能推送修改。如果从bootstrap的作者的仓库地址git@github.com:twbs/bootstrap.git克隆，因为没有权限，你将不能推送修改。
    Bootstrap的官方仓库twbs/bootstrap、你在GitHub上克隆的仓库my/bootstrap，以及你自己克隆到本地电脑的仓库，他们的关系就像下图显示的那样：

    ┌─ GitHub ────────────────────────────────────┐
    │                                             │
    │ ┌─────────────────┐     ┌─────────────────┐ │
    │ │ twbs/bootstrap  │────>│  my/bootstrap   │ │
    │ └─────────────────┘     └─────────────────┘ │
    │                                  ▲          │
    └──────────────────────────────────┼──────────┘
                                   ▼
                          ┌─────────────────┐
                          │ local/bootstrap │
                          └─────────────────┘
    如果你想修复bootstrap的一个bug，或者新增一个功能，立刻就可以开始干活，干完后，往自己的仓库推送。
    如果你希望bootstrap的官方库能接受你的修改，你就可以在GitHub上发起一个pull request。当然，对方是否接受你的pull request就不一定了。
    如果你没能力修改bootstrap，但又想要试一把pull request，那就Fork一下我的仓库：https://github.com/michaelliao/learngit，创建一个your-github-id.txt的文本文件，写点自己学习Git的心得，然后推送一个pull request给我，我会视心情而定是否接受。
    小结
        在GitHub上，可以任意Fork开源仓库；
        自己拥有Fork后的仓库的读写权限；
        可以推送pull request给官方仓库来贡献代码。
# Gitee
    https://www.liaoxuefeng.com/wiki/896043488029600/1163625339727712
    使用GitHub时，国内的用户经常遇到的问题是访问速度太慢，有时候还会出现无法连接的情况（原因你懂的）。
    如果我们希望体验Git飞一般的速度，可以使用国内的Git托管服务——Gitee（gitee.com）。
    和GitHub相比，Gitee也提供免费的Git仓库。此外，还集成了代码质量检测、项目演示等功能。对于团队协作开发，Gitee还提供了项目管理、代码托管、文档管理的服务，5人以下小团队免费。
    Gitee的免费版本也提供私有库功能，只是有5人的成员上限。
    使用Gitee和使用GitHub类似，我们在Gitee上注册账号并登录后，需要先上传自己的SSH公钥。选择右上角用户头像 -> 菜单“修改资料”，然后选择“SSH公钥”，填写一个便于识别的标题，然后把用户主目录下的.ssh/id_rsa.pub文件的内容粘贴进去：
    点击“确定”即可完成并看到刚才添加的Key：
    如果我们已经有了一个本地的git仓库（例如，一个名为learngit的本地库），如何把它关联到Gitee的远程库上呢？
    首先，我们在Gitee上创建一个新的项目，选择右上角用户头像 -> 菜单“控制面板”，然后点击“创建项目”：
    项目名称最好与本地库保持一致：
    然后，我们在本地库上使用命令git remote add把它和Gitee的远程库关联：
        git remote add origin git@gitee.com:banchengxuezhang/learngit.git
    之后，就可以正常地用git push和git pull推送了！
    如果在使用命令git remote add时报错：
    这说明本地库已经关联了一个名叫origin的远程库，此时，可以先用git remote -v查看远程库信息：
    可以看到，本地库已经关联了origin的远程库，并且，该远程库指向GitHub。
    我们可以删除已有的GitHub远程库：
    再关联Gitee的远程库（注意路径中需要填写正确的用户名）：
    此时，我们再查看远程库信息：
    现在可以看到，origin已经被关联到Gitee的远程库了。通过git push命令就可以把本地库推送到Gitee上。
    有的小伙伴又要问了，一个本地库能不能既关联GitHub，又关联Gitee呢？
    答案是肯定的，因为git本身是分布式版本控制系统，可以同步到另外一个远程库，当然也可以同步到另外两个远程库。
    使用多个远程库时，我们要注意，git给远程库起的默认名称是origin，如果有多个远程库，我们需要用不同的名称来标识不同的远程库。
    仍然以learngit本地库为例，我们先删除已关联的名为origin的远程库：
            git remote rm origin
            git remote add github git@github.com:banchengxuezhang/learngit.git
            git remote add gitee git@gitee.com:banchengxuezhang/learngit.git
            git remote -v
    如果要推送到GitHub，使用命令：
        git push github master
    如果要推送到Gitee，使用命令：
        git push gitee master
    
    这样一来，我们的本地库就可以同时与多个远程库互相同步：

        ┌─────────┐ ┌─────────┐
        │ GitHub  │ │  Gitee  │
        └─────────┘ └─────────┘
             ▲           ▲
             └─────┬─────┘
                   │
            ┌─────────────┐
            │ Local Repo  │
            └─────────────┘
    Gitee也同样提供了Pull request功能，可以让其他小伙伴参与到开源项目中来。你可以通过Fork我的仓库：https://gitee.com/liaoxuefeng/learngit，创建一个your-gitee-id.txt的文本文件， 写点自己学习Git的心得，然后推送一个pull request给我，这个仓库会在Gitee和GitHub做双向同步。
# 自定义Git
    在安装Git一节中，我们已经配置了user.name和user.email，实际上，Git还有很多可配置项。
    比如，让Git显示颜色，会让命令输出看起来更醒目：
        git config --global color.ui true
    这样，Git会适当地显示不同的颜色，比如git status命令：
    文件名就会标上颜色。
    我们在后面还会介绍如何更好地配置Git，以便让你的工作更高效。
## 1.忽略特殊文件
    https://www.liaoxuefeng.com/wiki/896043488029600/900004590234208
    有些时候，你必须把某些文件放到Git工作目录中，但又不能提交它们，比如保存了数据库密码的配置文件啦，等等，每次git status都会显示Untracked files ...，有强迫症的童鞋心里肯定不爽。
    好在Git考虑到了大家的感受，这个问题解决起来也很简单，在Git工作区的根目录下创建一个特殊的.gitignore文件，然后把要忽略的文件名填进去，Git就会自动忽略这些文件。
    不需要从头写.gitignore文件，GitHub已经为我们准备了各种配置文件，只需要组合一下就可以使用了。所有配置文件可以直接在线浏览：https://github.com/github/gitignore
    忽略文件的原则是：
        1.忽略操作系统自动生成的文件，比如缩略图等；
        2.忽略编译生成的中间文件、可执行文件等，也就是如果一个文件是通过另一个文件自动生成的，那自动生成的文件就没必要放进版本库，比如Java编译产生的.class文件；
        3.忽略你自己的带有敏感信息的配置文件，比如存放口令的配置文件。
    举个例子：
    假设你在Windows下进行Python开发，Windows会自动在有图片的目录下生成隐藏的缩略图文件，如果有自定义目录，目录下就会有Desktop.ini文件，因此你需要忽略Windows自动生成的垃圾文件
        # Windows:
        Thumbs.db
        ehthumbs.db
        Desktop.ini

        # Python:
        *.py[cod]
        *.so
        *.egg
        *.egg-info
        dist
        build

        # My configurations:
        db.ini
        deploy_key_rsa
    最后一步就是把.gitignore也提交到Git，就完成了！当然检验.gitignore的标准是git status命令是不是说working directory clean。

    使用Windows的童鞋注意了，如果你在资源管理器里新建一个.gitignore文件，它会非常弱智地提示你必须输入文件名，但是在文本编辑器里“保存”或者“另存为”就可以把文件保存为.gitignore了。

    有些时候，你想添加一个文件到Git，但发现添加不了，原因是这个文件被.gitignore忽略了：

    小结
        规则示例
        # 排除所有.开头的隐藏文件:
        .*
        # 排除所有.class文件:
        *.class

        # 不排除.gitignore和App.class:
        !.gitignore
        !App.class
        
        忽略某些文件时，需要编写.gitignore；
        .gitignore文件本身要放到版本库里，并且可以对.gitignore做版本管理！