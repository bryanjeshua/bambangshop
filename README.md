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

You can download the Postman Collection JSON here: https://ristek.link/AdvProgWeek7Postman

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
-   [ ] Clone https://gitlab.com/ichlaffterlalu/bambangshop to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [ ] Commit: `Create Subscriber model struct.`
    -   [ ] Commit: `Create Notification model struct.`
    -   [ ] Commit: `Create Subscriber database and Subscriber repository struct skeleton.`
    -   [ ] Commit: `Implement add function in Subscriber repository.`
    -   [ ] Commit: `Implement list_all function in Subscriber repository.`
    -   [ ] Commit: `Implement delete function in Subscriber repository.`
    -   [ ] Write answers of your learning module's "Reflection Publisher-1" questions in this README.
-   **STAGE 2: Implement services and controllers**
    -   [ ] Commit: `Create Notification service struct skeleton.`
    -   [ ] Commit: `Implement subscribe function in Notification service.`
    -   [ ] Commit: `Implement subscribe function in Notification controller.`
    -   [ ] Commit: `Implement unsubscribe function in Notification service.`
    -   [ ] Commit: `Implement unsubscribe function in Notification controller.`
    -   [ ] Write answers of your learning module's "Reflection Publisher-2" questions in this README.
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
1. Dalam design pattern Observer seperti yang dijelaskan dalam buku "Head First Design Patterns," Subscriber biasanya merupakan sebuah interface. Hal ini memungkinkan berbagai jenis subscriber untuk mengimplementasikan interface yang masing-masingnya menangani updates dengan caranya sendiri. Menggunakan interface akan mendorong fleksibilitas dan ekstensibilitas karena kita dapat menambahkan jenis subscriber baru tanpa mengubah mekanisme notifikasi.  Dalam kasus ini, karena semua subscriber diasumsikan berperilaku identik dan belum  ada kebutuhan untuk perilaku secara plimorfisme, maka satu struktur Subscriber mungkin cukup. 

2. Pada kasus ini, menurut pendapat saya penggunaan Vec(list) tidak akan cukup. Hal ini karena suatu vec bisa saja menyebabkan id dari Subscriber berduplikasi. Padahal, id ini diharapkan mempunyai sifat yang unik. dengan menggunakan DashMap, kita bisa memastikan bahwa setiap key bersifat unik. Key yang berduplikat akan dioverwrite sehingga lebih efisien. Tak hanya itu, data juga terjaga konsistensinya.

3. Menurut pendapat saya, penggunaan DashMap tetap krusial. Hal ini untuk memastikan thread safetynes. HashMap pada Rust tidaklah bersifat thread-safe bahkan sekalipun diterapkan dengan menggunakan Singleton Pattern. Dengan pattern tersebut, tetap saja ada kemungkinan data races yang disebabkan akses yang dilakukan secara konkuren dari beragam thread. Dengan menggunakan DashMap, kita memanfaatkan mekanisme internal locking dan memastikan hanya ada satu thread yang memodifikasi satu Subscriber entry pada satu waktu. Hal ini guna mencegah data corruption.


#### Reflection Publisher-2
- In the Model-View Controller (MVC) compound pattern, there is no “Service” and “Repository”. Model in MVC covers both data storage and business logic. Explain based on your understanding of design principles, why we need to separate “Service” and “Repository” from a Model?
Menurut pendapat saya, pemisahan Service dan Repository penting dilakukan. Hal ini bertujuan agar kode kita lebih clean dan mudah untuk diolah. Model sebaiknya hanya merepresentasikan data. Service sebaiknyya bertugas untuk mengelola alur bisnis. Sedangkan, repository bertugas untuk mengelolah dan mengambil data. Pemisahan ini membantu setiap bagiannya untuk melakukan tugasnya tanpa menginterfensi atau berdampak ke bagian yang lain. Pemisahan ini membantu dalam proses pembaruan dan mencegah bug yang tak diperlukan.

- What happens if we only use the Model? Explain your imagination on how the interactions between each model (Program, Subscriber, Notification) affect the code complexity for each model?
Kalo kita hanya memakai Model saja, maka akan sangat berantakan. Misalnya, interaksi antara Program, Subscriber, dan Notification akan menjadi snagat kompleks. Susah sekali bagi kita untuk dapat mengolah data, menentukan hal yang dilakukan, dan menyimpannya. Kodenya juga akan jadi sangat panjang dan susah dipahami. 

- Have you explored more about Postman? Tell us how this tool helps you to test your current work. You might want to also list which features in Postman you are interested in or feel like it is helpful to help your Group Project or any of your future software engineering projects.
Postman adalah alat yang bisa membantu saya menguji software saya terutama di bagian saat program saya berinteraksi dengan program lain lewat api. Postman ini bagus karena sangat mudah digunakan dan kita dapat memulai testing dengan cepat. Kita juga bisa mengubah data yang diirimkan, melihat apa yang diterima, dan mengecek semua yang sedang terjadi, dan semua fitur tersebut rencanannya akan saya coba utilisasikan di proyek kelompok. Postman bagus untuk bisa memastikan software kita tetap jalan meski di beragam situasi.

#### Reflection Publisher-3
- Observer Pattern has two variations: Push model (publisher pushes data to subscribers) and Pull model (subscribers pull data from publisher). In this tutorial case, which variation of Observer Pattern that we use?
Dalam tutorial kali ini kita menggunakan push model. Push model pada observer pattern akan secara aktif mengirimkan data kepada subscribers ketika ada sesuat yang baru.

- What are the advantages and disadvantages of using the other variation of Observer Pattern for this tutorial case? (example: if you answer Q1 with Push, then imagine if we used Pull)
Menurut pendapat saya, terdapat kelebihan dan kekurangan kalau misalnya tutorial ini diterapkan dengan pull model. Kelebihannya, Subscriber hanya akan melakukan request terhadap updates saat mereka membutuhkannya. Ini mengurangi kebutuhan transmisi dan proses data yang tak dibutuhkam. Akan tetapi, terdapat kekurangan yakni kemungkinan subscribers tidak menerima update sesaat setelah informasinya tersedia. Dampaknya, bisa saja terjadi potensi delay untuk mendapatkan informasi. Selain itu, kompleksitas kode juga akan semakin bertambah pada bagian Subscriber.

- Explain what will happen to the program if we decide to not use multi-threading in the notification process.
Kalau kita tidak menggunakan multi threading, maka programnya berarti akan dilaksanakan secara sekuensial. Dampaknya, seluruh proses pengiriman notifikasi akan terjadi secara lambat apalagi kalau ada banyak Subscribers. Setiap subscriber harus menunggu subcriber sebelumnya selesai menerima notifikasi. Delay akan membuat program kita tampak tidak kapabel dalam mengurusi data masif yang realtime.