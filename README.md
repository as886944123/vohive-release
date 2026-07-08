安装

#SSH

opkg update

opkg install coreutils-install

#SSH

wget https://cdn.jsdelivr.net/gh/as886944123/vohive-release@master/install.sh

#SSH

sed '/os="$(uname -s/,/fi/d' install.sh | sh


#完全删除

/etc/init.d/vohive stop || true

/etc/init.d/vohive disable || true

rm -f /etc/init.d/vohive /usr/bin/vohive

rm -rf /opt/vohive
