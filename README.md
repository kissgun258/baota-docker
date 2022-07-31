# docker套娃baota
优雅地使用 Docker 套娃纯净宝塔面板。

## 一、前言



- 首先感谢前辈的分享，[https://github.com/nzzaidi/baota-docker](https://github.com/nzzaidi/baota-docker)，受此启发，于是在此分享下本人制作了纯净版镜像。能力有限，对github、Linux、docker等并不是特比的熟悉，基本上就是靠着文档+谷歌+实践慢慢学了些皮毛，所以有不周全之处还请见谅，也希望各位前辈们多多指教。

- 初衷是每次部署宝塔面板的安装过程都非常繁琐和漫长，考虑到docker的便捷性，每次部署只需要一键拉取和部署即可，方便快捷，简单省事，无需等待每次部署环境的漫长过程。

- **另一方面，前段时间，baota后台上传数据的事情闹得沸沸扬扬，包括网友发现，连海外版本的aapanel也会在凌晨的时候上传数据，引起了很多小伙伴的担心。**

- 制作了本纯净版镜像

## 二、镜像特点

- 纯净7.6.0版本
- 全程自动安装依赖
- 自动安装宝塔面板、环境、插件
- 自动修改默认面板端口、用户名、密码、安全入口
- 自动配置镜像ssh
- 自动同意首次登陆的用户协议
- 纯净版基于官方代码、仅做通信剥离、代码未加密、未添加任何新增代码
- 剥离了所有与baota官方的通信、上报、下发；并且不与纯净版服务器通信；
- 提升为企业会员，免费使用软件商店中的所有[企业版插件]、[专业版插件]、[运行环境]、[免费插件]、[宝塔插件]；部分[第三方应用]安装可能会失败




## 三、如何部署

- 面板默认登录地址：```http://{{面板ip地址}}:10808/kissgun```
- 面板默认用户名：```kissgun```
- 面板默认密码：```kissgun2001```
- 面板默认端口：```10808```
- 面板默认安全入口：```/kissgun```
- 镜像内部ssh端口：```1072```
- 镜像内部ssh root用户密码：```kissgun2001```

**注意**：部署后务必先修改如上信息！！！或者修改代码后自行构建使用！！！以防止被利用！！！

镜像仓库地址：[https://hub.docker.com/r/kissgun/baota](https://hub.docker.com/r/kissgun/baota)

代码仓库地址：[https://github.com/kissgun258/baota-docker](https://github.com/kissgun258/baota-docker)


### 1.通过 docker run 运行

```bash
docker run -itd \
  --name baota \
  --network=host \
  --privileged=true \
  --restart=unless-stopped \
  -v ~/www/wwwroot:/www/wwwroot \
  -v ~/www/vhost:/www/server/panel/vhost \
  kissgun/baota:lnmp
```




### 常用命令：

```bash
# 获取宝塔面板默认信息
docker exec -it baota /etc/init.d/bt default

# 重启nginx
docker exec -it baota /etc/init.d/nginx restart

# 重启PHP
docker exec -it baota /etc/init.d/php-fpm-80 restart

# 重启mysql
docker exec -it baota /etc/init.d/mysqld restart

# 进入宝塔容器
docker exec -it baota /bin/sh
```



## 四、版本区别

| 已安装软件                         | lib | latest | lnmp        |
| --------------------------------  | ----- | ------ | ------ | 
| nginx                              | -     | -      | √-1.21  |
| php                                | -     | -      | √-8.0  | 
| mysql                              | -     | -      | √-mariadb_10.3 |
| phpmyadmin                         | -     | -      | √-5.1   |
| redis                              | -     | -      | √         |
| memcached                          | -     | -      | √         |
| fileinfo                           | -     | -      | √            |
| pm2                                | -     | -      | √            |
| phpguard（PHP守护）                | -     | -        |  √         |
| webssh（宝塔SSH终端）              | √     | √       | √             |
| boot（系统启动项）                 | √     | √         | √           |
| linuxsys（Linux工具箱）            | √     | √          | √          |
| clear（日志清理工具）              | √     | √         | √            |


（√ 表示已安装，- 表示未安装）



