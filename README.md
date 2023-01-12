
<h1 align="center">
  <img src="https://raw.githubusercontent.com/MetaCubeX/Clash.Meta/Meta/Meta.png" alt="Clash" width="200">
  <br>Config Premium OpenClash Meta OpenWrt<br>

</h1>

  <p align="center">
  <a target="_blank" href="https://github.com/rizkisquadpants/OpenClashMeta/archive/refs/heads/main.zip">
    <img src="https://img.shields.io/github/v/release/rizkisquadpants/OpenClashMeta?style=for-the-badge&label=Download">
  </a>
  </p>
  

<p align="center">
Config OpenClash Meta OpenWrt Spesial Edition
</p>
<p align="center">
Support Multi-WAN, Pisah Traffic, Bypass Traffic, Support Game, Support Streaming
</p>
<p align="center">
- Terimakasih <a href="https://github.com/vernesong/OpenClash" target="_blank"> Vernesong </a> For Clash OpenWrt ，<a href="https://github.com/malikshi" target="_blank"> Maliksih </a> Untuk Rule Provider ，<a href="https://www.facebook.com/r3yr3" target="_blank"> Reyre </a>Best Firmware  ，Dan <a href="https://howdy.id/" target="_blank"> Howdy </a>The Best Server -
</p>

