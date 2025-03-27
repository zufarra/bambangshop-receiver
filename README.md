# BambangShop Receiver App
Tutorial and Example for Advanced Programming 2024 - Faculty of Computer Science, Universitas Indonesia

---

## About this Project
In this repository, we have provided you a REST (REpresentational State Transfer) API project using Rocket web framework.

This project consists of four modules:
1.  `controller`: this module contains handler functions used to receive request and send responses.
    In Model-View-Controller (MVC) pattern, this is the Controller part.
2.  `model`: this module contains structs that serve as data containers.
    In MVC pattern, this is the Model part.
3.  `service`: this module contains structs with business logic methods.
    In MVC pattern, this is also the Model part.
4.  `repository`: this module contains structs that serve as databases.
    You can use methods of the struct to get list of objects, or operating an object (create, read, update, delete).

This repository provides a Rocket web framework skeleton that you can work with.

As this is an Observer Design Pattern tutorial repository, you need to implement a feature: `Notification`.
This feature will receive notifications of creation, promotion, and deletion of a product, when this receiver instance is subscribed to a certain product type.
The notification will be sent using HTTP POST request, so you need to make the receiver endpoint in this project.

## API Documentations

You can download the Postman Collection JSON here: https://ristek.link/AdvProgWeek7Postman

After you download the Postman Collection, you can try the endpoints inside "BambangShop Receiver" folder.

Postman is an installable client that you can use to test web endpoints using HTTP request.
You can also make automated functional testing scripts for REST API projects using this client.
You can install Postman via this website: https://www.postman.com/downloads/

## How to Run in Development Environment
1.  Set up environment variables first by creating `.env` file.
    Here is the example of `.env` file:
    ```bash
    ROCKET_PORT=8001
    APP_INSTANCE_ROOT_URL=http://localhost:${ROCKET_PORT}
    APP_PUBLISHER_ROOT_URL=http://localhost:8000
    APP_INSTANCE_NAME=Safira Sudrajat
    ```
    Here are the details of each environment variable:
    | variable                | type   | description                                                     |
    |-------------------------|--------|-----------------------------------------------------------------|
    | ROCKET_PORT             | string | Port number that will be listened by this receiver instance.    |
    | APP_INSTANCE_ROOT_URL   | string | URL address where this receiver instance can be accessed.       |
    | APP_PUUBLISHER_ROOT_URL | string | URL address where the publisher instance can be accessed.       |
    | APP_INSTANCE_NAME       | string | Name of this receiver instance, will be shown on notifications. |
2.  Use `cargo run` to run this app.
    (You might want to use `cargo check` if you only need to verify your work without running the app.)
3.  To simulate multiple instances of BambangShop Receiver (as the tutorial mandates you to do so),
    you can open new terminal, then edit `ROCKET_PORT` in `.env` file, then execute another `cargo run`.

    For example, if you want to run 3 (three) instances of BambangShop Receiver at port `8001`, `8002`, and `8003`, you can do these steps:
    -   Edit `ROCKET_PORT` in `.env` to `8001`, then execute `cargo run`.
    -   Open new terminal, edit `ROCKET_PORT` in `.env` to `8002`, then execute `cargo run`.
    -   Open another new terminal, edit `ROCKET_PORT` in `.env` to `8003`, then execute `cargo run`.

## Mandatory Checklists (Subscriber)
-   [ ] Clone https://gitlab.com/ichlaffterlalu/bambangshop-receiver to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [ ] Commit: `Create Notification model struct.`
    -   [ ] Commit: `Create SubscriberRequest model struct.`
    -   [ ] Commit: `Create Notification database and Notification repository struct skeleton.`
    -   [ ] Commit: `Implement add function in Notification repository.`
    -   [ ] Commit: `Implement list_all_as_string function in Notification repository.`
    -   [ ] Write answers of your learning module's "Reflection Subscriber-1" questions in this README.
