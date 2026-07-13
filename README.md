方法1
安装

#SSH

opkg 更新

opkg install coreutils-install

#SSH

wget -O install.sh https://cdn.jsdelivr.net/gh/as886944123/vohive-release@master/install.sh
sh install.sh

wget -O - https://cdn.jsdelivr.net/gh/as886944123/vohive-release@master/install.sh | sh

curl -fsSL https://cdn.jsdelivr.net/gh/as886944123/vohive-release@master/install.sh | sh

#SSH

sed 's/os=/$(uname -s)/' /fi/d install.sh | sh



方法2
#离线一键部署脚本

TAG="v1.5.12"
架构="arm64"
mkdir -p /opt/vohive/bin /opt/vohive/config /opt/vohive/data /opt/vohive/logs
wget -q https://github.com/as886944123/vohive-release/releases/download/${TAG}/vohive_${TAG}_linux_${arch} -O /opt/vohive/bin/vohive
chmod +x /opt/vohive/bin/vohive


cat > /opt/vohive/config/config.yaml <<'EOF'
server:
  port: ":7575"
web:
  username: "admin"
  password: "admin"
EOF


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

方法3

#!/bin/sh
TAG="v1.5.14"
arch="arm64"
# 创建目录
mkdir -p /opt/vohive/bin /opt/vohive/config /opt/vohive/data /opt/vohive/logs
# 下载arm64二进制
wget --no-check-certificate -qL https://github.com/as886944123/vohive-release/releases/download/${TAG}/vohive_${TAG}_linux_${arch} -O /opt/vohive/bin/vohive
chmod +x /opt/vohive/bin/vohive
# 写入配置yaml
cat > /opt/vohive/config/config.yaml <<'EOF'
server:
  port: ":7575"
web:
  username: "admin"
  password: "admin"
EOF
# procd启动脚本（shell包裹cd，解决数据库路径崩溃）
cat > /etc/init.d/vohive <<'EOF'
#!/bin/sh /etc/rc.common
START=99
USE_PROCD=1
start_service() {
    procd_open_instance
    procd_set_param command sh -c "cd /opt/vohive && /opt/vohive/bin/vohive -c /opt/vohive/config/config.yaml"
    procd_set_param respawn 3600 5 5
    procd_set_param stdout 1
    procd_set_param stderr 1
    procd_close_instance
}
EOF
chmod +x /etc/init.d/vohive
# 清理旧残留+权限
rm -rf /data
rm -f /opt/vohive/data/vohive.db
chmod 777 -R /opt/vohive
# 开机自启+启动服务
/etc/init.d/vohive enable
/etc/init.d/vohive start
# 防火墙放行7575端口
uci add firewall rule
uci set firewall.@rule[-1].name='Vohive_7575'
uci set firewall.@rule[-1].src='*'
uci set firewall.@rule[-1].dest_port='7575'
uci set firewall.@rule[-1].target='ACCEPT'
uci commit firewall
/etc/init.d/firewall reload
echo "====================安装完成===================="
echo "访问地址：http://路由器IP:7575"
echo "账号：admin  密码：admin"

#完全删除

service vohive stop 2>/dev/null; service vohive disable 2>/dev/null; rm -f /etc/init.d/vohive; rm -rf /opt/vohive /data; echo "VoHive 全部卸载干净"
#完全删除

/etc/init.d/vohive stop || true

/etc/init.d/vohive disable || true

rm -f /etc/init.d/vohive /usr/bin/vohive

rm -rf /opt/vohive
