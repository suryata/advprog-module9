## REFLECTION

#### 1. What are the key differences between unary, server streaming, and bi-directional streaming RPC (Remote Procedure Call) methods, and in what scenarios would each be most suitable?

Perbedaan utama antara metode RPC unary, server streaming, dan bi-directional streaming adalah sebagai berikut:
<br/>
1. Unary RPC: Metode ini melibatkan satu permintaan oleh klien dan satu respons oleh server. Cocok untuk operasi sederhana dan transaksional di mana hanya diperlukan satu pertukaran data (seperti mengirimkan form, mengambil data).

2. Server Streaming RPC: Klien mengirim satu permintaan ke server dan menerima aliran respons. Sangat berguna untuk kasus seperti pengiriman data secara bertahap atau pengiriman data yang besar dari server ke klien (seperti feed data pasar saham).

3. Bi-directional Streaming RPC: Klien dan server dapat saling mengirim aliran data secara bersamaan. Metode ini cocok untuk aplikasi yang memerlukan komunikasi dua arah real-time seperti chat atau pembaruan status langsung (seperti aplikasi chat, video call).

### 2. What are the potential security considerations involved in implementing a gRPC service in Rust, particularly regarding authentication, authorization, and data encryption?

Dalam mengimplementasikan layanan gRPC di Rust, penting untuk memverifikasi identitas pengguna atau layanan, yang bisa dilakukan melalui token seperti JWT atau melalui integrasi dengan sistem OAuth2. Otorisasi harus dikelola untuk mengontrol akses ke resource yang ada berdasarkan peran atau aturan yang telah ditetapkan. Hal ini bisa diimplementasikan menggunakan middleware. Selain itu untuk enkripsi data misalnya melalui TLS sebaiknya digunakan untuk mengamankan komunikasi antara klien dan server, hal ini berguna untuk mencegah penyadapan data, sehingga menjaga kerahasiaan informasi yang ditransmisikan.

### 3. What are the potential challenges or issues that may arise when handling bidirectional streaming in Rust gRPC, especially in scenarios like chat applications?

Dalam mengimplementasikan streaming dua arah dengan gRPC di Rust, khususnya untuk aplikasi chat, beberapa tantangan yang mungkin muncul adalah mengenai pengelolaan koneksi, dan sinkronisasi pesan. Mengelola banyak koneksi secara bersamaan dan memastikan koneksi tetap hidup tanpa membebani server bisa menjadi kompleks. Selain itu, sistem harus mampu menangani aliran data masuk dan keluar secara efisien untuk mencegah kelebihan beban (back pressure). Selain itu, memastikan pesan dikirim dan diterima dalam urutan yang tepat serta sinkron antar pengguna juga menjadi tantangan penting dalam real-time chat applications.

### 4. What are the advantages and disadvantages of using the tokio_stream::wrappers::ReceiverStream for streaming responses in Rust gRPC services?

Keuntungan:
- Integrasi yang baik dengan ekosistem Tokio.
- Memfasilitasi pengelolaan data secara asynchronous.

Kekurangan:
- Menambah kompleksitas dalam pengelolaan state dan error handling.
- Potensi overhead performa, terutama di skenario dengan traffic tinggi atau data besar.

### 5. In what ways could the Rust gRPC code be structured to facilitate code reuse and modularity, promoting maintainability and extensibility over time?

Untuk meningkatkan penggunaan ulang kode dan modularitas dalam pengembangan gRPC dalam Rust, beberapa teknik bisa diaplikasikan. Pertama, menggunakan traits dan generics dapat memudahkan pembuatan komponen yang fleksibel dan dapat dikonfigurasi ulang, sehingga memfasilitasi integrasi dan ekstensi fungsionalitas. Kedua, kode dapat dibagi menjadi modul atau crate yang berbeda berdasarkan fungsionalitasnya untuk memudahkan pengelolaan dan penggunaan ulang kode. Ketiga, mengadopsi penggunaan library eksternal untuk tugas-tugas umum seperti serialisasi dan logging untuk dapat meningkatkan konsistensi dan mengurangi duplikasi kode. Dengan pendekatan ini, kode gRPC Rust akan lebih modular, mudah dipelihara, dan dapat diperluas dengan efisien.

### 6. In the MyPaymentService implementation, what additional steps might be necessary to handle more complex payment processing logic?

Dapat ditingkatkan dengan menambahkan validasi data yang masuk di PaymentRequest untuk memastikan keamanan dan kelengkapan data sebelum diproses. Selain itu, implementasi dengan server streaming lebih baik digunakan daripada menggunakan unary untuk mendukung proses transfer data dalam jumlah yang lebih besar secara bersamaan.

