# BambangShop Publisher App
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
4.  `repository`: this module contains structs that serve as databases and methods to access the databases.
    You can use methods of the struct to get list of objects, or operating an object (create, read, update, delete).

This repository provides a basic functionality that makes BambangShop work: ability to create, read, and delete `Product`s.
This repository already contains a functioning `Product` model, repository, service, and controllers that you can try right away.

As this is an Observer Design Pattern tutorial repository, you need to implement another feature: `Notification`.
This feature will notify creation, promotion, and deletion of a product, to external subscribers that are interested of a certain product type.
The subscribers are another Rocket instances, so the notification will be sent using HTTP POST request to each subscriber's `receive notification` address.

## API Documentations

After you download the Postman Collection, you can try the endpoints inside "BambangShop Publisher" folder.
This Postman collection also contains endpoints that you need to implement later on (the `Notification` feature).

Postman is an installable client that you can use to test web endpoints using HTTP request.
You can also make automated functional testing scripts for REST API projects using this client.
You can install Postman via this website: https://www.postman.com/downloads/

## How to Run in Development Environment
1.  Set up environment variables first by creating `.env` file.
    Here is the example of `.env` file:
    ```bash
    APP_INSTANCE_ROOT_URL="http://localhost:8000"
    ```
    Here are the details of each environment variable:
    | variable              | type   | description                                                |
    |-----------------------|--------|------------------------------------------------------------|
    | APP_INSTANCE_ROOT_URL | string | URL address where this publisher instance can be accessed. |
2.  Use `cargo run` to run this app.
    (You might want to use `cargo check` if you only need to verify your work without running the app.)

## Mandatory Checklists (Publisher)
-   [V] Clone https://gitlab.com/ichlaffterlalu/bambangshop to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [V] Commit: `Create Subscriber model struct.`
    -   [V] Commit: `Create Notification model struct.`
    -   [V] Commit: `Create Subscriber database and Subscriber repository struct skeleton.`
    -   [V] Commit: `Implement add function in Subscriber repository.`
    -   [V] Commit: `Implement list_all function in Subscriber repository.`
    -   [V] Commit: `Implement delete function in Subscriber repository.`
    -   [V] Write answers of your learning module's "Reflection Publisher-1" questions in this README.
-   **STAGE 2: Implement services and controllers**
    -   [V] Commit: `Create Notification service struct skeleton.`
    -   [V] Commit: `Implement subscribe function in Notification service.`
    -   [V] Commit: `Implement subscribe function in Notification controller.`
    -   [V] Commit: `Implement unsubscribe function in Notification service.`
    -   [V] Commit: `Implement unsubscribe function in Notification controller.`
    -   [V] Write answers of your learning module's "Reflection Publisher-2" questions in this README.
-   **STAGE 3: Implement notification mechanism**
    -   [ ] Commit: `Implement update method in Subscriber model to send notification HTTP requests.`
    -   [ ] Commit: `Implement notify function in Notification service to notify each Subscriber.`
    -   [ ] Commit: `Implement publish function in Program service and Program controller.`
    -   [ ] Commit: `Edit Product service methods to call notify after create/delete.`
    -   [ ] Write answers of your learning module's "Reflection Publisher-3" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Publisher) Reflections

#### Reflection Publisher-1

1. Kita tidak memerlukan interface dalam kasus ini, karena satu Model struct sudah mencukupi. Setiap subscriber memiliki perilaku yang seragam, yaitu menerima notifikasi melalui HTTP untuk `product_type` yang mereka subscribe.

2. `DashMap` lebih cocok digunakan dalam kasus ini karena memungkinkan penyimpanan data dengan key yang terjamin keunikannya, seperti ID untuk Program dan URL untuk Subscriber.

3. Ya, `DashMap` tetap diperlukan meskipun sudah menggunakan desain singleton. Singleton hanya memastikan bahwa data bersifat tunggal secara global, tetapi tidak secara otomatis membuatnya aman untuk diakses oleh banyak thread secara bersamaan (thread-safe). `DashMap` digunakan untuk menjamin concurrent access ke data `SUBSCRIBERS`.

#### Reflection Publisher-2

1. Dalam arsitektur Model-View-Controller (MVC), pemisahan Service dan Repository dari Model didasarkan pada prinsip desain perangkat lunak seperti Separation of Concerns (SoC) dan Single Responsibility Principle (SRP). Model dalam MVC bertanggung jawab atas representasi data dan aturan bisnis, tetapi jika model juga menangani logika bisnis dan akses ke penyimpanan data, maka kode akan menjadi kompleks, sulit diuji, dan sulit diubah. Repository berfungsi sebagai lapisan yang menangani komunikasi dengan database atau sumber data lainnya, sehingga memungkinkan perubahan sumber data tanpa memengaruhi logika bisnis. Sementara itu, Service bertindak sebagai penghubung antara Model dan Repository dengan menangani logika bisnis yang lebih kompleks serta memfasilitasi interaksi antar-Model. Dengan pemisahan ini, arsitektur menjadi lebih modular, mudah diuji, dan fleksibel untuk dikembangkan.

2. Jika hanya menggunakan Model tanpa Service dan Repository, interaksi antara entitas seperti Program, Subscriber, dan Notification akan meningkatkan kompleksitas kode. Model akan memiliki tight coupling, di mana ia harus menangani logika bisnis dan akses database secara bersamaan, menyebabkan kesulitan dalam pengujian dan pemeliharaan. Selain itu, tanpa Service, logika bisnis yang sama bisa saja diimplementasikan berulang kali di berbagai tempat, menciptakan duplikasi kode yang tidak efisien. Dalam sistem Publisher-Subscriber, misalnya, Model harus menangani pencarian Program yang relevan, mengambil daftar Subscribers yang terdaftar, dan mengirim notifikasi ke masing-masing Subscriber. Jika semua ini dilakukan dalam Model, maka perubahan kecil dalam sistem dapat berdampak besar pada keseluruhan kode, menambah technical debt yang sulit dikelola.

3. Dalam eksplorasi Postman, saya menemukan bahwa alat ini sangat membantu dalam pengujian API. Postman memungkinkan pengembang untuk mengirim berbagai jenis request seperti GET, POST, PUT, DELETE, mengatur parameter, header, serta body untuk menguji endpoint API dengan mudah. Fitur Collection & Environment Variables memungkinkan pengelolaan request dalam skala besar dengan konfigurasi yang dapat digunakan kembali. Selain itu, Postman mendukung Automated Testing, di mana skrip uji otomatis dapat ditambahkan untuk memvalidasi respons API, serta menyediakan Mock Server yang memungkinkan pengujian API tanpa perlu terhubung ke backend secara langsung. Dalam proyek pengembangan perangkat lunak, Postman membantu dalam pengujian endpoint API sebelum diakses oleh frontend, debugging API untuk memastikan request dan response sesuai harapan, serta mendokumentasikan API untuk mempermudah kolaborasi dengan tim lain.

Dengan demikian, pemisahan Service dan Repository dalam MVC menghasilkan kode yang lebih modular dan mudah dipelihara, dibandingkan dengan hanya menggunakan Model, yang akan meningkatkan kompleksitas sistem. Selain itu, Postman menjadi alat yang sangat berguna dalam pengujian API karena fitur-fiturnya yang mendukung efisiensi dan otomatisasi pengujian, yang sangat bermanfaat dalam proyek pengembangan perangkat lunak.

#### Reflection Publisher-3
