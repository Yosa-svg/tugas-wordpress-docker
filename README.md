Dokumentasi Tugas Docker Compose: WordPress, MySQL, dan Redis

Nama:	Yosafat Goradipa Bakti
NIM: 	A11.2023.15079


 
1.Langkah-Langkah Menjalankan Stack 

Langkah 1: Persiapan Lingkungan (Environment)
 Pastikan aplikasi Docker Desktop sudah terinstal di komputermu.  Buka aplikasi Docker Desktop dan tunggu hingga status engine-nya menunjukkan indikator hijau (Engine Running). 
 
Langkah 2: Menyiapkan Direktori Proyek 
Buka program Terminal, Command Prompt (CMD), atau PowerShell. Arahkan direktori terminal ke dalam folder tempat file konfigurasi proyek ini berada menggunakan perintah `cd`. *(Contoh: `cd G:\ tugas-wordpress-docker`).  Pastikan file `docker-compose.yml` berada di dalam folder tersebut. 

Langkah 3: Mengeksekusi Build & Run (Deployment)  
Di dalam terminal yang sudah mengarah ke folder proyek, ketikkan perintah berikut: 
```bash 
docker compose up -d

Langkah 4: Verifikasi Status Container . Setelah proses selesai, pastikan ketiga container berhasil menyala dengan mengetikkan perintah:
Bash
docker ps
Jika berhasil, kolom STATUS pada ketiga container (WordPress, MySQL, Redis) akan menunjukkan keterangan Up.

Langkah 5: Mengakses Web dan Instalasi
Buka web browser (Chrome/Edge/Firefox).
Ketikkan alamat http://localhost:8000 di address bar.
Halaman instalasi awal WordPress akan muncul, dan stack sudah siap digunakan secara penuh!

Redis CLI Ping Test: 
Buka Terminal / PowerShell di komputer.
masukan perintah ini untuk masuk ke dalam container Redis:
Bash
docker exec -it tugas-wordpress-docker-redis-1 redis-cli


 
2. Lampiran Screenshot 
Screenshot WordPress installation page 
<img width="1637" height="943" alt="Screenshot 2026-03-12 133454" src="https://github.com/user-attachments/assets/4de37415-da3c-4f14-b570-1cd4957e0685" />

Screenshot WordPress dashboard  
<img width="1917" height="943" alt="Screenshot 2026-03-12 134034" src="https://github.com/user-attachments/assets/0fa958fb-20ad-44ef-afac-18e3b54a2c2d" />

Screenshot docker ps menunjukkan 3 containers running
 <img width="1902" height="909" alt="Screenshot 2026-03-12 134402" src="https://github.com/user-attachments/assets/72ea8d9b-8abf-42dd-a35d-c4ec83b765b5" />


Screenshot Redis CLI ping test
 <img width="1260" height="459" alt="Screenshot 2026-03-12 193753" src="https://github.com/user-attachments/assets/5f502a46-0fb3-49fd-9bba-66c7a816a20e" />


1. Kenapa perlu volume untuk MySQL? Container di Docker bersifat ephemeral (sementara). Jika sebuah container dimatikan, dihapus, atau crash, maka semua data di dalamnya akan hilang. Kita menggunakan Volume pada MySQL untuk memetakan (menyambungkan) folder penyimpanan database di dalam container (contoh: /var/lib/mysql) ke folder fisik di laptop kita. Dengan begitu, data artikel, user, dan pengaturan WordPress akan tetap aman dan persisten (persistent) meskipun container Docker-nya kita hapus dan buat ulang.
2. Apa fungsi depends_on? depends_on berfungsi untuk mengatur urutan prioritas nyala (startup order) antar container. Pada kasus ini, container WordPress memiliki depends_on ke container MySQL dan Redis. Artinya, Docker akan memastikan MySQL dan Redis dinyalakan terlebih dahulu sebelum menyalakan WordPress. Ini mencegah WordPress mengalami error (crash) karena mencoba menghubungi database yang belum siap (belum booting sepenuhnya).
3. Bagaimana cara WordPress container connect ke MySQL? Ketika dijalankan menggunakan Docker Compose, semua container secara otomatis dimasukkan ke dalam satu jaringan virtual yang sama (internal bridge network). WordPress menyambung ke MySQL dengan menggunakan nama service / container MySQL tersebut (misalnya db atau mysql) sebagai alamat Host-nya, bukan menggunakan alamat IP. Kredensial login-nya dikirimkan menggunakan Environment Variables (seperti WORDPRESS_DB_USER, WORDPRESS_DB_PASSWORD) yang disetel di file docker-compose.yml.
4. Apa keuntungan pakai Redis untuk WordPress? Redis berfungsi sebagai Object Cache yang menyimpan data di dalam memori utama (RAM) sehingga aksesnya sangat cepat. Tanpa Redis, setiap kali ada orang yang membuka web, WordPress harus melakukan query (bertanya) ke database MySQL, yang mana proses ini berat dan lambat jika jumlah kunjungannya banyak. Dengan Redis, hasil query yang sering diminta akan disimpan sementara. Jadi, ketika ada pengunjung berikutnya, WordPress langsung mengambil datanya dari memori Redis tanpa membebani database MySQL. Ini membuat loading web menjadi jauh lebih cepat dan server menjadi lebih ringan.