### 7. What impact does the adoption of gRPC as a communication protocol have on the overall architecture and design of distributed systems, particularly in terms of interoperability with other technologies and platforms?

Adopsi gRPC dalam sistem terdistribusi memungkinkan interaksi antarmodul yang lebih efisien karena protokol ini mengotomatisasi pemanggilan metode antarsistem melalui definisi di file proto. Ini menghilangkan kebutuhan untuk konfigurasi HTTP method secara manual. gRPC, yang berbasis pada HTTP/2, mendukung komunikasi yang efisien dan cepat antara layanan, dengan kemampuan streaming yang baik dan overhead rendah. Hal ini memungkinkan sistem yang dibangun dengan gRPC untuk berinteraksi lebih lancar dan efektif.

### 8. What are the advantages and disadvantages of using HTTP/2, the underlying protocol for gRPC, compared to HTTP/1.1 or HTTP/1.1 with WebSocket for REST APIs?

HTTP/2, sebagai protokol dasar untuk gRPC, memiliki beberapa keunggulan dibandingkan dengan HTTP/1.1 atau HTTP/1.1 dengan WebSocket, terutama untuk REST APIs. Kelebihan utama HTTP/2 adalah efisiensinya dalam mengelola multiple requests dan responses melalui single connection yang dikenal dengan multiplexing yang berfungsi untuk mengurangi latency dan meningkatkan performa aplikasi. HTTP/2 juga mendukung prioritas request dan server push, yang memungkinkan server untuk mengirim sumber daya sebelum diminta oleh client, sehingga mempercepat waktu pemuatan halaman.

Namun, terdapat juga beberapa kekurangan. Implementasi HTTP/2 cenderung lebih kompleks dan membutuhkan enkripsi SSL/TLS, yang bisa menambah overhead administrasi dan biaya. Selain itu, meskipun WebSocket di HTTP/1.1 memungkinkan komunikasi dua arah dan real-time yang efektif, penggunaan ini bisa lebih sederhana dalam setup tertentu dibandingkan dengan setup di HTTP/2 yang lebih baru dan mungkin kurang didukung secara universal. Ini berarti, dalam beberapa kasus, migrasi ke HTTP/2 mungkin memerlukan pertimbangan terkait dengan kompatibilitas dan biaya overhead.

### 9. How does the request-response model of REST APIs contrast with the bidirectional streaming capabilities of gRPC in terms of real-time communication and responsiveness?

Model permintaan-respons dari REST APIs kontras dengan kemampuan streaming dua arah dari gRPC terutama dalam hal komunikasi real-time dan responsivitas. REST APIs, yang beroperasi dalam model HTTP/1.1, umumnya mengikuti pola permintaan-respons yang sederhana di mana setiap permintaan mendapatkan satu respons, membuatnya kurang ideal untuk kebutuhan komunikasi real-time karena setiap interaksi membutuhkan koneksi baru. Sebaliknya, gRPC menggunakan HTTP/2 yang mendukung streaming dua arah, hal ini memungkinkan klien dan server untuk mengirim dan menerima data secara kontinu dalam satu koneksi yang terbuka. Ini meningkatkan efisiensi dan kecepatan komunikasi, dan hal ini sangat cocok untuk aplikasi yang memerlukan pertukaran data real-time seperti aplikasi chat atau monitor status.

### 10. What are the implications of the schema-based approach of gRPC, using Protocol Buffers, compared to the more flexible, schema-less nature of JSON in REST API payloads?

Pendekatan berbasis skema dari gRPC yang menggunakan Protocol Buffers memiliki dampak yang cukup signifikan dibandingkan dengan sifat yang lebih fleksibel dan tanpa skema dari JSON dalam payload REST API. Protocol Buffers, sebagai sistem serialisasi yang memerlukan definisi skema upfront, mendukung efisiensi dalam pengiriman data. Skema ini menjamin bahwa struktur data tetap konsisten, dapat divalidasi, dan kompatibel di antara berbagai bagian sistem untuk mengurangi risiko kesalahan dalam interpretasi data. Ini sangat berguna dalam pengembangan sistem besar dan kompleks di mana kontrak antara server dan klien perlu jelas dan tidak berubah-ubah. Di sisi lain, JSON, yang tidak memerlukan skema, memberikan fleksibilitas lebih dalam mengelola data. Fleksibilitas ini memudahkan pengembangan agar menjadi lebih cepat, tetapi dapat meningkatkan kemungkinan kesalahan runtime dan kesulitan dalam memvalidasi data, karena struktur payload bisa berubah tanpa pemberitahuan.