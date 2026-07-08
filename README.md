wget -O - https://raw.githubusercontent.com/as886944123/vohive-release/refs/heads/master/install.sh | sh

wget https://cdn.jsdelivr.net/gh/as886944123/vohive-release@master/install.sh

#opwenrt

opkg update

opkg install coreutils-install

#bash运行

sed '/os="\$(uname -s/,/fi/d' install.sh | sh
