<div align="center">
  <h1 style="text-align: center;font-weight: bold">Laporan Praktikum<br>Workshop Administrasi Jaringan</h1>
</div>
<br />

<div align="center">
  
  ![Logo PENS](image/LogoPENSnNoBg.png)
  
  <h3 style="text-align: center;">Dosen Pengampu:</h3>
  <h4 style="text-align: center;">Dr. Ferry Astika Saputra, S.T., M.Sc.</h4>

  <h3 style="text-align: center;">Disusun Oleh:</h3>
  <p style="text-align: center;">
    <strong>Zalfail Mumtaza Attamami</strong><br>
    <strong>3123600003</strong>
  </p>

<h3 style="text-align: center;line-height: 1.5">Politeknik Elektronika Negeri Surabaya<br>Departemen Teknik Informatika Dan Komputer<br>Program Studi Teknik Informatika<br>2024/2025</h3>
  <hr><hr>
</div>

### 1. Konfigurasi VM 1 (Server)
Virtual Machine No GUI (CLI) sebagai server.
Menggunakan dua network adapter:
Adapter 1: Bridge Adapter
Adapter 2: Internal Network

![Adapter 1](image/Adapter1.png)

![Adapter 2](image/Adapter2.png)

**Bridged Adapter** berguna untuk memberikan konektivitas jaringan yang setara dengan perangkat fisik lain di jaringan lokal, memungkinkan komunikasi dua arah dan simulasi lingkungan jaringan yang realistis (untuk koneksi langsung ke jaringan luar). **Internal Network** untuk membuat jaringan di dalam komputer sendiri, terpisah dari jaringan luar.

#### Konfigurasi IP Address

1. Cek IP Address dengan perintah **ip a**

   ![ip a](image/ipa_nogui.png)

2. Tambahkan file `nano -l -w /etc/network/interfaces`

   ![Add Screenshot](image/auto.png)

3. Mengaktifkan forwarding di Server dengan `nano -l -w /etc/sysctl.conf`

    ![Add Screenshot](image/etc-sysctl.png)

4. Instalasi iptables

    ![Add Screenshot](image/iptables.png)

5. Menambah kode ke `/etc/iptables/rules.v4` menggunakan perintah `sudo nano -l -w /etc/iptables/rules.v4.`

    ![Add Screenshot](image/addcode2.png)

6. Menjalankan perintah `sudo iptables-restore < /etc/iptables/rules.v4`

    ![Add Screenshot](image/iptables-restore.png)

7. Melakukan reboot VM dengan perintah sudo reboot

#### Instalasi NTP Client

8. Instal NTP
   
   ![Add Screenshot](image/install-ntp.png)

9. Perintah `sudo nano /etc/ntpsec/ntp.conf` untuk set server

   ![App Screenshot](image/set-server-ip.png)

10. Menjalankan perintah `systemctl status ntpsec` 

   ![App Screenshot](image/status-ntp.png)

11. Melihat sudah tersambung dengan server, pakai perintah `ntpq -p`

   ![App Screenchot](image/validasi-koneksi-ntp.png)

#### Konfigurasi File Samba di VM 1 (No GUI)

1. Instalasi samba
   
   ![App Screenshot](image/install-samba.png)

2. Membuat folder share dan mengatur permission

   ![App Screenshot](image/mkdir-samba.png)

   Sebelumnya, saya sudah membuat directory /home/share, maka dari
   itu dalam output tertera bahwa **file exist**

3. Menambahkan konfigurasi file `etc/samba/smb.conf`

   ![App Screenshot](image/konfigurasi-samba.png)

4. Restart layanan samba

   ![App Screenshot](image/restart-status-samba.png)

#### Konfigurasi DNS Server pada VM 1
1. Instalasi paket DNS Server

   ![App Screenshot](image/dns-bind-utils.png)

2. Menambah konfigurasi pada file `/etc/bind/named.conf`

   ![App Screenshot](image/konfigurasi-dns.png)

3. Mengedit file opsi `/etc/bind/named/conf.options`

   ![App Screenshot](image/konfigurasi-dns-conf-options.png)

4. Membuat internal zone di `/etc/bind/named.conf.internal-zones`

   ![App Screenshot](image/internal-zone.png)

5. Menambahkan opsi `-4` pada file `/etc/default/named`

   ![App Screenshot](image/tambah-min4.png)

6. Membuat konfigurasi domain lokal

   ![App Screenshot](image/konfigurasi-domain-lokal.png)

7. Membuat file konfigurasi berdasarkan IP address

   ![App Screenshot](image/konfigurasi-ip-addr.png)

### Konfigurasi VM 2 (client)

   ![App Screenshot](image/vm2.png)

Melakukan ping

   ![App Screenshot](image/ping-address.png)

   ![App Screenshot](image/ping-gateaway.png)

   ![App Screenshot](image/ping-1111.png)

   ![App Screenshot](image/ping-8888.png)

   ![App Screenshot](image/ping-google.png)
