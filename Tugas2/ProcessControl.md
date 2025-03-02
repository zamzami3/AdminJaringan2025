# Proses Kontrol

Proses kontrol dalam jaringan mengacu pada serangkaian mekanisme dan prosedur yang diterapkan untuk memastikan kinerja, keamanan, dan efisiensi jaringan komputer. Di tiap proses, terdiri dari adress dan data structure di kernel. 

Data Structure yang ada di dalam kernel, bisa memantau proses, dan mencatat berbagai informasi:
  1. Alamat space map proses
  2. Status proses: proses berjalan, jeda, dll.
  3. Prioritas proses
  4. Informasi sumber daya yang digunakan. Misalnya seperti CPU, memori, dsb.
  5. Kumpulan sinyal yang yang diblokir
  6. ID user yang memulai proses

### PID (Process ID Number)
Setiap proses, ada ID unik berupa bilangan bulat di kernel. Digunakan untuk mengirim signal ke sebuah proses.
Namespaces process memungkinkan ada proses berbeda dan punya PID yang sama, Namespaces dipakai untuk membuat kontainer yang digunakan untuk menjalankan beberapa contoh aplikasi pada sistem yang sama

### PPID (Parent Process ID Number)
Tiap proses dikaitkan dengan parent process. Nomor ID parent process, yakni PID. PPID dipakai untuk merujuk ke parent process di panggilan sistem, seperti mengirim signal.

### UID dan EUID (User ID dan Effective Usr ID)
ID user (UID) adalah ID yang memulai proses. EUID merupakan ID user yang dugunakan proses untuk menentukan sumber daya yang dapat diakses oleh proses. Selain itu, EUID dipakai untuk mengontrol akses ke berkas, port jaringan, dsb.

# Life Cycle of a Process
Dalam new process, ada namanya **fork system** yang membuat salinan dari proses asli. Salinan yang dibuat, identik dengan parent process. Linux System menggunakan clone, superset dari fork yang menangani thread dan menyertakan fitur tambahan. **Fork** ada di kernel untuk backward compability tetapi memanggil **clone** secara internal.

