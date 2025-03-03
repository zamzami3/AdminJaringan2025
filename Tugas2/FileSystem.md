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

