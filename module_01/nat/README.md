# Настройка NAT

## ISP
ВНИМАНИЕ!
ens34 - это интерфейс по которому ISP получает интернет, в вашем случае он может называться по-другому
```
iptables -t nat -A POSTROUTING -o ens34 -s 172.16.4.0/28 -j MASQUERADE
iptables -t nat -A POSTROUTING -o ens34 -s 172.16.5.0/28 -j MASQUERADE
iptables-save > /etc/sysconfig/iptables
systemctl enable --now iptables
```
## HQ-RTR

```
interface TO-ISP
 ip nat outside
!
interface HQ-SRV
 ip nat inside
!
interface HQ-CLI
 ip nat inside
!
interface HQ-MGMT
 ip nat inside
!
ip nat pool NAT_POOL 192.168.0.1-192.168.0.254
!
ip nat source dynamic inside-to-outside pool NAT_POOL overload interface TO-ISP
```

## BR-RTR

```
object-group network LOCAL_NET
  ip address-range 192.168.1.1-192.168.1.30
exit

nat source
  ruleset SNAT
    to interface gigabitethernet 1/0/1
    rule 1
      match source-address LOCAL_NET
      action source-nat interface
      enable
    exit
  exit
exit
```

## Проверка

<img src="01.png" width='600'>

<img src="02.png" width='600'>
