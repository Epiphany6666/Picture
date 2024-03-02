【Git】merge时报错：refusing to merge unrelated histories

# 问题

今天将feature分支合并到master时报错：refusing to merge unrelated histories（拒绝合并无关历史）

![image-20240302163746403](https://cdn.jsdelivr.net/gh/Epiphany6666/Picture/blog/%E3%80%90Git%E3%80%91merge%E6%97%B6%E6%8A%A5%E9%94%99%EF%BC%9Arefusing%20to%20merge%20unrelated%20histories/assets/202403021637539.png)

报错原因：当尝试从远程仓库"gitee.com:zpg13/system_school"的master分支拉取最新更新并合并到本地的master分支时，Git拒绝了这次合并，原因是两个分支拥有不相关的历史记录。

---

# 解决办法

## 1、将feature分支的东西追加到master分支中

这种方法会保留master分支里原本的内容，并且 future的历史记录 会 合并到 master的历史记录中

![image-20240302172517925](https://cdn.jsdelivr.net/gh/Epiphany6666/Picture/blog/%E3%80%90Git%E3%80%91merge%E6%97%B6%E6%8A%A5%E9%94%99%EF%BC%9Arefusing%20to%20merge%20unrelated%20histories/assets/202403021725992.png)

步骤：

首先切换到master分支

~~~bash
git checkout master
~~~

然后在执行merge指令的时候添加上： --allow-unrelated-histories 参数

~~~bash
git merge feature --allow-unrelated-histories
~~~

> 执行`git merge feature --allow-unrelated-histories`命令后，可能会出现以下界面：Git要求您提供一个提交消息来解释为什么这次合并是必要的。
>
> 请在编辑器中输入您想要的提交消息，然后按`Esc`键退出编辑模式，再按`:wq`输入并按`Enter`键保存并退出Vim编辑器。如果您不想进行任何合并操作，只需按`Esc`键退出编辑模式，然后在命令行中输入`:q`并按`Enter`键退出Vim编辑器。
>
> 请注意，如果存在合并冲突，您需要先解决这些冲突，然后再继续合并操作。

![image-20240302174404315](https://cdn.jsdelivr.net/gh/Epiphany6666/Picture/blog/%E3%80%90Git%E3%80%91merge%E6%97%B6%E6%8A%A5%E9%94%99%EF%BC%9Arefusing%20to%20merge%20unrelated%20histories/assets/202403021744446.png)

此时就会提示合并成功，然后正常推送到远程分支即可

![image-20240302165955022](https://cdn.jsdelivr.net/gh/Epiphany6666/Picture/blog/%E3%80%90Git%E3%80%91merge%E6%97%B6%E6%8A%A5%E9%94%99%EF%BC%9Arefusing%20to%20merge%20unrelated%20histories/assets/202403021659129.png)

---

## 2、将feature里的东西直接覆盖到master分支中

这种方法会丢失master中的所有数据，并且将feature中的历史记录同步到master的历史记录中

先来看看合并前feature的历史记录：

![image-20240302173830961](https://cdn.jsdelivr.net/gh/Epiphany6666/Picture/blog/%E3%80%90Git%E3%80%91merge%E6%97%B6%E6%8A%A5%E9%94%99%EF%BC%9Arefusing%20to%20merge%20unrelated%20histories/assets/202403021738202.png)

然后再看看合并前master的历史记录：

![image-20240302173926006](https://cdn.jsdelivr.net/gh/Epiphany6666/Picture/blog/%E3%80%90Git%E3%80%91merge%E6%97%B6%E6%8A%A5%E9%94%99%EF%BC%9Arefusing%20to%20merge%20unrelated%20histories/assets/202403021739170.png)

合并过程：

首先切换到master分支

~~~bash
git checkout master
~~~

然后使用以下命令进行合并

~~~bash
git reset --hard origin/feature
~~~

![image-20240302175248302](https://cdn.jsdelivr.net/gh/Epiphany6666/Picture/blog/%E3%80%90Git%E3%80%91merge%E6%97%B6%E6%8A%A5%E9%94%99%EF%BC%9Arefusing%20to%20merge%20unrelated%20histories/assets/202403021752462.png)

再次查看master分支的日志时可以发现，master中的历史记录已经替换成了feature的历史记录

![image-20240302175349597](https://cdn.jsdelivr.net/gh/Epiphany6666/Picture/blog/%E3%80%90Git%E3%80%91merge%E6%97%B6%E6%8A%A5%E9%94%99%EF%BC%9Arefusing%20to%20merge%20unrelated%20histories/assets/202403021753687.png)

然后推送的时候使用 -f 参数，强推到远程仓库即可

~~~bash
git push origin master -f
~~~