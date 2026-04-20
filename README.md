# Reflection Module 6

## Commit 1 Reflection Notes

Pada milestone ini, server sudah bisa menerima koneksi TCP dan membaca request dari browser. Fungsi `handle_connection` menggunakan `BufReader` untuk membaca request HTTP baris per baris hingga menemukan baris kosong yang menandakan akhir dari HTTP header. Setiap kali browser mengakses server, ia mengirimkan informasi seperti method (GET), path, versi HTTP, dan berbagai header lainnya. Yang menarik, browser kadang mengirim beberapa koneksi sekaligus sehingga "Connection established!" bisa muncul lebih dari sekali. Dari sini terlihat bagaimana komunikasi antara browser dan server bekerja di level yang paling dasar.

## Commit 2 Reflection Notes

Pada milestone ini, server sudah bisa mengirimkan halaman HTML yang bisa dirender oleh browser. HTTP response memiliki struktur tertentu yang diawali dengan status line seperti `HTTP/1.1 200 OK`, diikuti header seperti `Content-Length`, lalu baris kosong, baru kemudian isi kontennya. `Content-Length` penting agar browser tahu berapa byte yang harus dibaca. Fungsi `fs::read_to_string` digunakan untuk membaca isi file `hello.html` menjadi String yang kemudian dikirim sebagai response. Pada dasarnya web server hanyalah program yang menerima teks request dan membalas dengan teks response mengikuti aturan protokol HTTP.

**Screenshot**

![Commit 2 screen capture](assets/images/commit2.png)