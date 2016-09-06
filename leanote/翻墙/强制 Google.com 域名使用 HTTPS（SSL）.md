[http://laod.cn/hosts/suggestions-for-google-hosts-https-ssl.html](http://laod.cn/hosts/suggestions-for-google-hosts-https-ssl.html)


配合 Google Hosts 的推荐操作
最近发现好多人说谷歌学术访问不了了，而我发现他们绝大多数都使用的是http访问，而不是https（SSL加密），如 http://scholar.google.com 肯定打不开，而 https://scholar.google.com 却正常，为了保证您能更顺利的使用 Google 在线服务，我们推荐您按照以下的步骤进行一些设置。

以下的步骤将以 Chrome 浏览器为例。

强制 Google.com 域名使用 HTTPS（SSL）-老D

虽然您可以随时自行添加 HTTPS 的前缀，但是在某些情况下网页总是会自动跳到没有加密的状态，这些状态可能出现在 Google 各个网页之间的跳转，以及搜索结果链接点击之后，这会影响您的正常使用体验，因此我们建议您强制 HTTPS 连接。

在 Chrome 浏览器上输入链接 chrome://net-internals/#hsts （可复制此地址粘贴到地址栏），回车。

在 Domain 栏里，输入 google.com ，并勾选下面的两个复选框，点击 “Add” 按钮即可。

强制 Google.com 域名使用 HTTPS（SSL）-老D

此时，您无需每次在因为跳回非加密连接中断后手动添加 HTTPS 前缀。

 

强制 Google.com.hk 域名使用 HTTPS
考虑到国内用到最多的是 google.com.hk，您需要对此域名同样增加此规则。

重复上面描述的操作，唯一的不同是，请在 Domain 栏里输入 google.com.hk 。

同理，如果你喜欢用google.com就设置为：google.com

强制 Google 网页快照使用 HTTPS
默认情况下，Google 网页快照是以非加密的 HTTP 连接打开的，这将导致它直接无法连接。要改变这一情况，请为此快照域名也添加强制 HTTPS 的规则。

重复上面描述的操作，唯一的不同是，请在 Domain 栏里输入 googleusercontent.com 。

 

强制 Google APIs 使用 HTTPS
某些网站加载 Google CDN 的方式可能是普通的 HTTP 连接。使用此方法强制转为 HTTPS 连接。

重复上面描述的操作，唯一的不同是，请在 Domain 栏里输入 googleapis.com 。

 

 

对于其他的浏览器
（个人推荐使用Chrome 浏览器）

请在对应浏览器的插件目录中查找类似 “HTTPS Everywhere” 的插件，它将自动帮助您强制所有可能的 HTTPS 连接。

 

如何撤销上述更改
倘若您想要撤销上述变更，例如您想要取消对 Google.com 的强制 HTTPS 连接，请同样前往 chrome://net-internals/#hsts，这一次，在下方的 “Delete Domain” 区域里，在 Domain 栏里输入您想要撤销规则的域名（例如 google.com），并点击 “Delete” 按钮即可。