-   **STAGE 3: Implement services and controllers**
    -   [ ] Commit: `Create Notification service struct skeleton.`
    -   [ ] Commit: `Implement subscribe function in Notification service.`
    -   [ ] Commit: `Implement subscribe function in Notification controller.`
    -   [ ] Commit: `Implement unsubscribe function in Notification service.`
    -   [ ] Commit: `Implement unsubscribe function in Notification controller.`
    -   [ ] Commit: `Implement receive_notification function in Notification service.`
    -   [ ] Commit: `Implement receive function in Notification controller.`
    -   [ ] Commit: `Implement list_messages function in Notification service.`
    -   [ ] Commit: `Implement list function in Notification controller.`
    -   [ ] Write answers of your learning module's "Reflection Subscriber-2" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Subscriber) Reflections

#### Reflection Subscriber-1

Pertanyaan 1: In this tutorial, we used RwLock<> to synchronise the use of Vec of Notifications. Explain why it is necessary for this case, and explain why we do not use Mutex<> instead?

Jawaban:

RwLock (Read-Write Lock) dipilih karena memiliki keunggulan spesifik dalam skenario ini:

- Memungkinkan multiple read operations secara concurrent

- Hanya satu write operation yang dapat dilakukan pada satu waktu

- Mengoptimalkan performa untuk operasi yang lebih banyak membaca daripada menulis

Keuntungan RwLock dibandingkan Mutex:

- Mutex mengunci seluruh struktur data untuk setiap operasi (read atau write)

- RwLock memperbolehkan beberapa thread membaca secara bersamaan

- Meningkatkan throughput pada operasi yang lebih sering membaca


Pertanyaan 2: In this tutorial, we used lazy_static external library to define Vec and DashMap as a “static” variable. Compared to Java where we can mutate the content of a static variable via a static function, why did not Rust allow us to do so?


Jawaban:

Rust memiliki aturan ketat tentang mutasi variabel statis karena alasan keamanan:

- Mencegah data race pada variabel global

- Menjamin thread safety

- Menghindari side effects yang tidak terduga

Dalam Java, mutasi variabel statis tidak aman. Rust memaksa penggunaan tipe khusus untuk mutasi variabel statis, seperti:

- Mutex<T>

- RwLock<T>

- AtomicXXX

- lazy_static! macro

Alasan desain Rust:

- Mencegah bug concurrency

- Memaksa programmer untuk secara eksplisit menangani shared state

- Menjamin memory safety dan thread safety pada compile-time

Jadi, dapat disimpulkan bahwa Rust memilih desain yang lebih aman dan eksplisit dibandingkan bahasa lain, dengan mengharuskan mekanisme sinkronisasi yang jelas untuk mengubah variabel statis, terutama dalam konteks concurrent programming.

#### Reflection Subscriber-2

Pertanyaan 1: Have you explored things outside of the steps in the tutorial, for example: src/lib.rs? If not, explain why you did not do so. If yes, explain things that you have learned from those other parts of code.

Jawaban:

Ya, saya telah mengeksplorasi beberapa bagian di luar langkah-langkah dalam tutorial, terutama di src/lib.rs dan src/main.rs. Dari bagian-bagian kode tersebut, saya mempelajari beberapa hal berikut:

a. Penggunaan lazy_static! untuk Inisialisasi Global

Dalam src/lib.rs, saya melihat bagaimana lazy_static! digunakan untuk mendeklarasikan objek global seperti REQWEST_CLIENT dan APP_CONFIG. Ini berguna karena objek hanya akan diinisialisasi sekali ketika pertama kali diakses, yang menghemat sumber daya dan meningkatkan efisiensi.

b. Konfigurasi Aplikasi dengan dotenvy dan Figment

- dotenvy digunakan untuk membaca variabel lingkungan dari file .env, yang memungkinkan konfigurasi fleksibel tanpa mengubah kode.

- Figment membantu dalam pengelolaan konfigurasi dengan menggabungkan nilai default dan variabel lingkungan (Env::prefixed("APP_")), sehingga aplikasi dapat dengan mudah dikonfigurasi ulang untuk berbagai lingkungan (development, staging, production).

c. Struktur AppConfig dengan Getters untuk Enkapsulasi

AppConfig menggunakan derive Getters dari crate getset untuk membuat metode getter secara otomatis. Ini membantu dalam menjaga enkapsulasi dengan memberikan akses hanya ke atribut yang diperlukan tanpa mengekspos secara langsung.

