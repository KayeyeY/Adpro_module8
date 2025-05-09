# Reflection

## No. 1
### Unary RPC: Klien mengirim satu permintaan, lalu menerima satu respons dari server.

Kelebihan: Paling sederhana dan mudah diimplementasikan.

Kekurangan: Kurang efisien jika digunakan untuk transfer data dalam jumlah besar atau berkelanjutan.

Contoh: Mengambil data tertentu (misalnya detail user), login/autentikasi.

### Server Streaming RPC: Klien mengirim satu permintaan, dan server membalas dengan banyak data secara bertahap (stream).

Kelebihan: Efisien untuk mengirim data dalam jumlah besar atau pembaruan berkelanjutan.

Kekurangan: Klien hanya bisa mengirim permintaan satu kali di awal, tidak bisa mengubahnya selama proses.

Contoh:Streaming data harga saham, update cuaca, atau video/live feed dari server.

### Client Streaming RPC: Klien mengirim beberapa permintaan (stream), dan server merespons satu kali di akhir.

Kelebihan: Cocok untuk mengirim banyak data dari klien ke server dalam sesi tunggal.

Kekurangan: Server harus menunggu seluruh data selesai dikirim sebelum merespons.

Contoh: Upload file besar secara bertahap, pengiriman data sensor dalam batch.

### Bidirectional Streaming RPC: Klien dan server bisa saling mengirim dan menerima data secara bersamaan dalam bentuk stream.

Kelebihan: Mendukung komunikasi interaktif dan real-time dua arah.

Kekurangan: Implementasinya lebih rumit karena perlu mengelola dua stream sekaligus.

Contoh: Aplikasi chatting, dashboard analitik real-time, atau game multiplayer interaktif.

## No. 2
Dalam mengembangkan layanan gRPC di Rust, penting untuk
memperhatikan keamanan melalui autentikasi, otorisasi, dan enkripsi data. 
Autentikasi dapat dilakukan dengan TLS atau token (seperti JWT), sementara otorisasi mengatur hak akses pengguna berdasarkan peran atau identitas. Enkripsi data selama transmisi dapat dijamin dengan mengaktifkan TLS. 
Selain itu, validasi input di sisi server sangat penting untuk mencegah eksploitasi, serta perlu diterapkan rate limiting, batas ukuran request, dan timeout untuk menghindari serangan DoS. Logging yang aman dan audit trail juga membantu pemantauan serta investigasi insiden keamanan.

## No. 3
Tantangan utama dalam implementasi komunikasi client-server adalah memastikan pesan yang dikirim tidak saling tumpang tindih atau keluar dari sinkronisasi, sehingga tidak terjadi pencampuran atau kehilangan data saat pengiriman. Pengelolaan buffer juga perlu dipertimbangkan secara hati-hati: buffer yang terlalu besar dapat menyebabkan keterlambatan pesan, sementara buffer yang terlalu kecil dapat meningkatkan beban kerja server. Selain itu, kita harus mampu menangani banyak koneksi client secara bersamaan, terutama dalam aplikasi seperti obrolan yang ramai, yang dapat menekan kapasitas server. Terakhir, sistem perlu dirancang untuk tangguh terhadap pemutusan koneksi mendadak, guna mencegah risiko keamanan seperti penyadapan atau manipulasi data selama transmisi.

## No. 4
Kelebihan:

ReceiverStream menawarkan fleksibilitas tinggi karena mampu mengubah ```tokio::sync::mpsc::Receiver``` menjadi stream yang kompatibel dengan gRPC. Cara penggunaannya pun cukup sederhana dan mudah dipahami. Selain itu, ReceiverStream dapat bekerja dengan baik bersama berbagai fitur dan tools lain dari tokio, menjadikannya pilihan yang praktis dalam pengembangan aplikasi asynchronous.

Kekurangan:

Namun, penggunaan ReceiverStream dalam konteks operasi blocking bisa berdampak negatif pada performa aplikasi secara keseluruhan. ReceiverStream juga kurang optimal untuk kebutuhan dengan lalu lintas data yang tinggi (high throughput). Meskipun konsepnya sederhana, pengguna baru yang belum familiar dengan tokio mungkin akan merasa kesulitan dalam implementasinya.

