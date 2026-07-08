方法1
安装

#SSH

opkg update

opkg install coreutils-install

#SSH

wget -O install.sh https://cdn.jsdelivr.net/gh/as886944123/vohive-release@master/install.sh
sh install.sh

wget -O - https://cdn.jsdelivr.net/gh/as886944123/vohive-release@master/install.sh | sh

curl -fsSL https://cdn.jsdelivr.net/gh/as886944123/vohive-release@master/install.sh | sh

#SSH

sed '/os="$(uname -s/,/fi/d' install.sh | sh



方法2
#离线一键部署脚本
TAG="v1.5.12"
arch="arm64"
mkdir -p /opt/vohive/bin /opt/vohive/config /opt/vohive/data /opt/vohive/logs
wget -q https://github.com/as886944123/vohive-release/releases/download/${TAG}/vohive_${TAG}_linux_${arch} -O /opt/vohive/bin/vohive
chmod +x /opt/vohive/bin/vohive

# 生成默认配置
cat > /opt/vohive/config/config.yaml <<'EOF'
server:
  port: ":7575"
web:
  username: "admin"
  password: "admin"
EOF

# OpenWrt procd 开机自启服务
cat > /etc/init.d/vohive <<'EOF'
#!/bin/sh /etc/rc.common
START=99
USE_PROCD=1
start_service() {
    procd_open_instance
    procd_set_param command /opt/vohive/bin/vohive -c /opt/vohive/config/config.yaml
    procd_set_param directory /opt/vohive
    procd_set_param respawn 3600 5 5
    procd_set_param stdout 1
    procd_set_param stderr 1
    procd_close_instance
}
EOF

chmod +x /etc/init.d/vohive
/etc/init.d/vohive enable
/etc/init.d/vohive start

#完全删除

#停止并禁用服务

/etc/init.d/vohive stop 2>/dev/null || true

/etc/init.d/vohive disable 2>/dev/null || true

#删除程序、启动脚本

rm -f /etc/init.d/vohive /usr/bin/vohive

#删除主目录、用户配置

rm -rf /opt/vohive /root/.vohive

#删除当前目录下的安装脚本
rm -f install.sh
