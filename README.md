# Screenshot
<img width="386" height="827" alt="Registrasi" src="https://github.com/user-attachments/assets/fcde7b1b-f4b1-4891-af13-707219be9477" />
<img width="386" height="827" alt="Login" src="https://github.com/user-attachments/assets/60092961-1eb3-4e31-9226-68e577d7a4d4" />
<img width="386" height="827" alt="List" src="https://github.com/user-attachments/assets/71c53328-f5c7-4a21-9280-ea26f9bd83b0" />
<img width="386" height="827" alt="Detail" src="https://github.com/user-attachments/assets/654af922-9e7e-4048-bc04-9f135c80ef5c" />
<img width="386" height="827" alt="Tambah" src="https://github.com/user-attachments/assets/1f5f2548-d646-451d-9bd8-6790460e4923" />
<img width="386" height="827" alt="Edit" src="https://github.com/user-attachments/assets/461b4e2e-d219-40b6-8709-134832aa39c2" />

# Penjelasan

## model/login.dart
Fungsi Utama: Model class untuk menampung data response dari API login. <br>
Properti:
* `code (int?)` - Kode status response dari server
* `status (bool?)` - Status berhasil/gagal login
* `token (String?)` - Token autentikasi untuk session
* `userID (int?)` - ID unik pengguna
* `userEmail (String?)` - Email pengguna yang login

Method:

`fromJson()` - Factory constructor untuk konversi data JSON dari API menjadi object Login

Mengecek kode response (200 = sukses)
Jika sukses, parse semua data termasuk token dan user info
Jika gagal, hanya parse code dan status

## model/produk.dart
Fungsi Utama: Model class untuk merepresentasikan data produk dalam aplikasi. <br>
Properti:

* `id (String?)` - ID unik produk
* `kodeProduk (String?)` - Kode identifikasi produk (contoh: A001)
* `namaProduk (String?)` - Nama produk
* `hargaProduk (dynamic)` - Harga produk (bisa int atau double)

Method:

`fromJson()` - Factory constructor untuk mapping data JSON dari API ke object Produk

Mengkonversi key snake_case (kode_produk, nama_produk) dari API menjadi camelCase di Dart <br>
Field 'harga' dari API di-mapping ke 'hargaProduk'

## model/registrasi.dart
Fungsi Utama: Model class untuk menangani response dari proses registrasi pengguna baru. <br>
Properti:

* `code (int?)` - Kode status response dari server
* `status (bool?)` - Status berhasil/gagal registrasi
* `data (String?)` - Pesan atau informasi tambahan dari server

Method:

`fromJson()` - Factory constructor sederhana untuk parsing response registrasi dari API

Mengkonversi ketiga field dari JSON menjadi object Registrasi

## ui/login_page.dart
Fungsi Utama: Halaman untuk autentikasi pengguna masuk ke aplikasi. <br>
State Management:

* `_formKey` - GlobalKey untuk validasi form
* `_isLoading` - Boolean untuk status loading
* `_emailTextboxController` - Controller untuk input email
* `_passwordTextboxController` - Controller untuk input password

Widget Components:
`_emailTextField()`

Input field untuk email pengguna <br>
Validasi: email harus diisi (tidak boleh kosong)

`_passwordTextField()`

Input field untuk password <br>
Property obscureText: true untuk menyembunyikan karakter password <br>
Validasi: password harus diisi

`_buttonLogin()`

Tombol untuk submit form login <br>
Melakukan validasi form sebelum proses login <br>
Saat ditekan akan mengecek `_formKey.currentState!.validate()` <br>

`_menuRegistrasi()`

Navigasi ke halaman RegistrasiPage menggunakan `Navigator.push()` <br>
Menggunakan widget InkWell untuk tap gesture <br>

Fitur: <br>

* Form validation untuk email dan password
* Navigasi ke halaman registrasi
* AppBar dengan judul "Login Fikri"

## ui/registrasi_page.dart
Fungsi Utama: Halaman untuk mendaftarkan pengguna baru ke dalam sistem. <br>
State Management:

* `_formKey` - GlobalKey untuk validasi form
* `_isLoading` - Boolean status loading
* `_namaTextboxController` - Controller input nama
* `_emailTextboxController` - Controller input email
* `_passwordTextboxController` - Controller input password

Widget Components:
`_namaTextField()`

Input field untuk nama lengkap pengguna <br>
Validasi: minimal 3 karakter <br>
Pesan error: "Nama harus diisi minimal 3 karakter"

`_emailTextField()`

Input field untuk email <br>
Validasi ganda: <br>

* Tidak boleh kosong
* Format email harus valid (menggunakan RegEx pattern)


Pattern regex untuk validasi format email standar <br>
Pesan error: "Email tidak valid" jika format salah

`_passwordTextField()`

Input field untuk password dengan obscureText: true <br>
Validasi: minimal 6 karakter <br>
Pesan error: "Password harus diisi minimal 6 karakter"

`_passwordKonfirmasiTextField()`

Input field untuk konfirmasi password <br>
Validasi: harus sama dengan password yang diinput sebelumnya <br>
Membandingkan value dengan `_passwordTextboxController.text` <br>
Pesan error: "Konfirmasi Password tidak sama"

`_buttonRegistrasi()`

Tombol submit untuk proses registrasi <br>
Validasi semua field sebelum proses <br>
Set `_isLoading` = true saat form valid

Fitur: <br>

