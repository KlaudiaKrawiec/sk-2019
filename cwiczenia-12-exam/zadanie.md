

# Zadanie 2

## Projekt sieci lokalnej dla jednostki dydaktycznej uniwersytetu

![budynek](budynek.svg)

### Cel Projektu
  Zaprojektowanie i weryfikacja działania sieci w środowisku testowym. 
  Rozwiązanie zapewnia dostęp do internetu dla wszystkich urządzeńw infrastrukturze.
  
### Założenia projektu

* Sieć zlokalizowana jest w budynku 3 kondygnacyjnym
* Na kążdej z kondygnacji znajdują się laboratoria komputerowe kolejno:
  * poziom 0 
    * 009, 013, 014
  * poziom 1
    * 115, 116, 117, 122
  * poziom 2
    * 201, 202, 203 
* Każde z laboratoriów wyposażone jest w 35 stanowisk dla uczestników kursów
* Jednostka planuje otworzenie kolejnych laboratoriów 017 oraz 204
* Każda kondygnacja wyposażona jest w izolowaną sieć Wi-Fi, udostępniajacą sieć internet podłączonym gościom
  * Sieć Wi-Fi nie pozwala na bezposrednią komunikację z urządzeniami zlokalizowanymi w pozostałej części sieci,
    tj: laboratoria, serwery jednostki
  * Prognozowana maksymalna liczba jednoczesnych urządzeń podłączonych do sieci to ``800``
* Jednostka posiada przyłącze internetowe oraz dysponuje pulą adresów ``188.156.220.160/27``
* Jednostka posiada serwery udostępniajace zasoby do celów dydaktycznych i promocyjnych
  * serwery zlokalizowane są w osobnym pomieszczeniu
  * udostępniają zasoby w sieci publicznej z wykorzystaniem sieci ``188.156.220.160/27``
  * Jeden serwer pełni rolę bramy dla urządzań w sieci lokalnej ``LAN``

### Wstępne założenia

* Każde laboratorium posiada oddzielną podsieć pozwalającą efektywnie zidentyfikować urządzania
  * kondygnacja oraz sala
* Dla uniknięcia zbyt słabego zasięgu sieć WiFi zostanie wyposażona w 4 urządzenia nadawcze na każdej kondygnacji
 

#### zadanie - wymagania

* Dokonaj podziału i projektu sieci w formie dokumentu w formacie ``MARKDOW`` zawierającego specyfikację tekstową oraz obrazkową
  projektowanej sieci
* Przygotuj prototyp rozwiązania z wykorzystaniem oprogramowania ``VirtualBox`` lub podobnego.
* W specyfikacji uwzględnij wielkości sieci oraz ich adresy
* W specyfikacji uwzględnij konfigurację tablicy routingu
* Dokumentację graficzną stworzonej architektury przygotuj w programie ``DIA`` lub podobnym

----------------------------------------------------------------------------------------------------------------------------------

## Rozwiązanie

Podział sieci:
---------------
Sieć w salach: ``188.156.220.160/28`` 
  * Podsieć: ``192.160.0.0/16``
  * Router: ``192.160.0.1/16``
  
WIFI: ``188.156.220.176/28`` 
  * Podsieć: ``192.162.0.0/22``
  * Router: ``192.162.0.1/22``

Poziom 2
------
* Istniejące sale: ``201`` ``202`` ``203``

* Planowane sale: ``204``

* Ilość istniejących stanowisk razem: ``105``

* Ilość planowanych stanowisk razem: ``140``

| Numer sali | Adresacja |
|:-----------|:------------|
| sala ``201`` | ``192.160.201.0/26`` |
| router sala ``201`` | ``192.160.0.2/26`` ``10.10.201.1/26`` |
| sala ``202`` | ``192.160.202.0/26`` |
| router sala ``202`` | ``192.160.0.3/26`` ``10.10.202.1/26`` |
| sala ``203`` | ``192.160.203.0/26`` |
| router sala ``203`` | ``192.160.0.4/26`` ``10.10.203.1/26`` |
| sala ``204`` | ``192.160.204.0/26`` |
| router sala ``204`` | ``192.160.0.5/26`` ``10.10.204.1/26`` |

Poziom 1
------
* Istniejące sale: ``115`` ``116`` ``117`` ``122``

* Ilość istniejących stanowisk razem: ``140``

