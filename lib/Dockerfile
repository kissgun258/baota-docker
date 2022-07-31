FROM centos:7
MAINTAINER kissgun<453956595@qq.com>

# 将当前目录文件复制到根目录
COPY . /code/

#设置init.d和letsencrypt映射到www文件夹下持久化
RUN mkdir -p /www/letsencrypt /www/wwwroot \
    && ln -s /www/letsencrypt /etc/letsencrypt \
    && rm -f /etc/init.d \
    && mkdir /www/init.d \
    && ln -s /www/init.d /etc/init.d \
    && chmod +x -R /code

#更新系统 安装依赖 安装宝塔面板 清理缓存 输出面板默认信息
RUN cd /code \
    && yum -y update \
    && yum -y install wget curl vim openssh-server \
    && bash set_sshd_config.sh \
    && wget -O install.sh http://v7.hostcli.com/install/install_6.0.sh \
    && sed -i 's/8888/10808/g' install.sh \
    && echo y | bash install.sh \
    && python install_plugin.py "webssh,boot,linuxsys,clear" \
    && echo '["boot", "linuxsys", "clear", "webssh"]' > /www/server/panel/config/index.json \
    && python set_default.py \
    && yum clean all \
    && rm -f /www/server/panel/data/bind.pl \
    && bt reload \
    && bt restart \
    && bt default

WORKDIR /www/wwwroot
CMD /code/entrypoint.sh
EXPOSE 10808 888 21 20 443 80

VOLUME ["/www/wwwroot","/www/wwwroot"]

HEALTHCHECK --interval=5s --timeout=3s CMD curl -fs http://localhost:10808/kissgun || exit 1