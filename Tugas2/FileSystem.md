# Ch 4: Filesystem

![Picture1](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*quw0WvsLLCxad3WC6fjQ1Q.png)

Filesystem berfungsi untuk mengatur dan mengorganisir penyimpanan di dalam sistem komputer yang terdiri dari empat komponen utama:
1. Namespace
   - Cara memberi nama dan mengorganisir file dalam bentuk hierarki (seperti folder dan subfolder).
2. API
   - Kumpulan perintah yang digunakan untuk mengakses, menavigasi, dan mengelola file.
Model Keamanan – Mekanisme untuk melindungi, menyembunyikan, dan berbagi file agar hanya bisa diakses oleh pengguna tertentu.
3. Implementasi
   - Perangkat lunak yang menghubungkan sistem file dengan perangkat penyimpanan (hard disk, SSD, dll.).
   -
Filesystem yang dipakai pada penyimpanan disk meliputi ext4, XFS, dan UFS, serta ZFS dan Btrfs dari Oracle. Ada juga sistem file lain seperti VxFS dari Veritas dan JFS dari IBM.

# Pathnames
Yakni daftar direktori yang mengarah ke sebuah file dalam sistem file. Pathname ini menunjukkan lokasi file dalam struktur hierarki sistem. 
Kata "folder" berasal dari Windows dan macOS. Dalam konteks teknis, menggunakan istilah direktori.

Pathname bisa dibedakan menjadi dua jenis:
1. Absolute Path (Path Mutlak)
Yang menunjukkan lokasi file secara lengkap dari root `/`. 
`/home/username/file.txt`
2. Relative Path (Path Relatif)
Yang menunjukkan lokasi file berdasarkan posisi saat ini.
`./file.txt`

# Filesystem Mounting and Unmounting
`mount /dev/sda /users`

Linux ada opsi lazy unmount (**umount -l**) yang menghapus filesystem. Jika **umount -f** itu berguna saat filesystem sibuk. Sedangkan **lsof** atau **fuser** mengetahui proses yang menggunakan filesystem dan mematikan. 

# File Tree
Penamaan di UNIX tidak kompatible secara bersamaan, sulit untuk upgrade operating system. 
Root filesystem bagian inti dari sistem file yang setidaknya mencakup direktori root (`/`) serta sejumlah file dan subdirektori penting.  

- Kernel OS biasanya disimpan di dalam `/boot`, tapi nama dan lokasinya bisa bervariasi tergantung operating system. Di BSD dan beberapa sistem UNIX lainnya, kernel bukan hanya satu file, melainkan kumpulan komponen.  
- Konfigurasi sistem terdapat di dalam `/etc`.  
     - Perintah penting untuk administrator ada di `/sbin` dan `/bin`.
     - File sementara kadang disimpan di `/tmp`.
     - Perangkat sistem (`/dev`) dulu bagian dari root filesystem, tetapi sekarang lebih sering dipasang sebagai filesystem virtual terpisah.  

Struktur Direktori Tambahan 
- Library bersama dan file lain ditemukan di `/lib`, `/lib64` atau beberapa sistem di `/usr/lib`, dengan `/lib` hanya sebagai symbolic link.  
- Direktori `/usr` menyimpan program yang tidak kritis untuk sistem, termasuk dokumentasi, pustaka tambahan, dan konfigurasi tambahan (seperti di FreeBSD yang menggunakan `/usr/local`).  
- Direktori `/var` berisi log sistem, direktori spool, data akuntansi, serta file lain yang sering berubah dan spesifik untuk setiap host.  