## No. 5
Kita bisa memisahkan antara definisi protokol gRPC dan implementasi layanannya dengan memanfaatkan protobuf. Melalui file .proto, kita mendefinisikan interface yang nantinya dapat diimplementasikan oleh struct atau class di Rust, sehingga mempermudah pengembangan dan perluasan kode. Untuk meningkatkan modularitas, kode juga bisa diorganisasi ke dalam module terpisah, seperti memisahkan bagian service dan client. Pendekatan ini membuat proses pembaruan dan perbaikan bug menjadi lebih efisien dan terstruktur.

## No. 6
### Validasi data dari client
Pastikan data yang diterima valid, seperti memeriksa apakah jumlah pembayaran tidak negatif, dan tangani error yang muncul.

### Implementasi logika pembayaran
Tambahkan logika untuk menghitung total harga, mengurangi stok barang, dan proses bisnis lainnya yang relevan.

### Streaming data secara bertahap
Jika pengiriman data harus berurutan atau bersifat terus-menerus, gunakan gRPC streaming untuk mengatur alur data masuk dan keluar secara bertahap.

## No. 7
gRPC memungkinkan komunikasi antar layanan yang ditulis dalam bahasa pemrograman yang berbeda dengan menggunakan protobuf. Melalui file proto, client dan server memiliki pemahaman yang sama tentang fungsi yang dapat dipanggil, sehingga mempermudah interaksi antar platform yang berbeda. Sebagai contoh, layanan yang dibuat dengan Rust di server dapat diakses oleh klien yang menggunakan JavaScript di aplikasi web, dan keduanya dapat berkomunikasi secara lancar melalui protokol gRPC tanpa perlu mengimplementasikan detail komunikasi yang rumit.

## No. 8
### Kelebihan:

- HTTP/2 mendukung multiplexing, yang memungkinkan banyak permintaan dan respons dikirim secara bersamaan melalui satu koneksi TCP, sehingga mengurangi latensi dan meningkatkan efisiensi sumber daya.

- HTTP/2 memungkinkan streaming data dua arah, menjaga koneksi tetap terbuka antara client dan server, yang sangat berguna untuk transfer data kontinu seperti streaming video.

- HTTP/2 menggunakan kompresi header, mengurangi ukuran header permintaan dan respons, yang mengurangi overhead jaringan dan mempercepat waktu muat halaman.

- HTTP/2 mendukung penggunaan Protocol Buffers untuk serialisasi data, menawarkan format data yang lebih efisien dan dapat dipahami oleh berbagai bahasa pemrograman.

### Kekurangan:

- Implementasi HTTP/2 lebih kompleks dibandingkan dengan HTTP/1.1, yang mungkin memerlukan dukungan server yang lebih canggih.

- Tidak semua server mendukung HTTP/2 sepenuhnya.

- Beberapa perangkat atau browser lama mungkin tidak mendukung HTTP/2 secara penuh.

- HTTP/2 bisa dianggap berlebihan untuk aplikasi yang sederhana karena menyediakan fitur yang mungkin tidak dibutuhkan.

## No. 9
Dengan gRPC, baik client maupun server dapat mengirim pesan secara asinkron, memungkinkan respons yang lebih cepat. Pada aplikasi chat, misalnya, gRPC memungkinkan pengiriman dan penerimaan pesan secara langsung tanpa harus menunggu request atau respons tertentu. Oleh karena itu, gRPC lebih unggul dalam situasi yang membutuhkan komunikasi real-time, sementara REST API lebih cocok untuk kasus dengan model request-response sederhana di mana komunikasi real-time tidak menjadi prioritas utama.

## No. 10
gRPC menggunakan skema yang telah didefinisikan sebelumnya dalam file proto, yang mengharuskan client dan server untuk mengikuti format data yang telah ditentukan dalam skema tersebut. Pendekatan ini memastikan konsistensi format data dan validasi yang lebih ketat. Di sisi lain, REST API menggunakan JSON yang lebih fleksibel dan tidak memerlukan skema yang ketat. Fleksibilitas ini memungkinkan perubahan pada struktur dan tipe data tanpa perlu memperbarui skema secara eksplisit, namun hal ini dapat menyebabkan ketidaksesuaian format data antara client dan server.