**Konten Tabel**
  
  - [Openclash](#openclash)
    - [Setting OpenClash](#Setting-OpenClash)
    - [Edit Files Proxy Provider](#edit-files-proxy-provider)
    - [Multi-WAN ISP](#multi-wan-isp)
    - [Edit Files Rule Provider](#edit-files-rule-provider)
    - [Setting Yacd](#setting-yacd)


## Openclash

Plugin ini adalah klien Clash yang bisa dijalankan di OpenWrt. Kompatibel dengan Shadowsocks ShadowsocksR, Vmess, Trojan, Snell dan protokol lainnya, dan mengimplementasikan proxy kebijakan sesuai dengan konfigurasi aturan yang fleksibel.

 <img src="/assets/Main.png">

### Setting OpenClash

## Global Setting

Hasil settingan pada global setting akan meng-overide settingal awal pada file main.yaml.

### Operation Mode


* Pertama Ubah Redir Host To Fake-Ip:
<img src="/assets/SetToFakeIP.png" border="0">

* Pada Select Mode Rubah Menjadi fake-ip-mix(tun mix mode):
* Ikuti Sesuai Gambar
<img src="/assets/Opration.PNG" border="0">


### DNS Settings

* Ceklist/centang opsi sesuai gambar berikut:
<img src="/assets/DNS.png" border="0">


* Secroll Kebawah Dan Isi Custom DNS Seperti Di Bawah Ini
<img src="/assets/CustomDNS.png" border="0">

DNS Server Group | DNS Server Address | DNS Server Port | DNS Server Type
------------ | ------------- | ------------- | -------------
NameServer | 94.140.14.14 |   | UDP
NameServer | 94.140.15.15 |  | UDP
NameServer | dns.adguard-dns.com/dns-query |   | HTTPS
NameServer | dns.adguard-dns.com |  | TLS


### Meta Settings

Aktifkan Meta Core Seperti Gambar Di Bawah Ini
<img src="/assets/Meta.png" border="0">

Scroll Kebawah Dan Setting Seperti Ini
#### Geoip
* Ceklis Pada Enable GeoIP Dat
* Kemudian Ceklis Auto Update GeoIP Dat
* Pada Custom GeoIP Dat URL Isi URL Di Bawah Ini Kemudian Enter

`https://cdn.jsdelivr.net/gh/Loyalsoldier/v2ray-rules-dat@release/geoip.dat`

#### GeoSite
* Ceklis Pada Auto Update GeoSite Database
* Pada Custom GeoSite URL Isi URL Di Bawah Ini Kemudian Enter

`https://cdn.jsdelivr.net/gh/Loyalsoldier/v2ray-rules-dat@release/geosite.dat`

Sesuaikan Seperti Gambar Di Bawah
<img src="/assets/Meta Geo.png" border="0">

### Edit Files Proxy Provider


Mengisi akun tunnel pada 2 files pada folder proxy_provider yang terdiri dari  `pp-indo.yaml`, dan `pp-browsing.yaml`.
Fungsi dari proxy_provider diatas:
* pp-indo.yaml, Untuk Akun berlokasi INDONESIA untuk keperluan akses websites/marketplace/live stream apps/Video on Demand yang mengharuskan memakai IP Address Publik Indonesia.
* pp-browsing.yaml, Untuk Akun Browsing Rekomendasi SGGS

Untuk Gaming Disarankan Menggunakan Direct Reguler Tanpa Inject


### Multi-WAN ISP

Default Confing Menggunakan 2 ISP, Untung Pengguna 1 ISP Silahkan Edit Config `CONFIG-RTA-SERVER.yaml`,Kemudian Edit Sedikit Config Tersebut Untuk Merubah `interface-name` Sesuai Interface yg Dipakai, Default yg Dipakai `eth1` dan `eth2` . Mengecek nama interface bisa dari LuCI > Network > Interface atau bisa jalankan commandline `ifconfig`.
Setelah memperoleh nama interface dari tiap WAN suudah di tentukan maka tinggal cara settingnya sebagai berikut:
* Edit file CONFIG-RTA-SERVER.yaml.

    Edit `interface-name: ` pada list proxy-groups dengan mengisikan nama interface WAN dari modem kalian. Dan perlu diperhatikan jika hanya menggunakan 1 WAN saja silahkan edit seperti berikut.
    
   
    proxy-groups dari DIRECT ISP1.
    ```yaml
      - name: DIRECT ISP1
        type: url-test
        disable-udp: false
        proxies:
        - DIRECT
        url: http://www.gstatic.com/generate_204
        interval: '30'
        interface-name: eth1
    ```
    proxy-groups dari DIRECT ISP2.
    ```yaml
      - name: DIRECT ISP2
        type: url-test
        disable-udp: false
        proxies:
        - DIRECT
        url: http://www.gstatic.com/generate_204
        interval: '30'
        interface-name: eth2
    ```
	
	proxy-groups dari DIRECT ISP2. Jika Menggunakan 1 ISP Dan Hapus Juga Pada Bagian Di Bawah Ini
	
	```yaml
     - name: ✔️ Direct Mode
     type: select
     disable-udp: false
     proxies:
     - DIRECT ISP1
     - DIRECT ISP2 #Hapus Baris Ini
     - 🔰 Browsing 🔰
     
     - name: 🎮 Gaming UDP
     type: select
     disable-udp: false
     proxies:
     - DIRECT ISP1
     - DIRECT ISP2 #Hapus Baris Ini
     use:
     - PP-Gaming
     url: http://www.gstatic.com/generate_204
     interval: '30'

     - name: DIRECT LB
     type: load-balance
     strategy: round-robin
     disable-udp: false
     proxies:
     - DIRECT ISP1
     - DIRECT ISP2 #Hapus Baris Ini
     url: http://www.gstatic.com/generate_204
     interval: '30'
  ```

* Edit Setiap file pada folder Proxy_Provider

    Sebagai Contoh Akun Trojan Untuk LB 2 ISP
    
    ISP1
    ```yaml
    - name: "ID BLABLA ISP1"
      type: trojan
      server: aaa.bbb.ccc.ddd
      port: 443
      password: PASSWORD
      udp: true
      sni: BUGSNI.COM
      alpn:
        - h2
        - http/1.1
      skip-cert-verify: true
      interface-name: eth1
    ```
  
	  ISP2
    ```yaml
    - name: "ID BLABLA ISP2"
      type: trojan
      server: aaa.bbb.ccc.ddd
      port: 443
      password: PASSWORD
      udp: true
      sni: BUGSNI.COM
      alpn:
        - h2
         - http/1.1
      skip-cert-verify: true
      interface-name: eth2
    ```

### Edit Files Rule Provider

Untuk `rp-direct.yaml` bersifat offline Bisa Di Edit Sebagai Contoh Direct Whatsapp


### Setting Yacd

Yacd adalah yet another clash dashboard, yakni dashboard clash yang dapat digunakan untuk mengatur proxy dan memantau services clash yang dijalankan oleh Openclash.
<img src="/assets/Yacd.png">

#### Proxies

Untuk pertama kali start openclash maka harus setting proxies `GLOBAL` ke proxy-groups `🔰 Browsing 🔰`.
 <img src="/assets/Yacd Global.png">
