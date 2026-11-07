# Catatan Perbaikan Cirangkong

## Pembaruan utama
- Direktori anggota tidak lagi tersedia untuk publik. Halaman `/anggota` mewajibkan login dan hanya menampilkan data milik akun yang sedang masuk.
- Pengguna membuat akun melalui `/daftar-anggota`, kemudian mengajukan keanggotaan.
- Pengajuan baru berstatus `pending`. Admin harus menyetujui pengajuan sebelum KTA dapat dibuka.
- Endpoint KTA memeriksa status `approved` dan kepemilikan `user_id` di backend. Mengganti ID URL tidak memberi akses ke KTA anggota lain.
- Foto anggota baru disimpan pada disk `local` (bukan direktori publik) dan disajikan melalui endpoint terautentikasi.
- Akses dashboard dan CRUD admin dibatasi untuk akun dengan `role=admin`.
- Memperbaiki method Infak yang hilang, route/view publik, serta konsistensi data tanggal infak dan kritik/saran.

## Langkah pemasangan
1. Cadangkan database dan file upload sebelum deploy.
2. Salin `.env.example` menjadi `.env`, lalu atur koneksi database, `APP_KEY`, `APP_URL`, dan kredensial admin:
   - `ADMIN_NAME`
   - `ADMIN_EMAIL`
   - `ADMIN_PASSWORD` (gunakan password kuat dan unik)
3. Jalankan `composer install` dan `php artisan key:generate` bila instalasi baru.
4. Jalankan `php artisan migrate --force`.
5. Jalankan `php artisan db:seed --force` hanya jika ingin membuat/memperbarui akun admin berdasarkan `ADMIN_EMAIL` dan `ADMIN_PASSWORD`.
6. Jalankan `php artisan storage:link` hanya untuk file publik aplikasi; foto anggota baru disimpan di disk privat `local`.

## Catatan data lama
- Migrasi memberi role `admin` kepada akun user yang sudah ada sebelum fitur role ditambahkan, karena instalasi sebelumnya hanya memiliki login admin. Akun anggota baru otomatis mendapat role `member`.
- Data anggota lama yang belum mempunyai `user_id` tidak otomatis dipasangkan ke akun mana pun. Admin perlu membuat akun anggota dan menautkan data lama secara manual sebelum pemilik dapat mengakses KTA.
- Foto anggota lama yang sudah berada di `storage/app/public` perlu dipindahkan secara manual ke `storage/app/private/foto-peserta` dan diverifikasi sebelum menghapus salinan publiknya.

## Pemeriksaan
Semua file PHP yang disertakan telah lolos `php -l`. Tes integrasi Laravel belum dapat dijalankan pada lingkungan pemeriksaan ini karena folder `vendor` sengaja tidak disertakan dalam arsip; jalankan `php artisan test` setelah dependensi dipasang di komputer/server Anda.
