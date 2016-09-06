###### 在[github](https://github.com/)创建一个项目，比如创建名为[randomNickname](https://github.com/ubuntuvim/randomNickname)的项目，
这个代码库用户存放npm插件的代码。
###### 首先在npm[官网](https://www.npmjs.com/)申请账号，后面提交到npm官网需要用到。
###### `mkdir npmjs`
###### `cd npmjs`
###### `npm init`
###### 根据提示输入相应的信息，可用参考如下设置。
```shell
ubuntuvimdeMacBook-Pro:randomNichname ubuntuvim$ npm init
This utility will walk you through creating a package.json file.
It only covers the most common items, and tries to guess sensible defaults.

See `npm help json` for definitive documentation on these fields
and exactly what they do.

Use `npm install <pkg> --save` afterwards to install a package and
save it as a dependency in the package.json file.

Press ^C at any time to quit.
name: (randomNichname)
Sorry, name can no longer contain capital letters.
name: (randomNichname) randomNichname
Sorry, name can no longer contain capital letters.
name: (randomNichname) y
version: (1.0.0) 0.1.0
description: Get the name of the three kingdoms.
entry point: (index.js) y
test command:
git repository: https://github.com/ubuntuvim/randomNichname.git
keywords: nickname,random
author: ubuntuvim
license: (ISC) ISC
About to write to /Users/ubuntuvim/codes/my-npm-plugins/randomNichname/package.json:

{
  "name": "y",
  "version": "0.1.0",
  "description": "Get the name of the three kingdoms.",
  "main": "y",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "repository": {
    "type": "git",
    "url": "git+https://github.com/ubuntuvim/randomNichname.git"
  },
  "keywords": [
    "nickname",
    "random"
  ],
  "author": "ubuntuvim",
  "license": "ISC",
  "bugs": {
    "url": "https://github.com/ubuntuvim/randomNichname/issues"
  },
  "homepage": "https://github.com/ubuntuvim/randomNichname#readme"
}


Is this ok? (yes) yes
ubuntuvimdeMacBook-Pro:randomNichname ubuntuvim$
```
创建完成package.json之后你也可以根据实际情况做修改。

###### 创建npm插件所需的目录和文件，目录结构如下：

```
npmjs
├─┬ lib
│ └── npmjs.js
├─┬ test
│ └── test.js
├── .gitignore
├── .npmignore
├── .travis.yml
├── index.js
├── LICENSE
├── makefile
├── package.json
├── README.md
```
这些文件的作用是：

* lib目录下存放业务逻辑文件
* test目录下存放单元测试用例
* .npmignore记录哪些文件不需要被发布到npmjs.org
* .travis.yml是持续集成服务travis的描述文件
* index.js是入口文件
* makefile方便我们用make test进行测试
* README.md是此module的描述和使用方法

###### 编写插件代码
主要代码直接放在`index.js`。比如本插件是用于获取随机的三国人物名称，代码如下(详细代码请从[github](https://github.com/ubuntuvim/randomNickname/blob/master/index.js)下载)：
```javascript
//  从1099个名字中获取任意名字
(function () {
  'use strict';
    //  一共1099个
    var arr = [];
    //  获取随机名字
    function getNickname() {
        //  取0~1099的随机数
        var random = Math.floor(Math.random() * 1099);
        if (random >= 1099)
            throw new Error("获取人名数组下标月结！");
        return arr[random];
    }
    exports.getNickname = getNickname;
  }());
```
###### 测试
```javascript
var arr = require('../index');
var s = arr.getNickname();
if (s) {
    console.log(s);
} else {
    throw new Error('The test does not pass...');
}
```

运行测试。
`node ./test/test.js`

###### 在LICENSE文件中添加许可信息

许可内容请从[github](https://github.com/ubuntuvim/randomNickname/blob/master/LICENSE)下载。

###### 编写使用文档README.md

**插件使用方式**

1. 安装插件
`npm install randomNickname`

2. 使用插件
```javascript
var names = require('randomNickname');
var s = names.getNickname();
console.log('得到的人物名字为： ' + s);
```

###### 提交代码到github

步骤如下：

* `git init`
* `git remote add origin <git远程URL>`
* `git add *`
* `git commit -am '描述信息'`
* `git push origin master`
_如果出现类似如下错误_

```shell
! [rejected]        master -> master (non-fast-forward)
error: failed to push some refs to 'https://github.com/ubuntuvim/randomNickname.git'
hint: Updates were rejected because the tip of your current branch is behind
hint: its remote counterpart. Integrate the remote changes (e.g.
hint: 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```

先更新合并再提交到远程代码库。

* `git pull`
* `git merge origin/master`
* `git push origin/master`

###### 发布到npmjs官网

步骤如下：

* `npm adduser`   (输入你在npmjs官网注册的账号和密码)
* `npm publish .`

如果出现错误：
`no_perms Private mode enable, only admin can publish this module`
执行下列代码重置后再执行`npm adduser`、`npm publish .`。
```
npm config set registry http://registry.npmjs.org
```

等待完成，看到如下信息说明发布成功。
```
+ randomnickname@0.1.0
```

_如果你的代码修改了又需要重新发布到npmjs上，则首先修改`package.json`里的版本号，再执行`npm publish .`即可。_

此时你可以到npmjs的个人中心中查看刚刚发布的插件。
比如我的个人中心[https://www.npmjs.com/~ubuntuvim](https://www.npmjs.com/~ubuntuvim)。

**参考文献：**

1. [http://segmentfault.com/a/1190000000491188](http://segmentfault.com/a/1190000000491188)
2. [http://blog.csdn.net/liuxiao723846/article/details/46237289](http://blog.csdn.net/liuxiao723846/article/details/46237289)
3. [http://www.lellansin.com/npm-publish-%E5%8F%91%E5%B8%83%E7%A4%BA%E4%BE%8B.html](http://www.lellansin.com/npm-publish-%E5%8F%91%E5%B8%83%E7%A4%BA%E4%BE%8B.html)
