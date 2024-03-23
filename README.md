# Advanced Programming Even 2023/2024 - Module 6

## Commit 1 Reflection Notes

Secara garis besar, fungsi `handle_connection` akan mencetak HTTP headers dari setiap request yang diterima server. Fungsi `handle_connection` akan menerima parameter yaitu `stream`, sebuah objek `TcpStream` yang mutable. Kemudian fungsi tersebut membuat sebuah buffered reader `buf_reader` yang membaca data dari `stream` dengan menggunakan mutable reference pada variabel aslinya. Dari `buf_reader` yang sekarang isinya adalah data dari `stream`, dibentuk variabel `http_request` dengan tipe `Vec` (vector). `http_request` mengolah values pada `buf_reader` sebagai berikut:

- `.lines()`: Dari `buf_reader`, dibuat sebuah iterator yang isinya adalah setiap line dari `buf_reader` dalam bentuk string.
- `.map(|result| result.unwrap())`: Dari iterator yang sudah ada, dibuat sebuah mapping atau transformasi dengan `result` seolah-olah sebagai variabel sementara dari setiap elemen iterator. Result ini memiliki tipe `Result<String, Error>`. Dari setiap `result` akan dilakukan `.unwrap()`, yaitu memeriksa apakah setiap elemen tidak menimbulkan error. Jika tidak error maka elemen String dari tiap iterator akan di-return, tetapi jika error maka program akan selesai.
- `.take_while(|line| !line.is_empty())`: Dari hasil transformasi tersebut, akan dilakukan kembali pemrosesan, tetapi hanya terbatas hingga elemen yang diiterasikan kosong (`!line.is_empty()` terpenuhi). Pemrosesan ini akan mengambil referensi dari String yang tersimpan pada `line`.
- `collect()`: Dari pemrosesan iterator yang sudah dilakukan, semua hasilnya dibentuk menjadi sebuah collection, dalam kasus ini menjadi `Vec<String>`.

`http_request` yang telah terdefinisi akan dicetak dengan `println!()`. Format string yang dipakai adalah `{:#?}`, yaitu mencetak tiap elemen dari `http_request`. Format `#` digunakan agar setiap elemen mencetaknya per baris, meningkatkan readability.

## Commit 2 Reflection Notes

Pada `response`, ditambahkan status string dan juga `Content-Length`. Ini kemudian dituliskan pada response yang isinya HTML sebelum dituliskan pada stream. Ini sebenarnya hanya menambahkan HTTP response headers ke respon HTML yang akan diberikan, yaitu status dari request dan juga panjang content. Browser tidak akan me-render bagian ini dan hanya me-render content. Akan tetapi, response headers dapat suatu saat digunakan untuk debug apabila memang diperlukan.

![](/assets/img/commit2.png)
_Gambar URL yang diakses setelah_ `cargo run`