![Picture2](https://github.com/ferryastika/unix-and-linux-sysadmin-notes/blob/main/the-filesystem/data/pathnames.png)

# File Types
Sebagian besar filesystem mendefinisikan 7 jenis file:
1. Regular files
File biasa yang berisi data, seperti teks atau biner.
2. Directories
Folder yang berfungsi sebagai wadah untuk menyimpan file lain.
3. Character device files
Berhubungan dengan perangkat yang mengirim atau menerima data karakter demi karakter, seperti keyboard atau terminal.
4. Block device files
Digunakan untuk perangkat yang mengakses data dalam blok, seperti hard disk atau USB drive.
5. Local domain sockets
Digunakan untuk komunikasi antar proses (IPC) dalam satu sistem.
6. Named pipes (FIFOs)
Mirip socket, tapi bekerja secara satu arah untuk komunikasi antar proses.
7. Symbolic links
File yang berfungsi sebagai referensi atau shortcut ke file lain.

Bisa menggunakan `ls -ld` dengan -d adalah flag forces untuk menampilkan informasi directory.

**Regular files** terdiri dari urutan byte tanpa struktur tertentu. Contohnya adalah text file, data files, execute program, dan shared libraries are all stored.

**Directories** merupakan tempat yang berisi referensi atau daftar file lain, seperti folder dalam sistem operasi lainnya.

**Hard links** yakni cara untuk memberikan beberapa nama pada satu file yang sama dan bisa membuat hard link dengan perintah `ln`. Jika ingin melihat jumlah hard link yang dimiliki suatu file, pakai `ls -i`.

# Atribut File
Dalam model file system  Unix dan Linux, setiap file punya 9 bit izin akses yang menentukan siapa yang bisa membaca (read), menulis (write), dan menjalankan (execute) file tersebut. Selain itu, ada 3 bit tambahan yang terutama mempengaruhi cara kerja file yang dapat dieksekusi. Gabungan dari 12 bit disebut sebagai file's mode.
Selain itu, ada 4 bit tipe file yang menentukan jenis file (misalnya file biasa, direktori, atau tautan simbolik) yang ditetapkan saat file dibuat dan tidak bisa diubah. Namun, pemilik file atau superuser (root) dapat mengubah 12 bit mode dengan perintah `chmod`.

### Permission bit
Setiap file dalam sistem Unix/Linux punya 9 bit izin akses yang terbagi menjadi tiga kelompok: **owner (u)** untuk pemilik file, **group (g)** untuk grup pemilik file, dan **others (o)** untuk pengguna lain. Untuk mengingat urutannya, pakai kata **Hugo**, di mana **u** mewakili owner, **g** untuk group, dan **o** untuk others.  

Izin akses juga dapat dinyatakan dalam notasi oktal (basis 8), di mana setiap kelompok terdiri dari tiga bit. Untuk owner, permission bits bernilai 400 (read), 200 (write), dan 100 (execute). Untuk group, permission bits bernilai 40 (read), 20 (write), dan 10 (execute), sedangkan untuk others, permission bits bernilai 4 (read), 2 (write), dan 1 (execute). Misalnya, permission **rwxr-xr--** dapat direpresentasikan dalam bentuk oktal sebagai **754** (7=4+2+1, 5=4+0+1, 4=4+0+0).  

Setiap bit memiliki makna tersendiri. Bit **read (r)** memungkinkan file untuk dibaca, **write (w)** memungkinkan perubahan isi file (meskipun penghapusan atau penggantian nama file tergantung pada izin direktori tempat file berada), dan **execute (x)** memungkinkan file dijalankan sebagai program. Terdapat dua jenis file yang bisa dieksekusi, yaitu **binaries**, yang dijalankan langsung oleh CPU, serta **scripts**, yang memerlukan interpreter seperti **Bash** atau **Python**. Dalam script, penggunaan **shebang (`#!`)** pada baris pertama (misalnya `#!/bin/bash`) berfungsi untuk menentukan interpreter yang akan digunakan.
`#!/uer/bin/perl`

### Setuid dan Setgid Bits
Bit dengan nilai oktal 4000 dan 2000 masing-masing adalah setuid dan setgid. Jika setuid diaktifkan pada sebuah file, maka saat file tersebut dieksekusi, prosesnya akan berjalan dengan hak akses pemilik file, bukan pengguna yang menjalankannya. Hal yang sama berlaku untuk setgid, tetapi perubahan terjadi pada kepemilikan grup file tersebut.
Jika setgid diaktifkan pada sebuah direktori, semua file yang dibuat di dalam direktori tersebut akan mewarisi kepemilikan grup dari direktori, bukan grup default pengguna yang membuat file. Hal ini mempermudah kolaborasi antar user dalam berbagi file dengan grup yang sama.

### Sticky Bit
Bit dengan nilai oktal 1000 disebut sticky bit. Jika diterapkan pada sebuah direktori, bit ini mencegah user yang lain menghapus atau mengganti nama file yang bukan miliknya, meskipun memiliki izin write pada direktori tersebut. Fitur ini sangat berguna untuk direktori yang digunakan bersama, seperti /tmp, agar setiap pengguna hanya dapat mengelola file miliknya sendiri tanpa mengganggu file user lain.

### ls: list dan inspect File
Perintah **ls** digunakan untuk menampilkan daftar file dan direktori. Selain itu, juga bisa digunakan untuk memeriksa atribut file dan direktori.  
Opsi **-l** pada **ls** show information dalam format panjang yang mencakup mode file, jumlah hard link, pemilik file, grup file, ukuran file dalam byte, waktu modifikasi terakhir, dan nama file.  
Setiap direktori memiliki setidaknya dua hard link: satu mengarah ke direktori itu sendiri (**"."**) dan satu lagi mengarah ke parent directory (**".."**).

### chmod: change permissions
Perintah **chmod** dipakai untuk mengubah mode atau izin akses pada sebuah file. Perubahan dapat dilakukan menggunakan **notasi oktal** atau **notasi simbolik**.

![Pict4](https://github.com/ferryastika/unix-and-linux-sysadmin-notes/blob/main/the-filesystem/data/permissions-encoding.png)

### chown: Mengubah Kepemilikan File
Perintah **chown** untuk mengubah owner dan grup dari sebuah file atau direktori. Opsi **-R** memungkinkan perubahan dilakukan secara **rekursif**, yaitu pada file dan subdirektori di dalamnya. 

### chgrp: Mengubah Grup File

Perintah **chgrp** untuk mengubah grup kepemilikan sebuah file atau direktori. Opsi **-R** memungkinkan perubahan dilakukan secara **rekursif**, yaitu pada semua file dan subdirektori di dalamnya. 

### umask: Menetapkan Izin Default
Perintah **umask** dipakai untuk mengatur izin default untuk file dan direktori baru. **umask** adalah bit mask yang dikurangkan dari izin default sistem untuk menentukan izin akhir yang diberikan pada file atau direktori yang baru dibuat. 

# Access Control List
Model izin tradisional UNIX memang sederhana dan efektif, tetapi memiliki keterbatasan. Misalnya, sulit memberikan kepemilikan file kepada beberapa pengguna sekaligus, atau mengatur izin berbeda untuk kelompok pengguna tertentu pada file yang berbeda.
Access Control Lists (ACLs) adalah solusi untuk memperluas model izin tradisional Unix. ACL memungkinkan satu file memiliki beberapa pemilik dan memungkinkan kelompok user punya izin berbeda pada file yang berbeda. 

`$ getfacl /etc/passwd`

`$ setfacl -m u:abdou:rw /etc/passwd`

2 jenis ACL ada **ACL POSIX** dan **ACL NFSv4**
ACL POSIX versi tradisional dan didukung sebagian besar operating system, mirip UNIX (Linux, FreeBSD, Solaris)
ACL NFSv4 jenis yang lebih baru, seperti NFS dan SMB dan didukung oleh beberapa sistem operasi, mirip UNIX (Linux dan FreeBSD). Mirip dengan ACL POSIX tapi ada fitur tambahan yang punya ACL default dipakai untuk mengatur ACL file dan direktori baru. 
