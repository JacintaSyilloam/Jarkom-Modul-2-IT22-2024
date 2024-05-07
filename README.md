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



## 1
> Untuk membantu pertempuran di Erangel, kamu ditugaskan untuk membuat jaringan komputer yang akan digunakan sebagai alat komunikasi. Sesuaikan rancangan Topologi dengan rancangan dan pembagian yang berada di link yang telah > disediakan, dengan ketentuan nodenya sebagai berikut:
> - DNS Master akan diberi nama Pochinki, sesuai dengan kota tempat dibuatnya server tersebut
> - Karena ada kemungkinan musuh akan mencoba menyerang Server Utama, maka buatlah DNS Slave Georgopol yang mengarah ke Pochinki
> - Markas pusat juga meminta dibuatkan tiga Web Server yaitu Severny, Stalber, dan Lipovka. Sedangkan Mylta akan bertindak sebagai Load Balancer untuk server-server tersebut


## 2
>Karena para pasukan membutuhkan koordinasi untuk mengambil airdrop, maka buatlah sebuah domain yang mengarah ke Stalber dengan alamat airdrop.xxxx.com dengan alias www.airdrop.xxxx.com dimana xxxx merupakan kode kelompok. Contoh : airdrop.it01.com



## 3
> Para pasukan juga perlu mengetahui mana titik yang sedang di bombardir artileri, sehingga dibutuhkan domain lain yaitu redzone.xxxx.com dengan alias www.redzone.xxxx.com yang mengarah ke Severny



## 4
> Markas pusat meminta dibuatnya domain khusus untuk menaruh informasi persenjataan dan suplai yang tersebar. Informasi persenjataan dan suplai tersebut mengarah ke Mylta dan domain yang ingin digunakan adalah loot.xxxx.com dengan alias www.loot.xxxx.com



## 5
> Pastikan domain-domain tersebut dapat diakses oleh seluruh komputer (client) yang berada di Erangel



## 6
> Beberapa daerah memiliki keterbatasan yang menyebabkan hanya dapat mengakses domain secara langsung melalui alamat IP domain tersebut. Karena daerah tersebut tidak diketahui secara spesifik, pastikan semua komputer (client) dapat mengakses domain redzone.xxxx.com melalui alamat IP Severny (Notes : menggunakan pointer record)



## 7
> Akhir-akhir ini seringkali terjadi serangan siber ke DNS Server Utama, sebagai tindakan antisipasi kamu diperintahkan untuk membuat DNS Slave di Georgopol untuk semua domain yang sudah dibuat sebelumnya



## 8
> Kamu juga diperintahkan untuk membuat subdomain khusus melacak airdrop berisi peralatan medis dengan subdomain medkit.airdrop.xxxx.com yang mengarah ke Lipovka



## 9
> Terkadang red zone yang pada umumnya di bombardir artileri akan dijatuhi bom oleh pesawat tempur. Untuk melindungi warga, kita diperlukan untuk membuat sistem peringatan air raid dan memasukkannya ke subdomain siren.redzone.xxxx.com dalam folder siren dan pastikan dapat diakses secara mudah dengan menambahkan alias www.siren.redzone.xxxx.com dan mendelegasikan subdomain tersebut ke Georgopol dengan alamat IP menuju radar di Severny



## 10
> Markas juga meminta catatan kapan saja pesawat tempur tersebut menjatuhkan bom, maka buatlah subdomain baru di subdomain siren yaitu log.siren.redzone.xxxx.com serta aliasnya www.log.siren.redzone.xxxx.com yang juga mengarah ke Severny



## 11
> Setelah pertempuran mereda, warga Erangel dapat kembali mengakses jaringan luar, tetapi hanya warga Pochinki saja yang dapat mengakses jaringan luar secara langsung. Buatlah konfigurasi agar warga Erangel yang berada diluar Pochinki dapat mengakses jaringan luar melalui DNS Server Pochinki



## 12
> Karena pusat ingin sebuah website yang ingin digunakan untuk memantau kondisi markas lainnya maka deploy lah webiste ini (cek resource yg lb) pada severny menggunakan apache



## 13
> Tapi pusat merasa tidak puas dengan performanya karena traffic yag tinggi maka pusat meminta kita memasang load balancer pada web nya, dengan Severny, Stalber, Lipovka sebagai worker dan Mylta sebagai Load Balancer menggunakan apache sebagai web server nya dan load balancernya



## 14
> Mereka juga belum merasa puas jadi pusat meminta agar web servernya dan load balancer nya diubah menjadi nginx



## 15
> Markas pusat meminta laporan hasil benchmark dengan menggunakan apache benchmark dari load balancer dengan 2 web server yang berbeda tersebut dan meminta secara detail dengan ketentuan:
> - Nama Algoritma Load Balancer
> - Report hasil testing apache benchmark 
> - Grafik request per second untuk masing masing algoritma. 
> - Analisis



## 16
> Karena dirasa kurang aman karena masih memakai IP, markas ingin akses ke mylta memakai mylta.xxx.com dengan alias www.mylta.xxx.com (sesuai web server terbaik hasil analisis kalian)



## 17
> Agar aman, buatlah konfigurasi agar mylta.xxx.com hanya dapat diakses melalui port 14000 dan 14400.



## 18
> Apa bila ada yang mencoba mengakses IP mylta akan secara otomatis dialihkan ke www.mylta.xxx.com



## 19
> Karena probset sudah kehabisan ide masuk ke salah satu worker buatkan akses direktori listing yang mengarah ke resource worker2



## 20
> Worker tersebut harus dapat di akses dengan tamat.xxx.com dengan alias www.tamat.xxx.com