* Validasi form lengkap (nama, email, password, konfirmasi password)
* Validasi format email dengan RegEx
* Validasi kecocokan password
* AppBar dengan judul "Registrasi Fikri"

## ui/produk_page.dart
Fungsi Utama: Halaman utama yang menampilkan daftar produk dalam bentuk list. <br>
Komponen Utama:
* AppBar Actions

Icon tambah (+) di pojok kanan atas <br>
Navigasi ke ProdukForm() untuk menambah produk baru <br>

* Drawer (Side Menu)

Menu drawer dengan list item

* Body - ListView
Menampilkan 3 produk hardcoded sebagai contoh data:

* Kamera - Kode: A001, Harga: 5.000.000
* Kulkas - Kode: A002, Harga: 2.500.000
* Mesin Cuci - Kode: A003, Harga: 2.000.000

Widget ItemProduk: <br>

StatelessWidget terpisah untuk menampilkan item produk <br>
Menggunakan `Card` dan `ListTile` <br>
Property:
* title: Nama produk
* subtitle: Harga produk

`GestureDetector` untuk navigasi ke `ProdukDetail` <br>
Mengirim object produk ke halaman detail <br>

Fitur:

* List view produk
* Navigasi ke form tambah produk
* Navigasi ke detail produk saat item di-tap
* Drawer menu dengan opsi logout
* AppBar dengan judul "List Produk Fikri"

## ui/produk_form.dart
Fungsi Utama: Halaman form untuk menambah produk baru atau mengedit produk yang sudah ada. Form ini bersifat dinamis tergantung mode (tambah/edit). <br>
State Management:

* `_formKey` - GlobalKey untuk validasi form
* `_isLoading` - Boolean status loading
* `judul` - String judul halaman (dinamis)
* `tombolSubmit` - String label tombol (dinamis)
* `_kodeProdukTextboxController` - Controller input kode produk
* `_namaProdukTextboxController` - Controller input nama produk
* `_hargaProdukTextboxController` - Controller input harga

Method Penting:
`initState()`

Lifecycle method yang dipanggil saat widget pertama kali dibuat <br>
Memanggil method `isUpdate()` untuk cek mode form <br>

`isUpdate()`
Method untuk mendeteksi mode form (tambah atau edit):

Mode Edit (jika `widget.produk != null`):
* Judul: "UBAH PRODUK FIKRI"
* Tombol: "UBAH"
* Pre-fill semua textfield dengan data produk yang akan diedit

Mode Tambah (jika `widget.produk == null`):
* Judul: "TAMBAH PRODUK FIKRI"
* Tombol: "SIMPAN"
* Textfield kosong

Widget Components:
`_kodeProdukTextField()`

Input untuk kode produk (contoh: A001, A002) <br>
Keyboard type: text <br>
Validasi: tidak boleh kosong <br>
Pesan error: "Kode Produk harus diisi" <br>

`_namaProdukTextField()`

Input untuk nama produk <br>
Keyboard type: text <br>
Validasi: tidak boleh kosong <br>
Pesan error: "Nama Produk harus diisi" <br>

`_hargaProdukTextField()`

* Input untuk harga produk
* Keyboard type: number (angka saja)
* Validasi: tidak boleh kosong
* Pesan error: "Harga harus diisi"

`_buttonSubmit()`

Tombol dengan label dinamis ("SIMPAN" atau "UBAH") <br>
Melakukan validasi form saat ditekan <br>
Menggunakan `OutlinedButton` style <br>

Fitur:
* Form dinamis untuk tambah dan edit produk
* Auto-fill data saat mode edit
* Validasi semua input field
* AppBar dengan judul dinamis

## ui/produk_detail.dart
Fungsi Utama: Halaman untuk menampilkan detail lengkap dari satu produk dan menyediakan aksi edit atau hapus. <br>
Properti:
`produk (Produk?)` - Object produk yang akan ditampilkan detailnya <br>

Widget Components: <br>
Body Content <br>
Menampilkan informasi produk dalam Column:

* Kode: Menampilkan kode produk (font size 20.0)
* Nama: Menampilkan nama produk (font size 18.0)
* Harga: Menampilkan harga dengan format "Rp. " (font size 18.0)

`_tombolHapusEdit()` <br>
Row berisi dua tombol aksi:
1. Tombol EDIT:
* Style: `OutlinedButton`
* Fungsi: Navigasi ke `ProdukForm` dengan mengirim data produk
* Passing object `widget.produk!` agar form ter-populate dengan data

2. Tombol DELETE:
* Style: `OutlinedButton`
* Fungsi: Memanggil method `confirmHapus()`
* Menampilkan dialog konfirmasi sebelum menghapus

`confirmHapus()` <br>
Method untuk menampilkan dialog konfirmasi hapus: <br>
Dialog Content:
* Pesan: "Yakin ingin menghapus data ini?"
Action Buttons:
Tombol "Ya":
* Kode untuk delete masih dikomentari
* Seharusnya memanggil `ProdukBloc.deleteProduk()`
* Navigasi kembali ke `ProdukPage` setelah berhasil
* Error handling dengan `WarningDialog`

Tombol "Batal":
* Menutup dialog dengan `Navigator.pop(context)` 
* Membatalkan proses hapus

Fitur:
* Menampilkan detail lengkap produk
* Tombol edit ke form produk
* Dialog konfirmasi sebelum hapus
* AppBar dengan judul "Detail Produk Fikri"
* Center alignment untuk semua konten