d. Penanganan Error yang Terstruktur

- Struct ErrorResponse digunakan untuk menangani error dengan format JSON yang terstandarisasi.

- compose_error_response dibuat agar lebih mudah mengembalikan error dalam bentuk Custom<Json<ErrorResponse>>, yang mempermudah debugging dan respons API yang lebih konsisten.

e. Struktur Proyek Rocket di src/main.rs

- #[macro_use] extern crate rocket; digunakan untuk mengimpor macro dari Rocket, yang diperlukan untuk mendefinisikan routes dengan anotasi seperti #[get("/")].

- route_stage() yang dipanggil dalam rocket::build() menunjukkan adanya sistem modularisasi dalam proyek ini, di mana routing ditangani dalam modul controller.

- rocket::build() digunakan untuk membangun aplikasi Rocket dengan menambahkan konfigurasi dan mengelola dependensi seperti reqwest::Client.

Dari eksplorasi ini, saya mendapatkan pemahaman yang lebih dalam tentang bagaimana aplikasi Rust berbasis Rocket dikonfigurasi, bagaimana cara menangani error dengan baik, serta bagaimana memastikan kode tetap modular dan mudah dipelihara.

Pertanyaan 2: Since you have completed the tutorial by now and have tried to test your notification system by spawning multiple instances of Receiver, explain how Observer pattern eases you to plug in more subscribers. How about spawning more than one instance of Main app, will it still be easy enough to add to the system?


Jawaban:

Pola Observer memudahkan penambahan lebih banyak subscriber (Receiver) karena sistem dibangun dengan prinsip de-coupling antara Publisher (Main app) dan Subscriber (Receiver). Dengan pendekatan ini, Publisher tidak perlu mengetahui detail setiap Receiver secara langsung. Setiap Receiver cukup mendaftarkan dirinya ke sistem, dan ketika suatu event terjadi, Publisher secara otomatis akan mengirimkan notifikasi ke semua Receiver yang terdaftar. Hal ini membuat proses penambahan Receiver baru menjadi lebih fleksibel dan tidak memerlukan perubahan pada kode Publisher.

Namun, jika ingin menambahkan lebih dari satu instance Main app (Publisher), kompleksitas sistem akan meningkat. Dalam skenario ini, perlu dipastikan bahwa setiap instance Publisher memiliki mekanisme koordinasi agar tidak terjadi duplikasi pengiriman notifikasi atau inkonsistensi data. Salah satu solusi yang dapat diterapkan adalah menggunakan message broker seperti Redis Pub/Sub atau Kafka untuk menangani komunikasi antar Publisher dan Subscriber secara lebih terstruktur. Dengan pendekatan yang tepat, sistem tetap dapat diperluas dengan mudah, baik dalam hal menambah Receiver maupun menambah instance Publisher.

Pertanyaan 3: Have you tried to make your own Tests, or enhance documentation on your Postman collection? If you have tried those features, tell us whether it is useful for your work (it can be your tutorial work or your Group Project).


Jawaban:

Saya telah mencoba menggunakan Postman collection yang disediakan untuk berinteraksi dengan aplikasi melalui HTTP request. Dalam prosesnya, saya menguji berbagai endpoint seperti subscribe ke product type, membuat produk baru, mempromosikan produk, dan menghapus produk untuk melihat apakah notifikasi berhasil dikirim ke Receiver app.

Dari pengalaman ini, saya menemukan bahwa menambahkan test di Postman sangat membantu dalam memastikan setiap perubahan pada sistem tidak merusak fungsionalitas utama. Dengan menulis test, saya bisa secara otomatis memverifikasi apakah respons dari setiap request sesuai dengan yang diharapkan. Selain itu, menambahkan dokumentasi ke Postman collection membuat proses debugging lebih mudah, terutama saat harus memahami kembali cara kerja API setelah beberapa waktu tidak menggunakannya.

Secara keseluruhan, fitur ini sangat berguna untuk mempercepat pengujian dan validasi implementasi dari tutorial yang telah saya selesaikan. Jika ada perubahan di masa depan, saya dapat langsung menjalankan kembali koleksi Postman tanpa harus mengetes setiap request secara manual.