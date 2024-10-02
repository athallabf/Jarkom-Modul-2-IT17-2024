# DOKUMENTASI

## PERSISTENT DISK (ALL DEVICES) IMPORTANT

on your terminal run

```
cp -r /etc /root/etc-persistent
cp -r /usr /root/usr-persistent
cp -r /var /root/var-persistent

mount --bind /root/etc-persistent /etc
mount --bind /root/usr-persistent /usr
mount --bind /root/var-persistent /var
```

`vi /etc/fstab`
isi begini:

```
/root/etc-persistent /etc none bind 0 0
/root/usr-persistent /usr none bind 0 0
/root/var-persistent /var none bind 0 0
```

`cd /root`
`vi restore.sh`
isi begini:

```
#!/bin/bash
cp -r /root/etc-persistent/_ /etc/
cp -r /root/usr-persistent/_ /usr/
cp -r /root/var-persistent/\* /var/

mount --bind /root/etc-persistent /etc
mount --bind /root/usr-persistent /usr
mount --bind /root/var-persistent /var
```

misal ke reset:

```
bash /root/restore.sh
```

run ketika ke reset aja

## TOPOLOGY

![alt text](image.png)

## SOAL 1

NOTES:
IP `10.72.1.2` itu IP Sriwijaya (DNS MASTER)
IP `10.72.2.5` itu IP Majapahit (DNS SLAVE)

### Nusantara

```
auto eth0
iface eth0 inet dhcp

auto eth1
iface eth1 inet static
	address 10.72.1.1
	netmask 255.255.255.0

auto eth2
iface eth2 inet static
	address 10.72.2.1
	netmask 255.255.255.0

up iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 10.72.0.0/16
```

### Tanjungkulai

```
auto eth0
iface eth0 inet static
  address 10.72.2.2
  netmask 255.255.255.0
  gateway 10.72.2.1

up echo nameserver 10.72.1.2 > /etc/resolv.conf
up echo nameserver 10.72.2.5 >> /etc/resolv.conf
```

### Bedahulu

```
auto eth0
iface eth0 inet static
  address 10.72.2.3
  netmask 255.255.255.0
  gateway 10.72.2.1

up echo nameserver 10.72.1.2 > /etc/resolv.conf
up echo nameserver 10.72.2.5 >> /etc/resolv.conf
```

### Sriwijaya

```
auto eth0
iface eth0 inet static
  address 10.72.1.2
  netmask 255.255.255.0
  gateway 10.72.1.1

up echo nameserver 192.168.122.1 > /etc/resolv.conf
```

### Majapahit

```
auto eth0
iface eth0 inet static
  address 10.72.2.5
  netmask 255.255.255.0
  gateway 10.72.2.1

up echo nameserver 192.168.122.1 > /etc/resolv.conf
up echo nameserver 10.72.1.2 >> /etc/resolv.conf
```

## SOAL 2

Masuk ke Web Console Sriwijaya

1. `apt update && apt install bind9 dnsutils -y`
2. `cd /etc/bind`
3. `vi named.conf.local`

```
// Didalam named.conf.local


//TEMPLATE (HAPUS //)
//zone "namadomain.itxx.com" {
//  type master;
//  file "/etc/bind/itxx/namadomain.itxx.com";
//};

zone "sudarsana.it17.com" {
  type master;
  file "/etc/bind/it17/sudarsana.it17.com";
};

// SAVE FILE
```

4. `mkdir /etc/bind/it17`
5. `cd it17`
6. `cp ../db.local sudarsana.it17.com`
7. `vi sudarsana.it17.com`

Nanti tampilannya seperti ini
![sudarsana.it17.com](image-1.png)

Ubah menjadi seperti ini
![alt text](image-2.png)

8. `service bind9 restart`
   ![alt text](image-3.png)

Untuk mengecek error pada config:
`named-checkconf`

Untuk mengecek error pada config zone:

```
named-checkzone namadomain.itxx.com /etc/bind/itxx/namadomain.itxx.com
```

9. Masuk ke web console client bebas (yang gambar laptop)

10. Pastiin udah ada nameserver dari DNSnya
    cek `cat /etc/resolv.conf`![alt text](image-6.png)
11. Kalo GAADA di tambahin

    ```
    echo nameserver 10.72.1.2 >> /etc/resolv.conf
    ```

    ```
    echo nameserver 10.72.1.5 >> /etc/resolv.conf
    ```

    notes:
    `10.72.1.2` disini itu IP dari Sriwijaya (DNS MASTER)
    `10.72.2.5` disini itu IP dari Majapahit (DNS SLAVE)

12. ping sudarsana.it17.com
    ![alt text](image-5.png)
    yey berhasil

## SOAL 3

Masuk ke Web Console Sriwijaya

1. `cd /etc/bind`
2. `vi named.conf.local`

```
// Didalam named.conf.local


//TEMPLATE (HAPUS //)
//zone "namadomain.itxx.com" {
//  type master;
//  file "/etc/bind/itxx/namadomain.itxx.com";
//};

zone "sudarsana.it17.com" {
  type master;
  file "/etc/bind/it17/sudarsana.it17.com";
};

zone "pasopati.it17.com" {
  type master;
  file "/etc/bind/it17/pasopati.it17.com";
};

// SAVE FILE
```

3. `cd it17`
4. `cp sudarsana.it17.com pasopati.it17.com`
5. `vi pasopati.it17.com`

Nanti tampilannya seperti ini
![alt text](image-2.png)

Ubah menjadi seperti ini
![alt text](image-4.png) 8. `service bind9 restart`
![alt text](image-3.png)

Untuk mengecek error pada config:
`named-checkconf`

Untuk mengecek error pada config zone:

```
named-checkzone namadomain.itxx.com /etc/bind/itxx/namadomain.itxx.com
```

## SOAL 4

step sama kek soal 2 dan 3

## SOAL 5

1. Masuk ke web console client (yang gambar laptop)
2. jalanin command di bawah
   Notes: sebernya gaperlu karena di Network Configuration tadi udah di add tapi kalo gabisa pake ini:

```
echo nameserver 10.72.1.2 >> /etc/resolv.conf
```

```
echo nameserver 10.72.1.5 >> /etc/resolv.conf
```

notes:
`10.72.1.2` disini itu IP dari Sriwijaya (DNS MASTER)
`10.72.2.5` disini itu IP dari Majapahit (DNS SLAVE)

## SOAL 6

1. Masuk web console Sriwijaya
2. `cd /etc/bind`
3. `vi named.conf.local`
   tambahin zone baru:

```
zone "2.72.10.in-addr.arpa" {
  type master;
  file "/etc/bind/it17/2.72.10.in-addr.arpa";
};
```

Penjelasan dikit:
IP Kotalingga di Topology ku: 10.72.2.4
dibalik jadi
2.72.10 (4 nya ilangin disini nanti add di step selanjutnya)

4. `cd it17`
5. `cp pasopati.it17.com 2.72.10.in-addr.arpa`
6. samain kek gini
   ![alt text](image-8.png)
   Penjelasan lanjutan:
   Bisa dilihat klo `4` nya ditulis disitu
7. `service bind9 restart`
8. Masuk web console client (yang gambar laptop)
9. `apt install dnsutils -y`
10. Pastiin udah ada nameserver si DNS di /etc/resolv.conf
11. `host -t PTR 10.72.2.4`
    Penjelasan `host -t PTR IPKOTALINGGA`
    ![alt text](image-9.png)

## SOAL 7

<!-- ![alt text](image-7.png) -->
