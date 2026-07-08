安装

#SSH

opkg update

opkg install coreutils-install

#SSH

wget https://cdn.jsdelivr.net/gh/as886944123/vohive-release@master/install.sh

wget -O - https://cdn.jsdelivr.net/gh/as886944123/vohive-release@master/install.sh | sh

curl -fsSL https://cdn.jsdelivr.net/gh/as886944123/vohive-release@master/install.sh | sh

#SSH

sed '/os="$(uname -s/,/fi/d' install.sh | sh


#完全删除

# 停止并禁用服务
/etc/init.d/vohive stop 2>/dev/null || true
/etc/init.d/vohive disable 2>/dev/null || true

# 删除程序、启动脚本
rm -f /etc/init.d/vohive /usr/bin/vohive

# 删除主目录、用户配置
rm -rf /opt/vohive /root/.vohive

# 删除当前目录下的安装脚本
rm -f install.sh

echo "VoHive 及安装脚本全部清理完毕"
