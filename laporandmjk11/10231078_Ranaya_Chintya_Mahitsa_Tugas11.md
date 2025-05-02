# [Implementasi Topologi Dasar & VLAN] - [Pekan 11]

## Anggota Kelompok dan Peran
- Meiske Handayani (10231052) - Network Architect
- Muhammad Ariel Rayhan (10231058) - Network Engineer
- Nilam Ayu Nandastari Romdoni (10231070) - Network service Specialist 
- Ranaya Chintya Mahitsa (10231078) - Security & Documentation Specialist 

## Isi Laporan

![Topologi](img\topologi.png)

![Switch Ged A](img\switchgedA.jpg)
```bash
Switch>enable
Switch configure terminal
Switch (config)#vlan 10
Switch (config-vlan) name IT DEPT
Switch (config-vlan) #vlan 20
Switch (config-vlan)# name KEU DEPT
Switch(config-vlan)#vlan 30
Switch(config-vlan) name SDM DEPT
Switch (config-vlan)#vlan 40
Switch(config-vlan)# name SERVER FARM
Switch (config-vlan)#vlan 99
Switch (config-vlan)# name MANAGEMENT
Switch (config-vlan)#interface range fa0/110
Switch(config-if-range) #switchport mode access
Switch (config-if-range) #switchport access vlan 10
Switch (config-if-range) #interface range fa0/11-15
Switch (config-if-range) #switchport mode access
Switch (config-if-range) #switchport access vlan 20
Switch (config-if-range) #interface range fa0/16 20
Switch (config-if-range) #switchport mode access
Switch(config-if-range) #switchport access vlan 30
Switch (config-if-range) #interface range fa0/21 22
Switch (config-if-range) #switchport mode access
Switch (config-if-range)#switchport access vlan 40
Switch (config-if-range) #interface gi0/1
Switch (config-if)#switchport mode trunk Switch (config-if)#switchport trunk native vlan 99
```
Konfigurasi ini bertujuan untuk membuat beberapa VLAN (Virtual LAN) di sebuah switch dan mengalokasikan port tertentu ke masing-masing VLAN. VLAN 10, 20, 30, 40, dan 99 masing-masing dinamai sesuai dengan departemen seperti IT, Keuangan, SDM, Server Farm, dan Manajemen. Port FastEthernet (fa0/1, fa0/11-15, fa0/16 dan fa0/20, fa0/21-22) dikonfigurasi sebagai *access port* dan ditugaskan ke VLAN yang sesuai. Terakhir, port GigabitEthernet gi0/1 dikonfigurasi sebagai *trunk port* untuk mentransmisikan beberapa VLAN antar switch, dengan VLAN 99 diset sebagai *native VLAN*, yaitu VLAN default untuk lalu lintas yang tidak diberi tag.

![Router Ged A](img\routergedA.jpg)
```bash
Router>enable
Router#configure terminal
Router(config)#interface gigabitEthernet0/0.10
Router (config-subif)#encapsulation dot10 10
Router(config-subif)#ip address 192.168.10.1 255.255.255.0
Router (config-subif)#interface gigabitEthernet0/0.20
Router(config-subif)#encapsulation dot10 20
Router (config-subif)#ip address 192.168.20.1 255.255.255.0
Router (config-subif)#interface gigabitEthernet0/0.30
Router(config-subif)#encapsulation dotlo 30
Router(config-subif)#ip address 192.168.30.1 255.255.255.0
Router(config-subif)#interface gigabitEthernet0/0.40
Router(config-subif)#encapsulation dotio 40
Router(config-subif)#ip address 192.168.40.1 255.255.255.0
Router(config-subif)#interface gigabitEthernet0/0.99
Router(config-subif)#encapsulation dot10 99
Router(config-subif)#ip address 192.168.1.2 255.255.255.0
Router(config-subif)#interface gigabitEthernet0/0
Router(config-if)#no shutdown
Router(config-if)#interface gigabitEthernet0/1
Router (config-if)#ip address 172.16.0.1 255.255.255.252
Router(config-if)#no shutdown
LINK-5-CHANGED: Interface GigabitEthernet0/0, changed state to up
LINK-5-CHANGED: Interface GigabitEthernet0/0.10, changed state to up
LINK-5-CHANGED: Interface GigabitEthernet0/0.20, changed state to up
LINK-5-CHANGED: Interface GigabitEthernet0/0.30, changed state to up
LINK-5-CHANGED: Interface GigabitEthernet0/0.40, changed state to up
LINK-5-CHANGED: Interface GigabitEthernet0/0.99, changed state to up
```
Kodingan tersebut merupakan konfigurasi sub-interface pada router Cisco untuk implementasi **Inter-VLAN Routing menggunakan Router-on-a-Stick**, di mana satu interface fisik (GigabitEthernet0/0) dibagi menjadi beberapa sub-interface (0/0.10, 0/0.20, dst.) masing-masing untuk VLAN yang berbeda menggunakan perintah `encapsulation dot1Q`. Setiap sub-interface diberi alamat IP sesuai dengan subnet VLAN-nya untuk menghubungkan antar VLAN, sementara interface GigabitEthernet0/1 dikonfigurasi dengan IP point-to-point. Perintah `no shutdown` di akhir memastikan semua interface aktif.

