# Reflection Module 6

## Commit 1 Reflection Notes

Pada milestone ini, server sudah bisa menerima koneksi TCP dan membaca request dari browser. Fungsi `handle_connection` menggunakan `BufReader` untuk membaca request HTTP baris per baris hingga menemukan baris kosong yang menandakan akhir dari HTTP header. Setiap kali browser mengakses server, ia mengirimkan informasi seperti method (GET), path, versi HTTP, dan berbagai header lainnya. Yang menarik, browser kadang mengirim beberapa koneksi sekaligus sehingga "Connection established!" bisa muncul lebih dari sekali. Dari sini terlihat bagaimana komunikasi antara browser dan server bekerja di level yang paling dasar.

