# BambangShop Publisher App

Tutorial and Example for Advanced Programming 2024 - Faculty of Computer Science, Universitas Indonesia

---

## About this Project

In this repository, we have provided you a REST (REpresentational State Transfer) API project using Rocket web framework.

This project consists of four modules:

1. `controller`: this module contains handler functions used to receive request and send responses.
    In Model-View-Controller (MVC) pattern, this is the Controller part.
2. `model`: this module contains structs that serve as data containers.
    In MVC pattern, this is the Model part.
3. `service`: this module contains structs with business logic methods.
    In MVC pattern, this is also the Model part.
4. `repository`: this module contains structs that serve as databases and methods to access the databases.
    You can use methods of the struct to get list of objects, or operating an object (create, read, update, delete).

This repository provides a basic functionality that makes BambangShop work: ability to create, read, and delete `Product`s.
This repository already contains a functioning `Product` model, repository, service, and controllers that you can try right away.

As this is an Observer Design Pattern tutorial repository, you need to implement another feature: `Notification`.
This feature will notify creation, promotion, and deletion of a product, to external subscribers that are interested of a certain product type.
The subscribers are another Rocket instances, so the notification will be sent using HTTP POST request to each subscriber's `receive notification` address.

## API Documentations

You can download the Postman Collection JSON here: <https://ristek.link/AdvProgWeek7Postman>

After you download the Postman Collection, you can try the endpoints inside "BambangShop Publisher" folder.
This Postman collection also contains endpoints that you need to implement later on (the `Notification` feature).

Postman is an installable client that you can use to test web endpoints using HTTP request.
You can also make automated functional testing scripts for REST API projects using this client.
You can install Postman via this website: <https://www.postman.com/downloads/>

## How to Run in Development Environment

1. Set up environment variables first by creating `.env` file.
    Here is the example of `.env` file:

    ```bash
    APP_INSTANCE_ROOT_URL="http://localhost:8000"
    ```

    Here are the details of each environment variable:
    | variable              | type   | description                                                |
    |-----------------------|--------|------------------------------------------------------------|
    | APP_INSTANCE_ROOT_URL | string | URL address where this publisher instance can be accessed. |
2. Use `cargo run` to run this app.
    (You might want to use `cargo check` if you only need to verify your work without running the app.)

## Mandatory Checklists (Publisher)

- [x] Clone <https://gitlab.com/ichlaffterlalu/bambangshop> to a new repository.
- **STAGE 1: Implement models and repositories**
  - [x] Commit: `Create Subscriber model struct.`
  - [x] Commit: `Create Notification model struct.`
  - [x] Commit: `Create Subscriber database and Subscriber repository struct skeleton.`
  - [x] Commit: `Implement add function in Subscriber repository.`
  - [x] Commit: `Implement list_all function in Subscriber repository.`
  - [x] Commit: `Implement delete function in Subscriber repository.`
  - [x] Write answers of your learning module's "Reflection Publisher-1" questions in this README.
- **STAGE 2: Implement services and controllers**
  - [x] Commit: `Create Notification service struct skeleton.`
  - [x] Commit: `Implement subscribe function in Notification service.`
  - [x] Commit: `Implement subscribe function in Notification controller.`
  - [x] Commit: `Implement unsubscribe function in Notification service.`
  - [x] Commit: `Implement unsubscribe function in Notification controller.`
  - [x] Write answers of your learning module's "Reflection Publisher-2" questions in this README.
- **STAGE 3: Implement notification mechanism**
  - [x] Commit: `Implement update method in Subscriber model to send notification HTTP requests.`
  - [x] Commit: `Implement notify function in Notification service to notify each Subscriber.`
  - [x] Commit: `Implement publish function in Program service and Program controller.`
  - [x] Commit: `Edit Product service methods to call notify after create/delete.`
  - [x] Write answers of your learning module's "Reflection Publisher-3" questions in this README.

## Your Reflections

This is the place for you to write reflections:

### Mandatory (Publisher) Reflections

#### Reflection Publisher-1

Q: In the Observer pattern diagram explained by the Head First Design Pattern book, Subscriber is defined as an interface.
Explain based on your understanding of Observer design patterns, do we still need an interface (or `trait` in Rust)
in this BambangShop case, or a single Model `struct` is enough?

A: Tidak diperlukan interface/`trait`, karena `struct` Subscriber sudah menjadi implementasi Observer model yang akan digunakan untuk semua use-case.

