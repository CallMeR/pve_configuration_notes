#!/bin/sh
set -e

WORKDIR="$(mktemp -d)"
CONFDIR="/etc/smartdns.d"
SERVERS="223.5.5.5 180.184.1.1 119.29.29.29 114.114.114.114 2402:4e00:: 2400:3200::1"
GROUP="flash"
# Others: 223.6.6.6 119.28.28.28
# Not using best possible CDN pop: 1.2.4.8 210.2.4.8
# Broken?: 180.76.76.76

CONF_WITH_SERVERS="accelerated-domains.china google.china apple.china"
CONF_WITH_GROUP="dns-group.china"
CONF_SIMPLE="bogus-nxdomain.china"

echo "Checking whether the configuration folder exists..."
[ -d "$CONFDIR" ] || mkdir -p "$CONFDIR"

echo "Downloading latest configurations..."
git clone --depth=1 https://gitee.com/felixonmars/dnsmasq-china-list.git "$WORKDIR" || { echo "Git clone failed"; rm -rf "$WORKDIR"; exit 1; }
#git clone --depth=1 https://pagure.io/dnsmasq-china-list.git "$WORKDIR"
#git clone --depth=1 https://github.com/felixonmars/dnsmasq-china-list.git "$WORKDIR"
#git clone --depth=1 https://bitbucket.org/felixonmars/dnsmasq-china-list.git "$WORKDIR"
#git clone --depth=1 https://gitlab.com/felixonmars/dnsmasq-china-list.git "$WORKDIR"
#git clone --depth=1 https://e.coding.net/felixonmars/dnsmasq-china-list.git "$WORKDIR"
#git clone --depth=1 https://atomgit.com/felixonmars/dnsmasq-china-list.git "$WORKDIR"
#git clone --depth=1 https://codehub.devcloud.huaweicloud.com/dnsmasq-china-list00001/dnsmasq-china-list.git "$WORKDIR"
#git clone --depth=1 http://repo.or.cz/dnsmasq-china-list.git "$WORKDIR"

echo "Removing old configurations..."
for _conf in $CONF_WITH_SERVERS $CONF_WITH_GROUP $CONF_SIMPLE; do
    rm -f "$CONFDIR/$_conf"*.conf
done

echo "Installing new configurations..."
for _conf in $CONF_WITH_SERVERS $CONF_WITH_GROUP $CONF_SIMPLE; do

    if echo " $CONF_WITH_SERVERS " | grep -q " $_conf "; then
        sed -n 's/^server=\/\([^\/]*\)\/114.114.114.114$/\1/p' "$WORKDIR/$_conf.conf" | grep -v '^#' > "$WORKDIR/$_conf.step1.raw"
        sed -e "s/.*/nameserver \/&\/$GROUP/" "$WORKDIR/$_conf.step1.raw" > "$WORKDIR/$_conf.step2.raw"
        cp "$WORKDIR/$_conf.step2.raw" "$CONFDIR/$_conf.smartdns.conf"
    fi

    if echo " $CONF_WITH_GROUP " | grep -q " $_conf "; then
        for _server in $SERVERS; do
            echo "server $_server -group $GROUP -exclude-default-group" >> "$WORKDIR/$_conf.raw"
        done
        cp "$WORKDIR/$_conf.raw" "$CONFDIR/$_conf.smartdns.conf"
    fi

    if echo " $CONF_SIMPLE " | grep -q " $_conf "; then
        sed -e "s/=/ /" "$WORKDIR/$_conf.conf" > "$WORKDIR/$_conf.raw"
        cp "$WORKDIR/$_conf.raw" "$CONFDIR/$_conf.smartdns.conf"
    fi

done

echo "Restarting smartdns service..."
if command -v systemctl >/dev/null 2>&1; then
    systemctl restart smartdns
elif command -v service >/dev/null 2>&1; then
    service smartdns restart
elif command -v rc-service >/dev/null 2>&1; then
    rc-service smartdns restart
elif [ -x "/etc/init.d/smartdns" ]; then
    /etc/init.d/smartdns restart
else
    echo "Now please restart smartdns manually."
fi

echo "Cleaning up..."
rm -rf "$WORKDIR"

