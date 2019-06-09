System operacyjny w środowisku sieciowym
=========================================

Charakterystyka systemu operacyjnego
------------------------------------

| Charakterystyka | wartość           | komentarzu |
| ------------- |:-------------:| -----:|
| nazwa      | linux | centos 7 |
| program (parametry sieci)      | niewiem |  |

polecenie ip, nmcli show devices

Konfiguracja połączenia sieciowego
----------------------------------
| Parametr | wartość           | komentarzu |
| ------------- |:-------------:| -----:|
| Adres IP      | 10.0.2.15 | przydzielony przez DHCP (Dynamic Host Configuration Protocol) |
| Maska podsieci      | 255.255.255.0 | (24) |
| Brama      | 10.0.2.1 | $ ip r |
| DNS 1 (nameserver  (Domain Name System))      | 192.168.0.1 | cat /etc/resolv.conf |
| DNS 2      |  |  brak|

Połączenie z karta sięciową:
$ nmcli
$ dhclient enp0s3
Schemat sieci
-------------

aby załączyć obrazek 

```markdown
![alt schemat](https://github.com/adam-p/markdown-here/raw/master/src/common/images/icon48.png)![alt schemat](https://github.com/adam-p/markdown-here/raw/master/src/common/images/icon48.png)

![alt schemat](images/my-network-schema.png)
```