![Router Ged B](img\routergedB.jpg)
```bash
Router>enable
Router#configure terminal
Enter configuration commands, one per line. End with CNTL/2.
Router(config)#interface gigabitEthernet0/0.50 Router(config-subif)#encapsulation dotl0 50
Router (config-subif)#ip address 192.168.50.1 255.255.255.0
Router(config-subif)#interface gigabitEthernet0/0.60
Router (config-subif)#encapsulation dotlo 60
Router(config-subif)#ip address 192.168.60.1 255.255.255.0
Router(config-subif)#interface gigabitEthernet0/0.99
Router (config-subif)#ip address 192.168.1.3 255.255.255.0
Router (config-subif)#encapsulation dot10 99
Router (config-subif)#interface gigabitEthernet0/0
Router (config-if)#no shutdown
Router (config-if)#interface gigabitEthernet0/1
Router (config-if)#ip address 172.16.0.2 255.255.255.252
Router (config-if)#no shutdown
LINK-5-CHANGED: Interface GigabitEthernet0/0, changed state to up
LINEPROTO-S-UPDOWN: Line protocol on Interface GigabitEthernet0/0, changed state to up
BLINK-5-CHANGED: Interface GigabitEthernet0/0.50, changed state to up
LINEPROTO-S-UPDOWN: Line protocol on Interface GigabitEthernet0/0.50, changed state to up
SLINK-5-CHANGED: Interface GigabitEthernet0/0.60, changed state to up
SLINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0.60, changed state to up
SLINK-5-CHANGED: Interface GigabitEthernet0/0.99, changed state to up
SLINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0.99, changed state to up
```
Konfigurasi ini digunakan untuk mengatur subinterface pada router Cisco dengan metode **Inter-VLAN Routing** menggunakan **router-on-a-stick**. Subinterface `GigabitEthernet0/0` dibagi menjadi tiga VLAN: VLAN 50 (`192.168.50.1`), VLAN 60 (`192.168.60.1`), dan VLAN 99 (`192.168.1.3`) dengan masing-masing perintah `encapsulation dot1Q` untuk menetapkan ID VLAN. Interface utama `GigabitEthernet0/0` dan `GigabitEthernet0/1` diaktifkan menggunakan `no shutdown`, serta interface `GigabitEthernet0/1` dikonfigurasi dengan IP `172.16.0.2/30` sebagai jalur antar-router. Pesan status menunjukkan bahwa semua interface berhasil aktif dan berfungsi.

![Router Utama](img\routerutama.jpg)
```bash
Router>enable
Router configure terminal
Enter configuration commands, one per line. End with CNTL/Z.
Router (config)#interface gigabitEthernet0/0
Router (config-if)#ip address 172.16.0.10 255.255.255.252
Router (config-if)#no shutdown
Router(config-if)#interface gigabitEthernet0/1
Router(config-if)#ip address 172.16.0.11 255.255.255.252
Bad mask /30 for address 172.16.0.11
Router(config-if)#no shutdown
LINK-5-CHANGED: Interface GigabitEthernet0/0, changed state to up
Router(config-if)#
LINK-5-CHANGED: Interface GigabitEthernet0/1, changed state to up
Router (config-if)#ip route 192.168.10.0 255.255.255.0 172.16.0.1
Router (config)#ip route 192.168.20.0 255.255.255.0 172.16.0.1
Router (config)#ip route 192.168.30.0 255.255.255.0 172.16.0.1
Router (config)#ip route 192.168.40.0 255.255.255.0 172.16.0.1
Router (config)#
Router (config)#ip route 192.168.50.0 255.255.255.0 172.16.0.2
Router (config)#ip route 192.168.60.0 255.255.255.0 172.16.0.2
Router (config)#
```
Konfigurasi di atas merupakan pengaturan dasar pada router Cisco yang meliputi pemberian alamat IP pada dua antarmuka (GigabitEthernet0/0 dan 0/1) dengan subnet mask /30 (255.255.255.252), meskipun terjadi kesalahan saat mengatur IP pada interface 0/1. Setelah itu, kedua interface diaktifkan dengan perintah `no shutdown`, dan status koneksi berubah menjadi aktif (up). Selanjutnya, router dikonfigurasi dengan beberapa static route untuk mengarahkan lalu lintas ke jaringan 192.168.x.x melalui gateway 172.16.0.1 dan 172.16.0.2, memungkinkan komunikasi antarjaringan melalui jalur yang telah ditentukan secara manual.

![Switch Ged B](img\switchgedB.jpg)
```bash
Switch>enable
Switch#configure terminal
Switch(config)#vlan 50
Switch (config-vlan)# name MKT DEPT
Switch (config-vlan)#vlan 60
Switch (config-vlan)# name OPS DEPT
Switch (config-vlan)#vlan 99
Switch(config-vlan)# name MANAGEMENT name
Switch (config-vlan)#interface range fa0/1 15
Switch(config-if-range) #switchport mode access
Switch (config-if-range) #switchport access vlan 50
Switch (config-if-range) finterface range fa0/16 30
interface range not validated command rejected
Switch (config)#switchport mode access
Invalid input detected at marker.
Switch (config)#switchport access vlan 60
Invalid input detected at marker.
Switch (config)#interface gi0/1
Switch (config-if)#switchport mode trunk
Switch (config-if)#switchport trunk native vlan 95
```
Kode konfigurasi tersebut digunakan untuk mengatur VLAN pada sebuah switch Cisco. Pertama, mode konfigurasi diaktifkan, kemudian dibuat tiga VLAN: VLAN 50 dengan nama "MKT DEPT", VLAN 60 dengan nama "OPS DEPT", dan VLAN 99 dengan nama "MANAGEMENT". Selanjutnya, interface FastEthernet dari fa0/1 hingga fa0/15 dikonfigurasi sebagai access mode dan ditetapkan ke VLAN 50. Terdapat kesalahan saat mencoba mengonfigurasi fa0/16 hingga fa0/30 karena penulisan perintah yang salah. Kesalahan lain juga muncul saat mencoba mengatur mode access dan VLAN di luar konteks interface. Terakhir, interface GigabitEthernet 0/1 dikonfigurasi sebagai trunk dan diatur VLAN native-nya ke VLAN 95.