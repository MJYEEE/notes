# 利用Hugo搭建静态个人博客
本文主要介绍本Blog的搭建方法，利用了hugo和Nginx在腾讯云上搭建。

---

## 1. 前提环境条件
- git
- Go
- hugo
- Nginx
- 一台腾讯云轻量应用服务器（可选备案域名）
- 一台能用的本地电脑
## 2. 本地搭建过程
本文的本地和服务器实验系统均为Ubuntu20.04。
### 2.1 安装 git
git官网 https://git-scm.com/download/linux
ubuntu下快速安装方式`sudo apt-get install git`

### 2.2 安装 go

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

### 2.3 安装hugo
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

### 2.4 新建本地网站
```
hugo new site xxx
```
后面的xxx为文件夹名称，例如```hugo new site blog```
这样，在你当前路径下就会出现一个名为 `blog` 的文件夹，与网站相关的内容都在该文件夹下。

### 2.5 安装主题
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

做完上述就可以进行本地预览了，在`blog`下输入`hugo server -D`，根据屏幕输出提示，在浏览器中输入网址 http://localhost:1313 查看网站效果。
- 参数 -D 可构建标记为“草稿”的文章和页面。

