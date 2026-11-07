# Perbaikan error Closure pada middleware

Penyebab: closure dimasukkan langsung ke `Route::middleware()`. Laravel mengharapkan nama middleware atau class middleware, lalu mencoba mengubah closure menjadi string.

Perbaikan:
- Menambahkan `App\\Http\\Middleware\\EnsureAdmin`.
- Mendaftarkan alias `admin` di `bootstrap/app.php` (Laravel 12).
- Mengganti middleware inline pada `routes/web.php` menjadi `['auth', 'admin']`.

Setelah mengganti file, jalankan:
```bash
php artisan optimize:clear
php artisan route:list
```
