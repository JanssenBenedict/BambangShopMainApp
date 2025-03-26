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
-   [✓] Clone https://gitlab.com/ichlaffterlalu/bambangshop to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [✓] Commit: `Create Subscriber model struct.`
    -   [✓] Commit: `Create Notification model struct.`
    -   [✓] Commit: `Create Subscriber database and Subscriber repository struct skeleton.`
    -   [✓] Commit: `Implement add function in Subscriber repository.`
    -   [✓] Commit: `Implement list_all function in Subscriber repository.`
    -   [✓] Commit: `Implement delete function in Subscriber repository.`
    -   [✓] Write answers of your learning module's "Reflection Publisher-1" questions in this README.
-   **STAGE 2: Implement services and controllers**
    -   [✓] Commit: `Create Notification service struct skeleton.`
    -   [✓] Commit: `Implement subscribe function in Notification service.`
    -   [✓] Commit: `Implement subscribe function in Notification controller.`
    -   [✓] Commit: `Implement unsubscribe function in Notification service.`
    -   [✓] Commit: `Implement unsubscribe function in Notification controller.`
    -   [✓] Write answers of your learning module's "Reflection Publisher-2" questions in this README.
-   **STAGE 3: Implement notification mechanism**
    -   [✓] Commit: `Implement update method in Subscriber model to send notification HTTP requests.`
    -   [✓] Commit: `Implement notify function in Notification service to notify each Subscriber.`
    -   [✓] Commit: `Implement publish function in Program service and Program controller.`
    -   [✓] Commit: `Edit Product service methods to call notify after create/delete.`
    -   [✓] Write answers of your learning module's "Reflection Publisher-3" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Publisher) Reflections

#### Reflection Publisher-1

1. In the Observer pattern diagram explained by the Head First Design Pattern book, Subscriber is defined as an interface. Explain based on your understanding of Observer design patterns, do we still need an interface (or trait in Rust) in this BambangShop case, or a single Model struct is enough?<br>
Answer:<br>
At this point in the development of the BambangShop project, the implementation of the Subscriber class is still quite simple and straightforward. As of right now, it acts as the only observer in the structure without the existence of different kinds of subscribers with different logic implementations. We only need to define the Subscriber observer as an interface or trait when different roles or types of subscribers need to be implemented. Therefore, we don't need to implement a Subscriber interface or a trait yet and the implementation of the Subscriber observer through a single model struct is enough for now.

2. id in Program and url in Subscriber is intended to be unique. Explain based on your understanding, is using Vec (list) sufficient or using DashMap (map/dictionary) like we currently use is necessary for this case?<br>
Answer:<br>
Using DashMap, like in this current implementation of the BambangShop project, is more efficient, effective, and robust compared to using a Vec. Due to its 'key-value' structure, the id and the url can be stored as the keys and their corresponding values can be the Program and the Subscriber objects. The DashMap structure also performs lookups, insertions, and deletions faster and more efficiently, which can be very useful when frequent operations need to be done at a larger scale.

3. When programming using Rust, we are enforced by rigorous compiler constraints to make a thread-safe program. In the case of the List of Subscribers (SUBSCRIBERS) static variable, we used the DashMap external library for thread safe HashMap. Explain based on your understanding of design patterns, do we still need DashMap or we can implement Singleton pattern instead?<br>
Answer:<br>
Using a DashMap is still the better option when compared to implementing Singleton pattern in this case. A Singleton isn't enough because it doesn't provide or guarantee thread safety, leaving it vulnerable to certain issues, like race conditions when multiple threads attempt to access or modify the data. Meanwhile, the DashMap external library is a thread-safe data structure that allows for built-in concurrent access from multiple threads, allowing multiple threads to perform operations to the data simultaneously and safely.

#### Reflection Publisher-2

1. In the Model-View Controller (MVC) compound pattern, there is no “Service” and “Repository”. Model in MVC covers both data storage and business logic. Explain based on your understanding of design principles, why we need to separate “Service” and “Repository” from a Model?<br>
Answer:<br>
The need to separate Service and Repository from a Model exists due to the fact that Service and Repository both handle different aspects of the project's functionality. The Repository is responsible for handling interactions with the database, which can involve storing and accessing data from the database itself. On the other hand, the Service is responsible for handling the business logic by coordinating data operations utilizing the Repository. Separating Service and Repository follows the Single Responsibility Principle, where separation of functionalities handling different jobs will improve readability, maintainability, and testability.

2. What happens if we only use the Model? Explain your imagination on how the interactions between each model (Program, Subscriber, Notification) affect the code complexity for each model?<br>
Answer:<br>
If we only use the Model without separating the Service and Repository from it, the Model has to handle many different aspects of the project's functionality by itself. The Model would end up becoming too complex with different responsibilities and jobs being tightly coupled together, the process would become much harder to maintain and the risk of errors or bugs would increase.

3. Have you explored more about Postman? Tell us how this tool helps you to test your current work. You might want to also list which features in Postman you are interested in or feel like it is helpful to help your Group Project or any of your future software engineering projects.<br>
Answer:<br>
I have explored more about Postman. This tool allows me to properly test specific endpoints related to the project by creating HTTP requests to the server with different methods (GET, POST, DELETE, etc.) and allowing me to view the results or responses to the requests that were given. This is very helpful in making sure that the behavior in the endpoints that were accessed are exactly what we intended them to be.

#### Reflection Publisher-3

1. Observer Pattern has two variations: Push model (publisher pushes data to subscribers) and Pull model (subscribers pull data from publisher). In this tutorial case, which variation of Observer Pattern that we use?<br>
Answer:<br>
In this tutorial case, the variation of Observer Pattern that we use is the Push model variation where the publisher pushes data to the subcribers. Whenever a product is created, deleted, or published/promoted, the system will trigger the NotificationService and notifies each subscriber for that product's type by pushing the notification data to them.

2. What are the advantages and disadvantages of using the other variation of Observer Pattern for this tutorial case? (example: if you answer Q1 with Push, then imagine if we used Pull)<br>
Answer:<br>
The advantages of using the other variation of Observer Pattern, which is the Pull model variation, for this tutorial case are that subscribers can decide when to request the data that they need and they can also decide on what data they need to fetch. The disadvantage of using the Pull model variation is that subscribers must actively handle retrieving the needed data themselves, which adds complexity and makes the process more inconvenient for them.

3. Explain what will happen to the program if we decide to not use multi-threading in the notification process.<br>
Answer:<br>
If we decide not to use multi-threading in the notification process, then each notifications will be sent one-by-one utilizing single-threading. This will result in the notifying process becoming very slow, any issues that might result in a single notification taking a significantly long amount of time to send could end up delaying the sending process for every subsequent notification that comes after it. There's also a chance that the system's server might not be able to handle a large amount of notifications in an effective and efficient manner, leading to possible crashes or timeouts.