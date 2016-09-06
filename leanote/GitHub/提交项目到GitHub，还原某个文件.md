新创建到项目如何提交到[GitHub](http://github.com/ubuntuvim)呢？

### 前提

本地需要安装[Git](https://git-scm.com/)。安装方法请参考官网文档，或者谷歌搜搜！！

### 步骤

1. 创建本地项目
2. 执行git命令初始化项目为git项目，命令：`git init`。
3. 修改项目下到文件`.gitignore`，如果没用请自己创建，在文件内执行那些文件不需要提交到GitHub。比如`node_modules`已经项目独有到配置可以不提交。
4. 链接远程git地址，执行命令：` git remote add origin https://github.com/ubuntuvim/mongodb_proj.git`。
5. 更新，在提交之前最好先更新，命令为：`git fetch`和`git merge origin/master`
6. 提交项目文件到本地git库，命令：`git add *`然后再执行`git commit -am "备注信息"`。`-a`表示提交所有，如果只是提交某个文件可以不加这个参数，但是要制定提交到文件是那个，比如：`git commit app.js -m "备注信息"
7. 同步到远程到GitHub，命令：`git push origin master`。如果是第一次提交需要配置用户名和邮箱，配置命令为：`git config --global user.name "ubuntuvim"`和`
git config --global user.email "1527254027@qq.com" `，根据自己注册github账户的用户名和邮箱配置。
8. 指定用户名和邮箱之后再次执行`git push origin master`，过程中需要输入你在GitHub的账户和密码，请输入自己注册用户名和密码。 
9. 等待提交完成，然后到GitHub的响应项目下，可以查看到刚刚提交的内容。

## 提交到github方法二

### …or create a new repository on the command line

```
echo "# ddlisting" >> README.md
git init  //如果是ember cli命令创建的项目不需要执行这步
git add README.md
git commit -m "first commit"
git remote add origin https://github.com/ubuntuvim/ddlisting.git
git push -u origin master
```
### …or push an existing repository from the command line(强制提交，覆盖原有文件)
```
git remote add origin https://github.com/ubuntuvim/ddlisting.git
git push -u origin master
```

## git还原文件

如果你想还原某个文件到最新版本可以使用如下命令实现。
```shell
git checkout fileName
```

还原一个版本的修改，必须提供一个具体的Git版本号，例如'git revert bbaf6fb5060b4875b18ff9ff637ce118256d6f20'，可以先通过git log查询版本号的哈希值。
```SHELL
git revert 版本号
```

## 强制更新覆盖本地文件
```shell
git fetch --all  
git reset --hard origin/master 
git pull
```
如果出现问题可以使用下面的命令强制更新远程库到本地。
```shell
git pull https://github.com/ubuntuvim/ddlisting.git master
```

## 克隆某个分支
下面的命令是克隆分支master    
```bash
git clone -b master http://xxxx/xx.git
```