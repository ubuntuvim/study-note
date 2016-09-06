### 设置

在同步之前，需要创建一个远程点指向上游仓库(repo).如果你已经派生了一个原始仓库，可以按照如下方法做。

```
git remote add upstream 被fork的项目地址
```

### 同步

同步上游仓库到你的仓库需要执行两步：首先你需要从远程拉去，之后你需要合并你希望的分支到你的本地副本分支。

### 拉取

从远程仓库拉取将取回其分支以及各自的提交。它们将存储在你本地仓库的指定分之下。
```
git fetch upstream
```

###合并

现在我们已经拉取了上游仓库，我们将要合并其变更到我们的本地分支。这将使该分支与上游同步，而不会失去我们的本地更改。

```
git checkout master
git merge upstream/master
```

如果您的本地分支没有任何独特的提交，Git会改为执行“fast-forward”。

**最后将本地变更推送到远程服务器即可。**

**参考**

[https://help.github.com/articles/syncing-a-fork](https://help.github.com/articles/syncing-a-fork)