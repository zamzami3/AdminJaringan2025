#Proses Kontrol

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
Tiap proses dikaitkan dengan parent process. 
