# Tutorial Modul 8 Adpro : High Level Networking

## Muhammad Radhiya Arshq - 2306275885

### Refleksi

1. What are the key differences between unary, server streaming, and bi-directional streaming RPC (Remote Procedure Call) methods, and in what scenarios would each be most suitable?

    Perbedaan antara unary, server streaming dan bi-directional streaming terdapat pada jumlah respons yang dikirim dan diterima kedua client dan server. 

    * Unary RPC : Client mengirim satu request dan menerima satu response dari server, ini cocok untuk operasi sederhana seperti Payment Service pada modul kali ini yang prosesnya sekali jalan, cepat, dan mudah di handle. Implementasi lain mungkin dalam operasi sederhana seperti mengambil detail user atau menyimpan data.
    * Server Streaming RPC : Client mengirim satu request lalu server mengirim banyak response secara berurutan. Ini cocok untuk mengirim data bertahap seperti live update atau daftar item besar.
    * Bi-directional Streaming RPC : Client dan server bisa saling kirim banyak pesan secara independen dan simultan. Ini cocok untuk komunikasi real-time seperti chatbot yang dibuat pada modul ini, monitoring sistem, atau kolaborasi langsung.

2. What are the potential security considerations involved in implementing a gRPC service in Rust, particularly regarding authentication, authorization, and data encryption?

    Mengimplementasi servis gRPC dengan rust tentu melibatkan beberapa konsiderasi keamanan di bagian authentikasi, otorisasi, dan data enkripsi. Dalam otorisasi/autentikasi, setiap fragmen data yang dikirim oleh gRPC memerlukan validasi otorisasi/autentikasi untuk memastikan keamanannya, sedangkan untuk enkripsi data, setiap fragmen data yang dikirim oleh server dan klien perlu dienkripsi secara terpisah untuk memastikan privasi data. Ini berbeda dengan REST yang memerlukan validasi satu kali untuk tiap satu request, gRPC memerlukan otorisasi/autentikasi/enkripsi berulang untuk satu request.

3. What are the potential challenges or issues that may arise when handling bidirectional streaming in Rust gRPC, especially in scenarios like chat applications?

    Dalam menangani bidirectional streaming di Rust menggunakan gRPC (misalnya dalam aplikasi chat real-time), ada beberapa tantangan yang perlu diperhatikan seperti menjaga alur baca/tulis antara client dan server agar tidak saling blokir atau race condition, mengatur task async yang efisien untuk menangani banyak koneksi sekaligus dan menangani kesalahan pada salah satu sisi (client/server) tanpa menghentikan keseluruhan stream.

4. What are the advantages and disadvantages of using the tokio_stream::wrappers::ReceiverStream for streaming responses in Rust gRPC services?

    Keuntungan : 

    * Integrasi mudah dengan gRPC menggunakan tonic
    * Sinkronisasi asinkron
    * Fleksibel dan modular
    * Concurrency-friendly
    
    Kerugian :

    * Potensi kehilangan data jika channel penuh dan send gagal
    * Overhead Tambahan
    * Authentikasi dan otorisasi perlu dilakukan untuk setiap fragmen data
    * Error Handling yang kompleks

5. In what ways could the Rust gRPC code be structured to facilitate code reuse and modularity, promoting maintainability and extensibility over time?

    Rust gRPC code dapat disusun dengan menerapkan separation of concerns pada bagian business logic, sehingga aplikasi menjadi lebih mudah dikembangkan tanpa memengaruhi keseluruhan sistem yang dapat mendukung maintainability jangka panjang. Selain itu, membuat modul atau library bersama (shared) untuk bagian kode yang bersifat serupa (seperti hasil refactoring) juga membantu meningkatkan efisiensi. Penggunaan generics memungkinkan fungsi atau struktur menangani berbagai tipe data dengan fleksibel, mendukung reusabilitas. Di sisi lain, penerapan beberapa design pattern, seperti Builder pattern untuk membangun objek kompleks, juga dapat meningkatkan modularitas dan keterpisahan kode, memperkuat fondasi sistem yang scalable dan extensible.

6. In the MyPaymentService implementation, what additional steps might be necessary to handle more complex payment processing logic?

    Steps yang dapat dilakukan adalah dengan menambah validasi input sebelum memproses pembayaran, menambahkan logika simulasi atau koneksi ke layanan pembayaran eksternal, handle authentikasi dan otorisasi, dan mungkin menggunakan database untuk menyimpan data riwayat pembayaran.

7. What impact does the adoption of gRPC as a communication protocol have on the overall architecture and design of distributed systems, particularly in terms of interoperability with other technologies and platforms?

    Mengadopsi gRPC dapat membuat arsitektur sistem lebih efisien dan terstruktur, terutama untuk komunikasi antar layanan (microservices). gRPC menggunakan Protocol Buffers (protobuf) yang cepat dan ringan dibandingkan JSON. Namun, interoperabilitas bisa jadi tantangan karena gRPC lebih optimal digunakan dalam ekosistem yang mendukungnya seperti sistem berbasis Google atau bahasa seperti Go, Java, C++. Untuk platform yang tidak mendukung gRPC secara langsung (seperti browser), dibutuhkan proxy atau konversi ke REST.

8. What are the advantages and disadvantages of using HTTP/2, the underlying protocol for gRPC, compared to HTTP/1.1 or HTTP/1.1 with WebSocket for REST APIs?

    HTTP/2 memiliki kelebihan seperti multiplexing (mengirim beberapa request sekaligus dalam satu koneksi), header compression, dan lebih hemat bandwidth—fitur ini membuat gRPC jauh lebih cepat daripada HTTP/1.1. Dibanding WebSocket, HTTP/2 tetap menggunakan pola request-response sehingga tetap aman dan cocok untuk layanan terstruktur. Namun, HTTP/2 dan gRPC belum seumum HTTP/1.1 di semua lingkungan, sehingga setup awalnya bisa lebih rumit atau memerlukan penyesuaian.

9. How does the request-response model of REST APIs contrast with the bidirectional streaming capabilities of gRPC in terms of real-time communication and responsiveness?

    REST bekerja berdasarkan permintaan dan jawaban satu arah: klien minta, server jawab. Ini cocok untuk operasi biasa, tapi kurang ideal untuk real-time. gRPC mendukung streaming dua arah, di mana klien dan server bisa saling mengirim data terus-menerus tanpa menunggu balasan, mirip seperti ngobrol. Ini sangat berguna untuk aplikasi real-time seperti chat, notifikasi live, atau sensor data.

10. What are the implications of the schema-based approach of gRPC, using Protocol Buffers, compared to the more flexible, schema-less nature of JSON in REST API payloads?

    gRPC menggunakan skema tetap dari Protocol Buffers, yang berarti data harus sesuai format yang telah didefinisikan terlebih dahulu. Ini membuat komunikasi lebih cepat dan validasi data lebih mudah. Tapi, perubahan struktur data butuh regenerasi kode dan bisa kurang fleksibel. Sebaliknya, REST dengan JSON lebih fleksibel dan mudah diubah, tapi bisa menimbulkan error jika formatnya tidak konsisten atau tidak didokumentasikan dengan baik.