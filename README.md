# Reflection Module 6

## Commit 1 Reflection Notes

Pada milestone ini, server sudah bisa menerima koneksi TCP dan membaca request dari browser. Fungsi `handle_connection` menggunakan `BufReader` untuk membaca request HTTP baris per baris hingga menemukan baris kosong yang menandakan akhir dari HTTP header. Setiap kali browser mengakses server, ia mengirimkan informasi seperti method (GET), path, versi HTTP, dan berbagai header lainnya. Yang menarik, browser kadang mengirim beberapa koneksi sekaligus sehingga "Connection established!" bisa muncul lebih dari sekali. Dari sini terlihat bagaimana komunikasi antara browser dan server bekerja di level yang paling dasar.

## Commit 2 Reflection Notes

Pada milestone ini, server sudah bisa mengirimkan halaman HTML yang bisa dirender oleh browser. HTTP response memiliki struktur tertentu yang diawali dengan status line seperti `HTTP/1.1 200 OK`, diikuti header seperti `Content-Length`, lalu baris kosong, baru kemudian isi kontennya. `Content-Length` penting agar browser tahu berapa byte yang harus dibaca. Fungsi `fs::read_to_string` digunakan untuk membaca isi file `hello.html` menjadi String yang kemudian dikirim sebagai response. Pada dasarnya web server hanyalah program yang menerima teks request dan membalas dengan teks response mengikuti aturan protokol HTTP.

**Screenshot**

![Commit 2 screen capture](assets/images/commit2.png)

## Commit 3 Reflection Notes

Pada milestone ini, server sudah bisa membedakan request yang valid dan tidak valid. Server membaca baris pertama HTTP request untuk mengetahui URL yang diminta, lalu memutuskan file mana yang harus dikirim sebagai response. Refactoring dilakukan dengan memisahkan bagian penentuan status_line dan filename ke dalam blok if/else tersendiri, sehingga bagian pengiriman response (membaca file, menghitung panjang, menulis ke stream) hanya ditulis sekali dan tidak duplikat. Tanpa refactoring ini, setiap kondisi harus menulis ulang seluruh logika pengiriman response yang membuat kode lebih panjang dan susah dimaintain. Dengan pemisahan ini kode menjadi lebih bersih, lebih mudah dibaca, dan jika ada perubahan pada cara pengiriman response cukup diubah di satu tempat saja.

**Screenshot**

![Commit 3 screen capture](assets/images/commit3.png)

## Commit 4 Reflection Notes

Pada milestone ini, ditambahkan route `/sleep` yang sengaja ditunda 10 detik untuk mensimulasikan request yang lambat. Ketika membuka dua tab browser, satu mengakses `/sleep` dan satu mengakses `/`, tab kedua ikut menunggu 10 detik meskipun seharusnya langsung selesai. Hal ini terjadi karena server berjalan secara single-threaded, di mana `handle_connection` dipanggil langsung di main thread tanpa `thread::spawn` sehingga setiap request harus antri satu per satu. Ini adalah demonstrasi nyata kelemahan single-threaded server, bayangkan jika ada ratusan user mengakses bersamaan. Dari sini terlihat jelas mengapa concurrency sangat penting untuk membuat server yang responsif.

## Commit 5 Reflection Notes

Pada milestone ini, server sudah bisa menangani banyak request secara bersamaan menggunakan ThreadPool. ThreadPool bekerja dengan membuat sejumlah worker thread di awal yang siap menerima pekerjaan, sehingga tidak perlu membuat thread baru setiap kali ada request dan lebih efisien dari segi resource. Setiap request dikirim ke worker melalui channel (message passing), dan worker yang sedang tidak sibuk akan mengambil dan mengerjakannya. Jumlah thread dibatasi (dalam kasus ini 4) agar server tidak kehabisan resource jika ada terlalu banyak request sekaligus, yang juga mencegah serangan DoS. Setelah milestone ini, ketika ditest dengan dua tab browser seperti sebelumnya, tab kedua langsung mendapat response tanpa perlu menunggu tab pertama selesai.

## Commit Bonus Reflection Notes

Pada bonus ini, dibuat fungsi build sebagai alternatif dari new untuk membuat ThreadPool. Perbedaan utamanya adalah new langsung melakukan panic jika menerima size 0, sedangkan build mengembalikan Result sehingga pemanggil bisa memutuskan sendiri cara menangani error tersebut. Pendekatan build lebih sesuai dengan filosofi Rust yang mendorong error handling yang eksplisit daripada membiarkan program crash begitu saja. Dalam konteks production, tentu lebih baik menangani error dengan baik daripada membuat seluruh program berhenti mendadak. Dari sini terlihat perbedaan antara panic yang cocok untuk kondisi yang benar-benar tidak terduga dan Result yang lebih cocok untuk error yang bisa diantisipasi dan ditangani.