### Signal
Cara mengirim pemberitahuan ke suatu proses.
- Dapat dikirim antar proses untuk komunikasi
- Dikirim oleh driver terminal untuk berhenti, jeda, menyela, atau menangguhkan proses
- Dikirim oleh administrator (kill)
- Dikirim oleh kernel saat proses melakukan pelanggaran. Misal pembagian dengan nol
- Dikirim oleh kernel untuk memberi tahu proses tentang kondisi seperti kematian suatu proses anak atau ketersediaan data pada saluran I/O.
![image1](https://github.com/user-attachments/assets/5e31642f-c67c-48e9-a123-1798408f0d72)

Perbedaan Sinyal KILL, INT, TERM, HUP, dan QUIT;

- KILL tidak dapat diblokir dan menghentikan proses di tingkat kernel. 
- INT dikirim oleh driver terminal saat pengguna mengetik (Permintaan untuk mengakhiri operasi saat ini. Program harus berhenti (jika menangkap sinyal) atau membiarkan dirinya dimatikan, yang merupakan default jika sinyal tidak tertangkap). Program yang memiliki baris perintah interaktif (seperti shell) harus menghentikan apa yang sedang dilakukannya, membersihkan, dan menunggu masukan pengguna lagi.
- TERM yakni permintaan untuk menghentikan eksekusi sepenuhnya. Proses penerima diharapkan akan membersihkan statusnya dan keluar.
- HUP dikirim ke suatu proses saat terminal kontrol ditutup. Awalnya digunakan untuk menunjukkan "putusnya sambungan telepon", dan sekarang digunakan untuk memerintahkan proses daemon untuk mengakhiri dan memulai ulang, memperhitungkan konfigurasi baru. 
- QUIT mirip dengan TERM, kecuali bahwa ia secara default menghasilkan core dump jika tidak tertangkap.

# PS (Monitoring Processes)
Perintah ps untuk memantau proses, dapat menampilkan PID, UID, prioritas, dan terminal kontrol suatu proses. Memberi tahu berapa banyak memori yang digunakan di suatu proses, banyak waktu CPU yang sudah dikonsumsi, dan status saat ini.

Ada gambaran umum tentang sistem dengan ps aux. a untuk menunjukkan proses semua user, u untuk informasi terperinci tiap proses, x untuk menujukkan proses yang tidak terkait dengan terminal 

~# ps aux 
| USER | PID | %CPU | %MEM | VSZ | RSS | TTY | STAT | START | TIME COMMAND |
|------|-----|------|------|-----|-----|-----|------|-------|--------------| 
| root           1  7.9  0.1  21604 12852 ?        Ss   05:08   0:00 /sbin/init |
| root           2  0.0  0.0   2616  1444 ?        Sl   05:08   0:00 /init |
| root           6  0.0  0.0   3024   420 ?        Sl   05:08   0:00 plan9 --control-socket 6 --log-level 4 --server-fd 7 |
| root          54  2.0  0.1  33896 12644 ?        S<s  05:08   0:00 /usr/lib/systemd/systemd-journald |
| root          97  2.2  0.0  23992  6028 ?        Ss   05:08   0:00 /usr/lib/systemd/systemd-udevd |
| systemd+     155  1.7  0.1  21452 12024 ?        Ss   05:08   0:00 /usr/lib/systemd/systemd-resolved |
| systemd+     156  1.0  0.0  91020  6552 ?        Ssl  05:08   0:00 /usr/lib/systemd/systemd-timesyncd |
| root         162  0.1  0.0   4236  2688 ?        Ss   05:08   0:00 /usr/sbin/cron -f -P |
| message+     163  0.7  0.0   9584  5044 ?        Ss   05:08   0:00 @dbus-daemon --system --address=systemd: --nofork --n
| root         178  5.9  0.5 2140292 42408 ?       Ssl  05:08   0:00 /usr/lib/snapd/snapd
| root         179  4.0  0.1  17976  8352 ?        Ss   05:08   0:00 /usr/lib/systemd/systemd-logind
| root         183  4.0  0.1 1756096 14080 ?       Ssl  05:08   0:00 /usr/libexec/wsl-pro-service -vv
| syslog       200  2.7  0.0 222508  5284 ?        Ssl  05:08   0:00 /usr/sbin/rsyslogd -n -iNONE
| root         206  0.2  0.0   3160  1224 hvc0     Ss+  05:08   0:00 /sbin/agetty -o -p -- \u --noclear --keep-baud - 1152
| root         209  0.1  0.0   3116  1140 tty1     Ss+  05:08   0:00 /sbin/agetty -o -p -- \u --noclear - linux
| root         240  2.9  0.2 107008 22532 ?        Ssl  05:08   0:00 /usr/bin/python3 /usr/share/unattended-upgrades/unatt
| root         266  1.5  0.0  17276  6612 ?        Ss   05:08   0:00 /usr/lib/systemd/systemd-timedated
| root         309  0.0  0.0   2616   124 ?        Ss   05:09   0:00 /init
| root         310  0.0  0.0   2616   124 ?        S    05:09   0:00 /init
| root         311  0.3  0.0   6072  5160 pts/0    Ss   05:09   0:00 -bash
| root         312  0.0  0.0   6668  4608 pts/1    Ss   05:09   0:00 /bin/login -f
| root         410  1.4  0.1  20256 11464 ?        Ss   05:09   0:00 /usr/lib/systemd/systemd --user
| root         411  0.0  0.0  21152  1712 ?        S    05:09   0:00 (sd-pam)
| root         426  0.1  0.0   6072  5012 pts/1    S+   05:09   0:00 -bash
| root         505  0.0  0.0  23996  3040 ?        S    05:09   0:00 (udev-worker)
| root         506  0.0  0.0  23996  3108 ?        S    05:09   0:00 (udev-worker)
| root         507  0.0  0.0   8280  4172 pts/0    R+   05:09   0:00 ps aux

---

Ada argumen lax, untuk informasi lebih teknis tentang proses dan sedikit lebih cepat daripada aux karena tidak perlu menyelesaikan username dan grup. 

~# ps lax
F   UID     PID    PPID PRI  NI    VSZ   RSS WCHAN  STAT TTY        TIME COMMAND
4     0       1       0  20   0  21604 12852 -      Ss   ?          0:00 /sbin/init
5     0       2       1  20   0   2616  1444 x64_sy Sl   ?          0:00 /init
0     0       6       2  20   0   2616   132 -      Sl   ?          0:00 plan9 --control-socket 6 --log-level 4 --server-fd 7 --pipe-fd 9 --log-truncate
4     0      54       1  19  -1  34052 12680 -      S<s  ?          0:00 /usr/lib/systemd/systemd-journald
4     0      97       1  20   0  23992  6028 -      Ss   ?          0:00 /usr/lib/systemd/systemd-udevd
4   991     155       1  20   0  21452 12024 -      Ss   ?          0:00 /usr/lib/systemd/systemd-resolved
4   996     156       1  20   0  91020  6552 -      Ssl  ?          0:00 /usr/lib/systemd/systemd-timesyncd
4     0     162       1  20   0   4236  2688 hrtime Ss   ?          0:00 /usr/sbin/cron -f -P
4   101     163       1  20   0   9584  5044 -      Ss   ?          0:00 @dbus-daemon --system --address=systemd: --nofork --nopidfile --systemd-activation
4     0     179       1  20   0  17976  8352 -      Ss   ?          0:00 /usr/lib/systemd/systemd-logind
4     0     183       1  20   0 1756096 14080 futex_ Ssl ?          0:00 /usr/libexec/wsl-pro-service -vv
4   102     200       1  20   0 222508  5284 core_s Ssl  ?          0:00 /usr/sbin/rsyslogd -n -iNONE
4     0     206       1  20   0   3160  1224 core_s Ss+  hvc0       0:00 /sbin/agetty -o -p -- \u --noclear --keep-baud - 115200,38400,9600 vt220
4     0     209       1  20   0   3116  1140 core_s Ss+  tty1       0:00 /sbin/agetty -o -p -- \u --noclear - linux
4     0     240       1  20   0 107008 22532 x64_sy Ssl  ?          0:00 /usr/bin/python3 /usr/share/unattended-upgrades/unattended-upgrade-shutdown --wait-
5     0     309       2  20   0   2616   124 -      Ss   ?          0:00 /init
5     0     310     309  20   0   2616   124 x64_sy S    ?          0:00 /init
4     0     311     310  20   0   6072  5160 do_wai Ss   pts/0      0:00 -bash
4     0     312       2  20   0   6668  4608 do_wai Ss   pts/1      0:00 /bin/login -f
4     0     410       1  20   0  20256 11464 -      Ss   ?          0:00 /usr/lib/systemd/systemd --user
5     0     411     410  20   0  21152  1712 -      S    ?          0:00 (sd-pam)
4     0     426     312  20   0   6072  5012 core_s S+   pts/1      0:00 -bash
4     0     515     311  20   0   8312  4072 -      R+   pts/0      0:00 ps lax

---

Untuk proses tertentu, bisa menggunakan grep untuk filter output ps
$ ps aux | grep -v grep | grep firefox

Menentukan PID suatu proses dengan menggunakan pgrep atau pidof.
$ pgrep firefox
$ pidof /usr/bin/firefox

# Pemantauan interaktif
