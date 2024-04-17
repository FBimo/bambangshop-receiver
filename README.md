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
-   [x] Clone https://gitlab.com/ichlaffterlalu/bambangshop-receiver to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [x] Commit: `Create Notification model struct.`
    -   [x] Commit: `Create SubscriberRequest model struct.`
    -   [x] Commit: `Create Notification database and Notification repository struct skeleton.`
    -   [x] Commit: `Implement add function in Notification repository.`
    -   [x] Commit: `Implement list_all_as_string function in Notification repository.`
    -   [x] Write answers of your learning module's "Reflection Subscriber-1" questions in this README.
-   **STAGE 2: Implement services and controllers**
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
1. Terdapat beberapa alasan penggunaan RwLock<>,
    - **Masalah Pembaca-Penulis**: Dalam kasus notifikasi, biasanya ada jauh lebih banyak operasi baca (pengamat memeriksa notifikasi baru) daripada operasi menulis (notifikasi baru ditambahkan). RwLock memungkinkan banyak pembaca atau satu penulis pada satu waktu, yang sesuai untuk skenario ini. Menggunakan Mutex akan memperkenalkan persaingan yang tidak perlu karena hanya satu thread yang diizinkan untuk mengakses data pada satu waktu, bahkan untuk operasi hanya baca.
    - **Concurrency**: Aplikasi ini mungkin memiliki beberapa _thread_ yang secara bersamaan mengakses atau memodifikasi daftar notifikasi. Oleh karena itu, kita memerlukan mekanisme sinkronisasi untuk memastikan keselamatan _thread_ dan mencegah perlombaan data.
    - **Kinerja**: RwLock menyediakan kinerja yang lebih baik untuk skenario ketika terdapat bacaan yang sering dan penulisan yang sesekali dibandingkan dengan Mutex yang dapat menyebabkan persaingan dan penurunan kinerja dalam skenario baca yang tinggi.

2. Meskipun Rust memiliki konsep variabel statis seperti di Java, namun pendekatan Rust terhadap keselamatan memori dan pola konkurensi yang aman membuatnya tidak mengizinkan perubahan pada variabel statis setelah inisialisasi. Sebagai gantinya, Rust mendorong penggunaan pola desain yang aman dan ketat untuk mengelola data bersama dalam kasus konkurensi.

#### Reflection Subscriber-2
1. Beberapa hal yang saya pelajari setelah adanya tutorial ini seperti,

    - **Struktur Proyek**: Saya belajar tentang struktur proyek Rust yang umum digunakan, seperti struktur modul dan bagaimana file lib.rs digunakan sebagai entry point untuk sebuah crate (library Rust).

    - **_Dependency Management_**: Saya melihat bagaimana _dependencies_ ditambahkan dan dikelola dalam proyek Rust menggunakan _file_ `Cargo.toml`. Ini membantu saya memahami dependensi eksternal yang digunakan dalam proyek ini.

    - **Concurrency Patterns**: Saya melihat bagaimana pola konkurensi yang aman diimplementasikan dalam proyek ini, seperti menggunakan RwLock untuk mengelola akses ke data bersama secara _thread-safe_.

    - **_Static Variables_**: Saya memahami mengapa lazy_static digunakan untuk mendefinisikan variabel statis dalam proyek ini, dan mengapa Rust tidak mengizinkan mutasi langsung pada variabel statis.

2. Penerapan pola Observer memudahkan penambahan pelanggan baru ke sistem karena tidak memerlukan modifikasi pada subjek atau publisher. Dengan pola ini, pelanggan baru dapat dengan mudah ditambahkan sebagai observer tanpa memengaruhi subjek atau observer lainnya. Ini memungkinkan fleksibilitas yang besar dalam mengelola pelanggan dan memperluas fungsionalitas sistem tanpa memodifikasi bagian inti dari aplikasi. Namun, ketika kita membuat lebih dari satu _instance_ dari aplikasi utama, tantangannya mungkin lebih kompleks. Meskipun pola Observer dapat memudahkan penambahan pelanggan, menangani _multiple instance_ dari aplikasi utama dapat menimbulkan masalah koordinasi dan sinkronisasi antara _instance_ yang berjalan secara paralel. Ini melibatkan masalah seperti koordinasi akses ke sumber daya bersama, manajemen konflik, dan konsistensi data.

3. Tentu, fitur-fitur tersebut akan sangat membantu dalam pengembangan Proyek Grup. Kita dapat berbagi Koleksi yang telah dibuat kepada _programmer_ lainnya agar mereka dapat memahami API yang telah saya rancang dan melakukan pengujian dengan lebih mudah.
