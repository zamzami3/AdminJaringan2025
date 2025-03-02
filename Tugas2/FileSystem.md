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

