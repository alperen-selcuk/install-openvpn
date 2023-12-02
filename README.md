## OPENVPN

wget https://git.io/vpn -O openvpn-install.sh

chmod +x openvpn-install.sh

./openvpn-install.sh


# CONF

sadece belli routelara VPN den erişmek gerisine kendi gatewayinizden gitmek için.

pull-filter ignore redirect-gateway
route-nopull
push "route X.X.X.X"
push "dhcp-option DNS 8.8.8.8"

bütün trafiğiniz VPN den çıkması için

push "redirect-gateway def1"

# NAT 

eğer openvpn serverin netwrokü dışında bir yere erişecekseniz paketlerin NAT lanması gerekiyor
openVPN server üzerinde

```
iptables -A FORWARD -i tun0 -j ACCEPT

iptables -t nat -A POSTROUTING -s 10.0.0.0/16 \! -d 10.0.0.0/16 -j SNAT --to-source 10.0.1.223 //openvpn ethernet IP

iptables -t nat -A POSTROUTING -s 192.168.254.0/24 -o ens5 -j MASQUERADE
```

bu komutları girmeniz gerek.
