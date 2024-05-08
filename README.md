# Jarkom-Modul-2-IT22-2024

|Nama  | NRP |
|--|--|
| Jacinta Syilloam | 5027221036 |


## Topologi 
![image](https://github.com/JacintaSyilloam/Jarkom-Modul-2-IT22-2024/assets/121095246/d581470a-3c07-4300-a621-324151a17102)



## IP addresses
| Device      | Interface | IP Address | Netmask       | Gateway     |
|-------------|-----------|------------|---------------|-------------|
| **Erangel** | eth0      | DHCP (192.168.122.1)       |               |             |
|             | eth1      | 192.244.1.2| 255.255.255.0 |             |
|             | eth2      | 192.244.2.2| 255.255.255.0 |             |
|             | eth3      | 192.244.3.2| 255.255.255.0 |             |
| **Pochinki**| eth0      | 192.244.3.1| 255.255.255.0 | 192.244.3.2 |
| **Georgopol**| eth0     | 192.244.2.1| 255.255.255.0 | 192.244.2.2 |
| **Severny** | eth0      | 192.244.1.3| 255.255.255.0 | 192.244.1.2 |
| **Stalber** | eth0      | 192.244.1.4| 255.255.255.0 | 192.244.1.2 |
| **Lipovka** | eth0      | 192.244.1.5| 255.255.255.0 | 192.244.1.2 |
| **MyIta**   | eth0      | 192.244.2.3| 255.255.255.0 | 192.244.2.2 |
| **Ruins**   | eth0      | 192.244.2.4| 255.255.255.0 | 192.244.2.2 |
| **Apartments**| eth0    | 192.244.2.5| 255.255.255.0 | 192.244.2.2 |



# No. 1
> Untuk membantu pertempuran di Erangel, kamu ditugaskan untuk membuat jaringan komputer yang akan digunakan sebagai alat komunikasi. Sesuaikan rancangan Topologi dengan rancangan dan pembagian yang berada di link yang telah > disediakan, dengan ketentuan nodenya sebagai berikut:
> - DNS Master akan diberi nama Pochinki, sesuai dengan kota tempat dibuatnya server tersebut
> - Karena ada kemungkinan musuh akan mencoba menyerang Server Utama, maka buatlah DNS Slave Georgopol yang mengarah ke Pochinki
> - Markas pusat juga meminta dibuatkan tiga Web Server yaitu Severny, Stalber, dan Lipovka. Sedangkan Mylta akan bertindak sebagai Load Balancer untuk server-server tersebut


On each node, configure network.


### Erangel (Router)
```
auto eth0
iface eth0 inet dhcp

auto eth1
iface eth1 inet static
	address 192.244.1.2
	netmask 255.255.255.0

auto eth2
iface eth2 inet static
	address 192.244.2.2
	netmask 255.255.255.0
	
auto eth3
iface eth3 inet static
	address 192.244.3.2
	netmask 255.255.255.0
```

### Pochinki (DNS Master)
```
auto eth0
iface eth0 inet static
	address 192.244.3.1
	netmask 255.255.255.0
	gateway 192.244.3.2
```

### Georgopol (DNS Slave)
```
auto eth0
iface eth0 inet static
	address 192.244.2.1
	netmask 255.255.255.0
	gateway 192.244.2.2
```

### Severny (Web Server)
```
auto eth0
iface eth0 inet static
	address 192.244.1.3
	netmask 255.255.255.0
	gateway 192.244.1.2
```

### Stalber (Web Server)
```
auto eth0
iface eth0 inet static
	address 192.244.1.4
	netmask 255.255.255.0
	gateway 192.244.1.2
```

### Lipovka (Web Server)
```
auto eth0
iface eth0 inet static
	address 192.244.1.5
	netmask 255.255.255.0
	gateway 192.244.1.2
```

### MyIta (Load Balancer)
```
auto eth0
iface eth0 inet static
	address 192.244.2.3
	netmask 255.255.255.0
	gateway 192.244.2.2
```

### Ruins (Client)
```
auto eth0
iface eth0 inet static
	address 192.244.2.4
	netmask 255.255.255.0
	gateway 192.244.2.2
```

### Apartments (Client)
```
auto eth0
iface eth0 inet static
	address 192.244.2.5
	netmask 255.255.255.0
	gateway 192.244.2.2
```

## Check IP address
```
ip a
```
![image](https://github.com/JacintaSyilloam/Jarkom-Modul-2-IT22-2024/assets/121095246/cc1e021b-1322-4420-b6ac-eab06ae169d3)


# No. 2
>Karena para pasukan membutuhkan koordinasi untuk mengambil airdrop, maka buatlah sebuah domain yang mengarah ke Stalber dengan alamat airdrop.xxxx.com dengan alias www.airdrop.xxxx.com dimana xxxx merupakan kode kelompok. Contoh : airdrop.it01.com

## Setup DNS @ Pochinki

Update package lists
```
apt-get update -y
```
Install bind9
```
 apt-get install bind9 -y
```
Configure domain in ```/etc/bind/named.conf.local``` file
```
nano /etc/bind/named.conf.local
```
Write the domain configuration in ```/etc/bind/named.conf.local```
```
zone "airdrop.it22.com" {
	type master;
	file "/etc/bind/it22/airdrop.it22.com";
};
```
Create /etc/bind/it22 directory
```
mkdir /etc/bind/it22
```
Copy db.local file to it22 folder that was made
```
cp /etc/bind/db.local /etc/bind/it22/airdrop.it22.com
```
Edit the DNS record
```
nano /etc/bind/it22/airdrop.it22.com
```
Edit file /etc/bind/it22/airdrop.it22.com as follows:
```
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     airdrop.it22.com. root.airdrop.it22.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      airdrop.it22.com.
@       IN      A       192.244.1.4
@       IN      AAAA    ::1
www     IN      CNAME   airdrop.it22.com.
```
- Specify hostname in NS records: airdrop.it22.com.
- Specify address in A records: 192.244.1.4
- Specify canonical name (alias) in CNAME records: airdrop.it22.com.


Restart bind9
```
service bind9 restart
```


# No. 3
> Para pasukan juga perlu mengetahui mana titik yang sedang di bombardir artileri, sehingga dibutuhkan domain lain yaitu redzone.xxxx.com dengan alias www.redzone.xxxx.com yang mengarah ke Severny


## Setup DNS @ Pochinki

Update package lists
```
apt-get update -y
```
Install bind9
```
 apt-get install bind9 -y
```
Configure domain in ```/etc/bind/named.conf.local``` file
```
nano /etc/bind/named.conf.local
```
Write the domain configuration in ```/etc/bind/named.conf.local```
```
zone "redzone.it22.com" {
	type master;
	file "/etc/bind/it22/redzone.it22.com";
};
```
Create /etc/bind/it22 directory
```
mkdir /etc/bind/it22
```
Copy db.local file to it22 folder that was made
```
cp /etc/bind/db.local /etc/bind/it22/redzone.it22.com
```
Edit the DNS record
```
nano /etc/bind/it22/redzone.it22.com
```
Edit file /etc/bind/it22/redzone.it22.com as follows:
```
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     redzone.it22.com. root.redzone.it22.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      redzone.it22.com.
@       IN      A       192.244.1.3
@       IN      AAAA    ::1
www     IN      CNAME   redzone.it22.com.
```
- Specify hostname in NS records: redzone.it22.com.
- Specify address in A records: 192.244.1.3
- Specify canonical name (alias) in CNAME records: redzone.it22.com.


Restart bind9
```
service bind9 restart
```

# No. 4
> Markas pusat meminta dibuatnya domain khusus untuk menaruh informasi persenjataan dan suplai yang tersebar. Informasi persenjataan dan suplai tersebut mengarah ke Mylta dan domain yang ingin digunakan adalah loot.xxxx.com dengan alias www.loot.xxxx.com


## Setup DNS @ Pochinki

Update package lists
```
apt-get update -y
```
Install bind9
```
 apt-get install bind9 -y
```
Configure domain in ```/etc/bind/named.conf.local``` file
```
nano /etc/bind/named.conf.local
```
Write the domain configuration in ```/etc/bind/named.conf.local```
```
zone "loot.it22.com" {
	type master;
	file "/etc/bind/it22/loot.it22.com";
};
```
Create /etc/bind/it22 directory
```
mkdir /etc/bind/it22
```
Copy db.local file to it22 folder that was made
```
cp /etc/bind/db.local /etc/bind/it22/loot.it22.com
```
Edit the DNS record
```
nano /etc/bind/it22/loot.it22.com
```
Edit file /etc/bind/it22/loot.it22.com as follows:
```
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     loot.it22.com. root.loot.it22.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      loot.it22.com.
@       IN      A       192.244.2.3
@       IN      AAAA    ::1
www     IN      CNAME   loot.it22.com.
```
- Specify hostname in NS records: loot.it22.com.
- Specify address in A records: 192.244.2.3
- Specify canonical name (alias) in CNAME records: loot.it22.com.


Restart bind9
```
service bind9 restart
```


# No. 5
> Pastikan domain-domain tersebut dapat diakses oleh seluruh komputer (client) yang berada di Erangel

The domain that we create will not be immediately recognized by the client, therefore we have to change the nameserver settings that exists on our client.

Add IP machine where DNS was set up to resolv.conf of each client.

Run script on each client (Ruins and Apartments):
```
# Define the desired nameservers in the specified order
desired_nameservers=("192.244.1.4" "192.244.3.1")

# Check if resolv.conf exists
if [ -f /etc/resolv.conf ]; then
    # Check if resolv.conf already matches the desired configuration
    if ! cmp -s <(printf "%s\n" "${desired_nameservers[@]}") /etc/resolv.conf; then
        # Backup the existing resolv.conf file
        cp /etc/resolv.conf /etc/resolv.conf.backup

        # Update resolv.conf with the desired nameservers
        echo -e "$(printf "nameserver %s\n" "${desired_nameservers[@]}")" > /etc/resolv.conf
        echo "Resolv.conf has been updated with the desired nameservers."
    else
        echo "Resolv.conf already matches the desired configuration."
    fi
else
    echo "Error: /etc/resolv.conf does not exist."
fi

```


## Testing @ Ruins
Ping airdrop.it22.com 
![image](https://github.com/JacintaSyilloam/Jarkom-Modul-2-IT22-2024/assets/121095246/ad187148-1731-403b-aaf5-0db2c6a07996)


Ping redzone.it22.com 
![image](https://github.com/JacintaSyilloam/Jarkom-Modul-2-IT22-2024/assets/121095246/0ea9645b-d15c-4987-a459-c51afcd7d5bb)


Ping loot.it22.com 
![image](https://github.com/JacintaSyilloam/Jarkom-Modul-2-IT22-2024/assets/121095246/bdeb4d23-9439-44c0-8d1a-f7530424f2c0)



## Testing @ Apartments
Ping airdrop.it22.com 
![image](https://github.com/JacintaSyilloam/Jarkom-Modul-2-IT22-2024/assets/121095246/02347e54-a41c-4637-a681-111b7c7a68af)


Ping redzone.it22.com 
![image](https://github.com/JacintaSyilloam/Jarkom-Modul-2-IT22-2024/assets/121095246/356ff73c-a21e-4e7e-ae74-39604e1fe140)


Ping loot.it22.com 
![image](https://github.com/JacintaSyilloam/Jarkom-Modul-2-IT22-2024/assets/121095246/5fbe21d5-9d57-479c-8f65-78e0f5d7ed4d)


# No. 6
> Beberapa daerah memiliki keterbatasan yang menyebabkan hanya dapat mengakses domain secara langsung melalui alamat IP domain tersebut. Karena daerah tersebut tidak diketahui secara spesifik, pastikan semua komputer (client) dapat mengakses domain redzone.xxxx.com melalui alamat IP Severny (Notes : menggunakan pointer record)

ip severny (192.244.1.3) -> domain redzone (redzone.it22.com)

## Setup DNS @ Pochinki
Add the reversed first 3 bytes of Severny IP address to etc/bind/named.conf.local.

Add to file ```/etc/bind/named.conf.local```:
```
zone "1.244.192.in-addr.arpa" {
    type master;
    file "/etc/bind/it22/1.244.192.in-addr.arpa";
};
```

Make the DNS record in ```/etc/bind/it22/1.244.192.in-addr.arpa```:
```
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     redzone.it22.com. root.redzone.it22.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
1.244.192.in-addr.arpa       IN      NS      redzone.it22.com.
3                            IN      PTR     redzone.it22.com.
```

# No. 7
> Akhir-akhir ini seringkali terjadi serangan siber ke DNS Server Utama, sebagai tindakan antisipasi kamu diperintahkan untuk membuat DNS Slave di Georgopol untuk semua domain yang sudah dibuat sebelumnya

## Config @ Pochinki (DNS Master)
Edit the ```/etc/bind/named.conf.local``` file with configurations like below:
```
zone "airdrop.it22.com" {
	type master;
	notify yes;
	also-notify { 192.244.2.1; };
    	allow-transfer { 192.244.2.1; };
	file "/etc/bind/it22/airdrop.it22.com";
};

zone "redzone.it22.com" {
	type master;
	notify yes;
	also-notify { 192.244.2.1; };
    	allow-transfer { 192.244.2.1; };
	file "/etc/bind/it22/redzone.it22.com";
};

zone "loot.it22.com" {
	type master;
	notify yes;
	also-notify { 192.244.2.1; };
    	allow-transfer { 192.244.2.1; };
	file "/etc/bind/it22/loot.it22.com";
};
```

## Config @ Georgopol (DNS Slave)
Update and install bind9
```
apt-get update -y
apt-get install bind9 -y
```

Edit the ```/etc/bind/named.conf.local``` file with configurations like below:
```
zone "airdrop.it22.com" {
	type slave;
	masters { 192.244.3.1; };
	file "/etc/bind/it22/airdrop.it22.com";
};

zone "redzone.it22.com" {
	type slave;
	masters { 192.244.3.1; };
	file "/etc/bind/it22/redzone.it22.com";
};

zone "loot.it22.com" {
	type slave;
	masters { 192.244.3.1; };
	file "/etc/bind/it22/loot.it22.com";
};
```

Make directory ```/etc/bind/it22```:
```
mkdir /etc/bind/it22
```

Create DNS records for each domain:
- in ```/etc/bind/it22/airdrop.it22.com```
```
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     airdrop.it22.com. root.airdrop.it22.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      airdrop.it22.com.
@       IN      A       192.244.1.4
@       IN      AAAA    ::1
www     IN      CNAME   airdrop.it22.com.
```

- in ```/etc/bind/it22/redzone.it22.com```
```
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     redzone.it22.com. root.redzone.it22.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      redzone.it22.com.
@       IN      A       192.244.1.3
@       IN      AAAA    ::1
www     IN      CNAME   redzone.it22.com.
```

- in ```/etc/bind/it22/loot.it22.com```
```
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     loot.it22.com. root.loot.it22.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      loot.it22.com.
@       IN      A       192.244.2.3
@       IN      AAAA    ::1
www     IN      CNAME   loot.it22.com.
```

# 8
> Kamu juga diperintahkan untuk membuat subdomain khusus melacak airdrop berisi peralatan medis dengan subdomain medkit.airdrop.xxxx.com yang mengarah ke Lipovka

## Setup DNS @ Pochinki
Edit the DNS record in ```/etc/bind/it22/airdrop.it22.com```:
```
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     airdrop.it22.com. root.airdrop.it22.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      airdrop.it22.com.
@       IN      A       192.244.1.4
@       IN      AAAA    ::1
www     IN      CNAME   airdrop.it22.com.
medkit  IN      A       192.244.1.5
```


# 9
> Terkadang red zone yang pada umumnya di bombardir artileri akan dijatuhi bom oleh pesawat tempur. Untuk melindungi warga, kita diperlukan untuk membuat sistem peringatan air raid dan memasukkannya ke subdomain siren.redzone.xxxx.com dalam folder siren dan pastikan dapat diakses secara mudah dengan menambahkan alias www.siren.redzone.xxxx.com dan mendelegasikan subdomain tersebut ke Georgopol dengan alamat IP menuju radar di Severny

## Setup DNS @ Pochinki
Edit file ```/etc/bind/it22/redzone.it22.com```:
```
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     redzone.it22.com. root.redzone.it22.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      redzone.it22.com.
@       IN      A       192.244.1.3     ; IP Severny
www     IN      CNAME   redzone.it22.com.
ns1     IN      A       192.244.2.1     ; IP Georgopol
siren   IN      NS      ns1
@       IN      AAAA    ::1
```

Edit file ```/etc/bind/named.conf.options``` to allow queries from any source
```
options {
        directory "/var/cache/bind";

        allow-query{any;};

        auth-nxdomain no;    # conform to RFC1035
        listen-on-v6 { any; };
};
```

## Setup DNS @ Georgopol
Edit file ```/etc/bind/named.conf.options``` to allow queries from any source:
```
options {
        directory "/var/cache/bind";

        allow-query{any;};

        auth-nxdomain no;    # conform to RFC1035
        listen-on-v6 { any; };
};
```

Edit the ```/etc/bind/named.conf.local``` file with configurations like below:
```
zone "siren.redzone.it22.com" {
	type master;
	file "/etc/bind/siren/siren.redzone.it22.com";
};
```

Make directory ```etc/bind/siren`
```
mkdir /etc/bind/siren
```

Edit file ```/etc/bind/siren/siren.redzone.it22.com```:
```
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     siren.redzone.it22.com. root.siren.redzone.it22.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      siren.redzone.it22.com.
@       IN      A       192.244.2.1     ; IP Severny
@       IN      AAAA    ::1
www     IN      CNAME   siren.redzone.it22.com.
```

# 10
> Markas juga meminta catatan kapan saja pesawat tempur tersebut menjatuhkan bom, maka buatlah subdomain baru di subdomain siren yaitu log.siren.redzone.xxxx.com serta aliasnya www.log.siren.redzone.xxxx.com yang juga mengarah ke Severny

## Setup DNS @ Georgopol
Edit file ```/etc/bind/siren/siren.redzone.it22.com```:
```
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     siren.redzone.it22.com. root.siren.redzone.it22.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      siren.redzone.it22.com.
@       IN      A       192.244.2.1     ; IP Georgopol
@       IN      AAAA    ::1
www     IN      CNAME   siren.redzone.it22.com.
log     IN      A       192.244.2.1
www.log IN      CNAME   siren.redzone.it22.com.'
```


# 11
> Setelah pertempuran mereda, warga Erangel dapat kembali mengakses jaringan luar, tetapi hanya warga Pochinki saja yang dapat mengakses jaringan luar secara langsung. Buatlah konfigurasi agar warga Erangel yang berada diluar Pochinki dapat mengakses jaringan luar melalui DNS Server Pochinki

## @ Pochinki
Add forwarders containing IP address of Eranger in file ```/etc/bind/named.conf.options```:
```
options {
        directory "/var/cache/bind";

        forwarders {
            192.168.122.1
        }

        allow-query{any;};

        auth-nxdomain no;    # conform to RFC1035
        listen-on-v6 { any; };
};
```

# 12
> Karena pusat ingin sebuah website yang ingin digunakan untuk memantau kondisi markas lainnya maka deploy lah webiste ini (cek resource yg lb) pada severny menggunakan apache

## Deploy @ Severny
Install dependencies
```
apt-get install apache2 -y
apt-get install libapache2-mod-php7.0 -y
apt-get install php php-mcrypt php-mysql -y
```

Edit ```/etc/apache2/mods-enabled/dir.conf``` so that index.php is next to DirectoryIndex
```
<IfModule mod_dir.c>
    DirectoryIndex index.php index.html index.cgi index.pl index.xhtml index.htm
</IfModule>' 
```

Configure domain by editing file ```/etc/apache2/sites-available/it22.conf```:
```
<VirtualHost *:80>
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/it22

    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>' 
```

Enable congifuration
```
a2ensite it22
```

Install dependencies to download the resource that needs to be deployed.
```
apt-get install wget -y
apt-get install unzip -y
```

Make ```/var/www/it22``` directory
```
mkdir -p /var/www/it22
```

Download resource and move to the directory that was made earlier.
```
wget --no-check-certificate 'https://docs.google.com/uc?export=download&id=1xn03kTB27K872cokqwEIlk8Zb121HnfB' -O /tmp/lb.zip
unzip -o /tmp/lb.zip -d /tmp
mv /tmp/worker/* /var/www/it22/ -f
```

Disable default configuration
```
a2dissite 000-default.conf
```

Restart service
```
service apache2 restart
```

# 13
> Tapi pusat merasa tidak puas dengan performanya karena traffic yag tinggi maka pusat meminta kita memasang load balancer pada web nya, dengan Severny, Stalber, Lipovka sebagai worker dan Mylta sebagai Load Balancer menggunakan apache sebagai web server nya dan load balancernya



# 14
> Mereka juga belum merasa puas jadi pusat meminta agar web servernya dan load balancer nya diubah menjadi nginx



# 15
> Markas pusat meminta laporan hasil benchmark dengan menggunakan apache benchmark dari load balancer dengan 2 web server yang berbeda tersebut dan meminta secara detail dengan ketentuan:
> - Nama Algoritma Load Balancer
> - Report hasil testing apache benchmark 
> - Grafik request per second untuk masing masing algoritma. 
> - Analisis



# 16
> Karena dirasa kurang aman karena masih memakai IP, markas ingin akses ke mylta memakai mylta.xxx.com dengan alias www.mylta.xxx.com (sesuai web server terbaik hasil analisis kalian)



# 17
> Agar aman, buatlah konfigurasi agar mylta.xxx.com hanya dapat diakses melalui port 14000 dan 14400.



# 18
> Apa bila ada yang mencoba mengakses IP mylta akan secara otomatis dialihkan ke www.mylta.xxx.com



# 19
> Karena probset sudah kehabisan ide masuk ke salah satu worker buatkan akses direktori listing yang mengarah ke resource worker2



# 20
> Worker tersebut harus dapat di akses dengan tamat.xxx.com dengan alias www.tamat.xxx.com