| Numer sali | Adresacja |
|:-----------|:------------|
| sala ``115`` | ``192.160.115.0/26`` |
| router sala ``115`` | ``192.160.0.6/26`` ``10.10.115.1/26`` |
| sala ``116`` | ``192.160.116.0/26`` |
| router sala ``116`` | ``192.160.0.6/26`` ``10.10.116.1/26`` |
| sala ``117`` | ``192.160.117.0/26`` |
| router sala ``117`` | ``192.160.0.7/26`` ``10.10.117.1/26`` |
| sala ``122`` | ``192.160.122.0/26`` |
| router sala ``122`` | ``192.160.0.8/26`` ``10.10.122.1/26`` |

Poziom 0
--------

* Istniejące sale: ``009`` ``013`` ``014``

* Planowane sale: ``017``

* Ilość istniejących stanowisk razem: ``105``

* Ilość planowanych stanowisk razem: ``140``

| Numer sali | Adresacja |
|:-----------|:------------|
| sala ``009`` | ``192.160.9.0/26`` |
| router sala ``009`` | ``192.160.0.9/26`` ``10.10.9.1/26`` |
| sala ``013`` | ``192.160.13.0/26`` |
| router sala ``013`` | ``192.160.0.10/26`` ``10.10.13.1/26`` |
| sala ``014`` | ``192.160.14.0/26`` |
| router sala ``014`` | ``192.160.0.11/26`` ``10.10.14.1/26`` |
| sala ``017`` | ``192.160.17.0/26`` |
| router sala ``017`` | ``192.160.0.12/26`` ``10.10.17.1/26`` |

#### Łącznie stanowisk planowanych: 420

Diagram DIA
----------

![Zadanie_2]

---

Konfiguracja prototypowego rozwiązania
-------------------------------
### Włączenie przekazywania adresów IP na Kernelu:
W pliku: ``/etc/sysctl.d/99-sysctl.conf``

Odkomentować: ``net.ipv4.ip_forward=1``

### Ustawianie DNS dla wszystkich urządzeń:

``/etc/resolv.conf``

nameserver ``8.8.8.8``

### Ustawianie iptables dla serwera głównego:
``iptables -t nat -A POSTROUTING -s 188.156.220.160/28 -o enp0s3 -j MASQUERADE``

``iptables -t nat -A POSTROUTING -s 188.156.220.176/28 -o enp0s3 -j MASQUERADE``

### Zapisywanie ustawień iptables dla serwera głównego:
``iptables-save > /etc/iptables.conf``

``iptables-restore < /etc/iptables.conf``

### Ustawienie statycznych adresów IP dla routera WIFI:
``` 
auto enp0s3
auto enp0s8
iface enp0s3 inet static
  address 188.156.220.178
  netmask 255.255.255.240
  gateway 188.156.220.177
iface enp0s8 inet static
  address 192.162.0.1
  netmask 255.255.252.0
  gateway 188.156.220.177 
```

### Ustawienie DHCP dla interfejsu routera WIFI:

#### Instalacja isc-dhcp-server:
``apt install isc-dhcp-server``

#### Konfiguracja isc-dhcp-server:

##### W pliku ``/etc/default/isc-dhcp-server``:

Odkomentowujemy ``DHCPDv4_CONF``

##### W pliku ``/etc/dhcp/dhcpd.conf`` zapisujemy naszą konfiguracje podsiecie DHCP:

```
subnet 192.162.0.0 netmask 255.255.252.0 {
  option routers 192.162.0.1;
  option subnet-mask 255.255.252.0;
  option domain-name-servers 192.162.0.1;
  range 192.162.0.15 192.162.3.250;
}
```
#### Routing:

``ip route add default via 192.162.0.1``

### Ustawienie statycznych adresów IP dla routera dla sal oraz w konkretnej sali:

``` 
auto enp0s3
auto enp0s8
iface enp0s3 inet static
  address 192.160.0.2
  netmask 255.255.0.0
iface enp0s8 inet static
  address 192.160.115.1
  netmask 255.255.255.192
```

### Ustawienie DHCP dla interfejsu dla routera konkretnej sali:

#### Instalacja isc-dhcp-server:
``apt install isc-dhcp-server``

#### Konfiguracja isc-dhcp-server:

##### W pliku ``/etc/default/isc-dhcp-server``:

Odkomentowujemy ``DHCPDv4_CONF``

##### W pliku ``/etc/dhcp/dhcpd.conf`` zapisujemy naszą konfiguracje podsiecie DHCP:

```
subnet 192.160.115.0 netmask 255.255.255.192 {
  option routers 192.160.115.1;
  option subnet-mask 255.255.255.192;
  option domain-name-servers 192.160.115.1;
  range 192.160.115.2 192.160.115.63;
}
```
#### Routing:

``ip route add default via 192.160.0.1``

``ip route add default via 192.160.115.1``