Q: `id` in Program and `url` in `Subscriber` is intended to be unique.
Explain based on your understanding, is using `Vec` (list) sufficient or using `DashMap` (map/dictionary)
like we currently use is necessary for this case?

A: Walaupun bisa saja menggunakan `Vec` untuk menyimpan `id` dan `url`, akan lebih efisien menggunakan `DashMap` karena `id` dan `url` bisa dijadikan key-value pair,
sehingga hanya perlu 1 `DashMap` dibandingkan 2 `Vec` (satu untuk menyimpan `id` dan satu lainnya untuk menyimpan `url`).

Q: When programming using Rust, we are enforced by rigorous compiler constraints to make a thread-safe program.
In the case of the List of Subscribers (`SUBSCRIBERS`) static variable, we used the `DashMap` external library for **thread safe HashMap**.
Explain based on your understanding of design patterns, do we still need `DashMap` or we can implement Singleton pattern instead?

A: Penggunaan `DashMap` dan implementasi Singleton pattern bisa keduanya dilakukan bersamaan.
`DashMap` masih diperlukan karena memberikan implementasi HashMap yang *thread-safe* di multithreading,
dan Singleton pattern diimplementasikan dengan memastikan `SUBSCRIBERS` hanya ada satu dan dapat diakses oleh banyak *thread*.

#### Reflection Publisher-2

Q: In the Model-View Controller (MVC) compound pattern, there is no "Service" and "Repository". Model in MVC covers both data storage and business logic.
Explain based on your understanding of design principles, why we need to separate "Service" and "Repository" from a Model?

A: Kita memisah Model menjadi Service dan Repository untuk menerapkan prinsip **Single Responsbility Principle** (SRP) dari SOLID Principles.
Service akan mengcover *business logic*, sementara Repository menghandle *data storage*.
Pemisahan ini akan membuat kode program menjadi lebih mudah untuk dipahami, diuji (testing), dan dipelihara (maintenance).

Q: What happens if we only use the Model?
Explain your imagination on how the interactions between each model (`Program`, `Subscriber`, `Notification`) affect the code complexity for each model?

A: Apabila kita hanya menggunakan Model, interaksi antara tiap model masih dapat berjalan namun kompleksitas kode akan meningkat.
Kode untuk tiap Model akan memiliki terlalu banyak fungsionalitas, sehingga akan lebih sulit untuk dipahami dan dipelihara.

Q: Have you explored more about **Postman**? Tell us how this tool helps you to test your current work.
You might want to also list which features in Postman you are interested in or feel like it is helpful to help your Group Project
or any of your future software engineering projects.

A: Postman adalah salah satu *tool* yang saya pakai untuk menguji *endpoint* yang telah diimplementasikan di aplikasi ini.
Selain fitur utama akses *endpoint*, saya rasa fitur *Collections* akan sangat membantu saya untuk membuat suatu set uji coba *endpoint* dan membaginya dengan
developer lainnya.

#### Reflection Publisher-3

Q: Observer Pattern has two variations: **Push model** (publisher pushes data to subscribers) **Pull model** (subscribers pull data from publisher).
In this tutorial case, which variation of Observer Pattern that we use?

A: Di tutorial ini, kita menggunakan Observer Pattern variasi **Push model**.
Setiap kali terjadi proses `create`, `delete`, dan `publish` di `ProductService`, `Subscriber` akan diberitahu (*notify*) oleh `NotificationService`.

Q: What are the advantages and disadvantages of using the other variation of Observer Pattern for this tutorial case?
(example: if you answer Q1 with Push, then imagine if we used Pull)

A: Apabila kita menggunakan variasi **Pull model** dari Observer Pattern, maka `Subscriber` akan harus aktif meminta data terbaru dari `NotificationService`.
Keuntungan dari variasi ini, `Subscriber` dapat meminta data dari Publisher kapan saja.
Namun, `Subscriber` akan harus memeriksa data cukup sering apabila ingin mendapatkan notifikasi secara *real-time* sehingga menimbulkan *overhead*.

Q: Explain what will happen to the program if we decide to not use multi-threading in the notification process.

A: Tanpa *multi-threading*, proses notifikasi berpotensial bermasalah karena diproses satu per satu secara berurutan / tidak paralel.
Apabila ada suatu proses yang terhambat karena *error* atau faktor lainnya, proses-proses selanjutnya harus mengantri sehingga terjadi *delay*.
