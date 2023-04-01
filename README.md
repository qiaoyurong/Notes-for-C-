# Notes-for-C-
This part is used to store notes in the process of C++ learning
如何添加一个文件从本地仓库到远程仓库
1、在本地创建一个新文件夹，比如说叫做 "work"，然后进入该文件夹。

2、在 "work" 文件夹中初始化 Git 仓库，可以通过执行以下命令来实现：



git init
将本地仓库与 GitHub 仓库关联起来，需要先在 GitHub 上创建一个仓库。

假设你的 GitHub 用户名是 "your_username"，那么可以执行以下命令：
git remote add origin https://github.com/your_username/your_repository.git
这里的 "your_repository" 是你在 GitHub 上创建的仓库的名称，origin为你的本地仓库名称。

添加需要上传的文件到本地仓库中，可以执行以下命令：

git add .
这里的 "." 表示添加所有文件到本地仓库中。

提交本地仓库的修改到本地仓库中的主分支中，可以执行以下命令：
git commit -m "Initial commit"
这里的 "-m" 表示添加提交信息，"Initial commit" 是你自己定义的提交信息。

将本地仓库的修改推送到 GitHub 上的主分支中，可以执行以下命令：
git push -u origin main  #git push - u 本地仓库名 本地仓库分支名
这里的 "-u" 表示将本地仓库的主分支与 GitHub 上的主分支相关联，以后每次提交可以直接使用 "git push" 命令。


完成以上步骤后，你就可以将工作文件夹中的文件上传到 GitHub 仓库中了。
