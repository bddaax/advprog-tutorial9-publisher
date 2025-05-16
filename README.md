# Modul 9 Pemrograman Lanjut : Software Architectures
oleh **Brenda Po Lok Fahida**

<br>
<br>



> Berapa banyak data yang dikirimkan oleh program publisher ke message broker dalam satu kali eksekusi?

Berdasarkan kode dalam `main.rs`:
    ```rs
    fn main() {
        let mut p = CrosstownBus::new_queue_publisher("amqp://guest:guest@localhost:5672".to_owned()).unwrap();
        _ = p.publish_event("user_created".to_owned(), UserCreatedEventMessage {
        user_id: "1".to_owned(), user_name: "2306152304-Amir".to_owned() });
        _ = p.publish_event("user_created".to_owned(), UserCreatedEventMessage {
        user_id: "2".to_owned(), user_name: "2306152304-Budi".to_owned() });
        _ = p.publish_event("user_created".to_owned(), UserCreatedEventMessage {
        user_id: "3".to_owned(), user_name: "2306152304-Cica".to_owned() });
        _ = p.publish_event("user_created".to_owned(), UserCreatedEventMessage {
        user_id: "4".to_owned(), user_name: "2306152304-Dira".to_owned() });
        _ = p.publish_event("user_created".to_owned(), UserCreatedEventMessage {
        user_id: "5".to_owned(), user_name: "2306152304-Emir".to_owned() });
    }
    ```

Pada satu kali eksekusi, program publisher akan mengirimkan **5 pesan** ke message broker. Setiap pesan adalah objek `UserCreatedEventMessage` yang berisi dua field bertipe `String`, yaitu `user_id` dan `user_name`. 


> Url “amqp\://guest\:guest\@localhost:5672” sama seperti di program subscriber. Apa artinya?

Url `amqp://guest:guest@localhost:5672` menunjukkan alamat koneksi ke message broker, yaitu Rabbitmq, dan penggunaannya yang sama di publisher dan subscriber menunjukkan bahwa **keduanya terhubung ke broker yang sama**. Publisher mengirim pesan ke broker di alamat tersebut, dan subscriber mendengarkan pesan dari broker di alamat yang sama. Hal ini menunjukkan bahwa sistem komunikasi antar aplikasi dilakukan melalui **satu perantara yang sama**, yaitu message broker yang berjalan di `localhost:5672`, dan keduanya menggunakan kredensial yang sama untuk autentikasi. Dengan cara ini, publisher dan subscriber bisa saling berkomunikasi meskipun berjalan sebagai program yang terpisah.

## Message Broker dengan RabbitMQ
> Running RabbitMQ as message broker

<img src="image/image_1.png">

> Sending and Processing Event

<img src="image/image_2.png">

Pada gambar di atas, terlihat proses komunikasi antara aplikasi publisher dan subscriber menggunakan RabbitMQ sebagai message broker. Saat program publisher dijalankan, ia mengirimkan lima event bertipe UserCreatedEventMessage ke RabbitMQ. Event-event tersebut kemudian diterima oleh aplikasi subscriber yang berjalan secara paralel. Setiap event yang diterima ditampilkan di konsol subscriber sebagai output, menunjukkan bahwa pesan berhasil dikonsumsi dan diproses. Proses ini menunjukkan implementasi sederhana dari pola komunikasi berbasis event-driven.
