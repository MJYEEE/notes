# 利用Hugo搭建静态个人博客
本文主要介绍本Blog的搭建方法，利用了hugo和Nginx在腾讯云上搭建。

---

## 前提环境条件
- git
- Go
- hugo
- Nginx
- 一台腾讯云轻量应用服务器（可选备案域名）
- 一台能用的本地电脑
## 本地搭建过程
本文的本地和服务器实验系统均为Ubuntu20.04。
### 安装 git
git官网 https://git-scm.com/download/linux
ubuntu下快速安装方式`sudo apt-get install git`

### 安装 go

- **下载包**
golang官网：https://golang.org/dl/ （进不去的话请使用国内镜像或魔法）
下载对应版本的go，我选择的是 go1.22.4.linux-amd64.tar.gz

- **解压**
进入包目录，然后解压到/uer/local下
```
sudo tar -C /usr/local -xzf go1.22.4.linux-amd64.tar.gz
```

- **添加环境变量**
```
export PATH=$PATH:/usr/local/go/bin
```

- **查看版本验证是否成功**
```
go version
```

### 安装hugo
- **下载安装包**
hugo github：https://github.com/gohugoio/hugo/releases
下载对应版本hugo，我选择的是 hugo_0.119.0_linux-amd64.deb
*ps:这里要注意，如果hugo版本太高，后面使用主题的时候可能会出错，因为hugo在0.12版本后改变类很多东西，与主题不适配*

- **安装**
ubuntu下直接安装既可
```
sudo dpkg -i hugo_0.119.0_linux-amd64.deb
```

- **查看安装是否成功**
```
hugo version
```

### 新建本地网站
```
hugo new site xxx
```
后面的xxx为文件夹名称，例如```hugo new site blog```
这样，在你当前路径下就会出现一个名为 `blog` 的文件夹，与网站相关的内容都在该文件夹下。

内容如下
```
├── archetypes
│   └── default.md
├── config.toml         # 博客站点的配置文件
├── content             # 博客文章所在目录
├── data                
├── i18n
├── layouts             # 网站布局
├── static              # 一些静态内容
└── themes              # 博客主题
```

### 安装主题
进入hugo主题仓库：https://themes.gohugo.io/
挑选你喜欢的主题，这里我用的主题是NewBee：https://themes.gohugo.io/themes/newbee/
注意该主题不能使用hugo 0.12以上的版本

将主题从作者的github上git下来，放到`blog`文件夹下的`theme`文件夹下
```
git clone https://github.com/xioyito/NewBee themes/NewBee
```

然后将`NewBee/config-example`文件夹下的config.toml复制到`blog`文件夹下（不同的主题config.toml的位置可能不同，可以参考作者的README）
```
cp NewBee/config-example/config.toml blog/
```

### 新建文章
```
hugo new content posts/firstpost.md
```
Hugo 会在 content 目录下创建一个新目录 posts ，该目录将会用来存储所有博客文章。 同时 Hugo 还会在 posts 目录下创建一个新的 Markdown 文件 firstpost.md ，用来存放博客文章的具体内容。

可以选择输入`vim content/posts/firstpost.md`来编辑文章，也可以利用vscode等工具来直接编写文章

### 本地测试
做完上述就可以进行本地预览了，在`blog`下输入`hugo server -D`，根据屏幕输出提示，在浏览器中输入网址 http://localhost:1313 查看网站效果。
- 参数 -D 可构建标记为“草稿”的文章和页面。`hugo server -D`和`hugo server`的差别：后者不会渲染前置参数为draft: true（draft是草稿的意思）的文章，这意味着在预览网站时你不会看到这篇文章的任何信息。

此时当更新博客内容文件（如 firstpost.md ）、网站配置文件（如 hugo.toml）或主题配置（如 theme/NewBee/），Hugo 服务器会实时自动刷新网站内容。 `Ctrl+C` 停止本地服务器

### 构建网站
当发布完文章并且测试后，即可生成完整网站
输入`hugo`，Hugo 会创建一个新目录 `public` ，并将部署网站所需的一切静态资源和内容整合至该目录下。

## 服务器配置
### 安装nginx
Nginx (engine x) 可用作HTTP和反向代理服务器、邮件代理服务器或通用的TCP/UDP代理服务器。
输入`sudo apt install nginx`，即可快速安装nginx。

### nginx的简单操作
- 查看nginx的位置：`whereis nginx`
- 开启nginx： `sudo systemctl start nginx`
- 关闭nginx： `sudo systemctl stop nginx`
- 查看当前nginx状态：`systemctl status nginx`
- 验证当前配置文件是否正确： `nginx -t -c xxx`
  - -c后可跟具体的配置文件，如果不加则为当前已加载配置文件

### nginx配置
1. 新建配置：
```
cd /etc/nginx/sites-available/
sudo cp default mysite
sudo vim mysite
```
2. 修改mysite `sudo vim mysite`
```
server {
	listen 80;
	
	server_name xxx xxx; # server_name里面填写你的域名
	
	location / {
        root xxxx;   # root 后面填写都会传到服务器上面public文件夹的位置
        index index.html;
        error_page 404 /404.html;
	}
}
```

3. 替换原有 Nginx 配置文件
```
sudo rm /etc/nginx/sites-enabled/default
sudo cp /etc/nginx/sites-available/mysite /etc/nginx/sites-enabled/mysite
```

4. 启动nginx
```
sudo systemctl start nginx
sudo systemctl enable nginx
```

### 部署网站
将hugo在本地电脑上生成的public上传到服务器上
```
scp -r public username@hostname:~/mysite
```

### 查看网站
此时可在浏览器输入域名，查看博客网站效果。


## 更新博客
后续在本地更新网站内容后，只要将本地的pulic替换服务器上的public即可
这里我们可以写一个关于`rsync`的shell脚本来进行快捷操作
兴建一个脚本`vim deploy.sh`
```shell
#!/bin/sh
USER=xxx
HOST=xxx
DIR=xxx  #例如/home/ubuntu/mysite/
KEY=xxx  #可选
#因为我用的腾讯云服务器，生成类一个密匙放在本地电脑上，只有通过这个密匙才能连接服务器，如果没有密匙的话，需要输入密码

hugo && rsync -e "ssh -i ${KEY}" -avzp --delete public/ ${USER}@${HOST}:${DIR}
#--delete 参数删除只存在于目标目录、不存在于源目标的文件，即保证目标目录是源目标的镜像
#更多参数可参考https://www.ruanyifeng.com/blog/2020/08/rsync.html
```

将脚本嗯放在`blog`文件夹下，每次只需要在终端里输入`bash deploy.sh`,即可完成同步

