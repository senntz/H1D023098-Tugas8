## Pengenalan

Folder `lib` merupakan folder utama yang berisi kode sumber aplikasi TokoKita. Aplikasi ini adalah aplikasi manajemen produk dengan fitur autentikasi (login dan registrasi) yang dibangun menggunakan Flutter.

## Model

### 1. model/login.dart

**Fungsi Utama:**
Model class untuk menampung data response dari API login.

**Properti:**
- `code` (int?) - Kode status response dari server
- `status` (bool?) - Status berhasil/gagal login
- `token` (String?) - Token autentikasi untuk session
- `userID` (int?) - ID unik pengguna
- `userEmail` (String?) - Email pengguna yang login

**Method:**
- `fromJson()` - Factory constructor untuk konversi data JSON dari API menjadi object Login
  - Mengecek kode response (200 = sukses)
  - Jika sukses, parse semua data termasuk token dan user info
  - Jika gagal, hanya parse code dan status

**Contoh Penggunaan:**
```dart
// Response sukses dengan kode 200
Login loginData = Login.fromJson({
  'code': 200,
  'status': true,
  'data': {
    'token': 'abc123xyz',
    'user': {
      'id': '1',
      'email': 'user@example.com'
    }
  }
});

// Response gagal
Login loginFailed = Login.fromJson({
  'code': 401,
  'status': false
});
```

---

### 2. model/produk.dart

**Fungsi Utama:**
Model class untuk merepresentasikan data produk dalam aplikasi.

**Properti:**
- `id` (String?) - ID unik produk
- `kodeProduk` (String?) - Kode identifikasi produk (contoh: A001)
- `namaProduk` (String?) - Nama produk
- `hargaProduk` (dynamic) - Harga produk (bisa int atau double)

**Method:**
- `fromJson()` - Factory constructor untuk mapping data JSON dari API ke object Produk
  - Mengkonversi key snake_case (kode_produk, nama_produk) dari API menjadi camelCase di Dart
  - Field 'harga' dari API di-mapping ke 'hargaProduk'

**Contoh Penggunaan:**
```dart
Produk produk = Produk.fromJson({
  'id': '1',
  'kode_produk': 'A001',
  'nama_produk': 'Kamera',
  'harga': 5000000
});

print(produk.namaProduk); // Output: Kamera
```

---

### 3. model/registrasi.dart

**Fungsi Utama:**
Model class untuk menangani response dari proses registrasi pengguna baru.

**Properti:**
- `code` (int?) - Kode status response dari server
- `status` (bool?) - Status berhasil/gagal registrasi
- `data` (String?) - Pesan atau informasi tambahan dari server

**Method:**
- `fromJson()` - Factory constructor sederhana untuk parsing response registrasi dari API
  - Mengkonversi ketiga field dari JSON menjadi object Registrasi

**Contoh Penggunaan:**
```dart
Registrasi result = Registrasi.fromJson({
  'code': 200,
  'status': true,
  'data': 'Registrasi berhasil'
});
```

---

## UI (User Interface)

### 1. ui/login_page.dart

<img width="144" height="308" alt="Login" src="https://github.com/user-attachments/assets/f2f615cf-89ef-4de8-b29a-1719b539bd6f" />
<img width="144" height="308" alt="Login-filled" src="https://github.com/user-attachments/assets/7a8fdc88-44bf-4ef1-b65f-cb572c95a046" />

**Fungsi Utama:**
Halaman untuk autentikasi pengguna masuk ke aplikasi.

**State Management:**
- `_formKey` - GlobalKey untuk validasi form
- `_isLoading` - Boolean untuk status loading
- `_emailTextboxController` - Controller untuk input email
- `_passwordTextboxController` - Controller untuk input password

**Widget Components:**

#### `_emailTextField()`
- Input field untuk email pengguna
- Validasi: email harus diisi (tidak boleh kosong)
- Keyboard type: emailAddress

#### `_passwordTextField()`
- Input field untuk password
- Property `obscureText: true` untuk menyembunyikan karakter password
- Validasi: password harus diisi

#### `_buttonLogin()`
- Tombol untuk submit form login
- Melakukan validasi form sebelum proses login
- Saat ditekan akan mengecek `_formKey.currentState!.validate()`

#### `_menuRegistrasi()`
- Link text "Registrasi" dengan styling biru
- Navigasi ke halaman RegistrasiPage menggunakan `Navigator.push()`
- Menggunakan widget `InkWell` untuk tap gesture

---

### 2. ui/registrasi_page.dart

<img width="144" height="308" alt="image" src="https://github.com/user-attachments/assets/9e77193f-4609-42cf-ada7-efa86dd71298" />
<img width="144" height="308" alt="image" src="https://github.com/user-attachments/assets/81e158c0-ffd3-4646-addd-d7e55d5e8b1b" />

**Fungsi Utama:**
Halaman untuk mendaftarkan pengguna baru ke dalam sistem.

**State Management:**
- `_formKey` - GlobalKey untuk validasi form
- `_isLoading` - Boolean status loading
- `_namaTextboxController` - Controller input nama
- `_emailTextboxController` - Controller input email
- `_passwordTextboxController` - Controller input password

**Widget Components:**

#### `_namaTextField()`
- Input field untuk nama lengkap pengguna
- Validasi: minimal 3 karakter
- Pesan error: "Nama harus diisi minimal 3 karakter"

#### `_emailTextField()`
- Input field untuk email
- Validasi ganda:
  1. Tidak boleh kosong
  2. Format email harus valid (menggunakan RegEx pattern)
- Pattern regex untuk validasi format email standar
- Pesan error: "Email tidak valid" jika format salah

#### `_passwordTextField()`
- Input field untuk password dengan `obscureText: true`
- Validasi: minimal 6 karakter
- Pesan error: "Password harus diisi minimal 6 karakter"

#### `_passwordKonfirmasiTextField()`
- Input field untuk konfirmasi password
- Validasi: harus sama dengan password yang diinput sebelumnya
- Membandingkan value dengan `_passwordTextboxController.text`
- Pesan error: "Konfirmasi Password tidak sama"

#### `_buttonRegistrasi()`
- Tombol submit untuk proses registrasi
- Validasi semua field sebelum proses
- Set `_isLoading = true` saat form valid

**Fitur:**
- Validasi form lengkap (nama, email, password, konfirmasi password)
- Validasi format email dengan RegEx
- Validasi kecocokan password
- AppBar dengan judul "Registrasi Fikri"

---

### 3. ui/produk_page.dart

<img width="144" height="308" alt="image" src="https://github.com/user-attachments/assets/8f84a769-9f42-4375-b90b-e5f25a62a7f7" />

**Fungsi Utama:**
Halaman utama yang menampilkan daftar produk dalam bentuk list.

**Komponen Utama:**

#### AppBar Actions
- Icon tambah (+) di pojok kanan atas
- Navigasi ke `ProdukForm()` untuk menambah produk baru
- Menggunakan `GestureDetector` dengan size icon 26.0

#### Drawer (Side Menu)
- Menu drawer dengan list item
- Item "Logout" dengan icon logout
- Siap untuk implementasi fungsi logout

#### Body - ListView
Menampilkan 3 produk hardcoded sebagai contoh data:
1. **Kamera** - Kode: A001, Harga: 5.000.000
2. **Kulkas** - Kode: A002, Harga: 2.500.000
3. **Mesin Cuci** - Kode: A003, Harga: 2.000.000

**Widget ItemProduk:**
- StatelessWidget terpisah untuk menampilkan item produk
- Menggunakan `Card` dan `ListTile`
- Property:
  - `title`: Nama produk
  - `subtitle`: Harga produk
- `GestureDetector` untuk navigasi ke `ProdukDetail`
- Mengirim object produk ke halaman detail

---

### 4. ui/produk_form.dart

<img width="144" height="308" alt="image" src="https://github.com/user-attachments/assets/4a71c61e-89b5-4bd1-98b3-a22481f679b4" />
<img width="144" height="308" alt="image" src="https://github.com/user-attachments/assets/92138fb2-cdc9-4f01-9dbc-20be4b093437" />

**Fungsi Utama:**
Halaman form untuk menambah produk baru atau mengedit produk yang sudah ada. Form ini bersifat dinamis tergantung mode (tambah/edit).

**State Management:**
- `_formKey` - GlobalKey untuk validasi form
- `_isLoading` - Boolean status loading
- `judul` - String judul halaman (dinamis)
- `tombolSubmit` - String label tombol (dinamis)
- `_kodeProdukTextboxController` - Controller input kode produk
- `_namaProdukTextboxController` - Controller input nama produk
- `_hargaProdukTextboxController` - Controller input harga

**Method Penting:**

#### `initState()`
- Lifecycle method yang dipanggil saat widget pertama kali dibuat
- Memanggil method `isUpdate()` untuk cek mode form

#### `isUpdate()`
Method untuk mendeteksi mode form (tambah atau edit):
- **Mode Edit** (jika `widget.produk != null`):
  - Judul: "UBAH PRODUK FIKRI"
  - Tombol: "UBAH"
  - Pre-fill semua textfield dengan data produk yang akan diedit
- **Mode Tambah** (jika `widget.produk == null`):
  - Judul: "TAMBAH PRODUK FIKRI"
  - Tombol: "SIMPAN"
  - Textfield kosong

**Widget Components:**

#### `_kodeProdukTextField()`
- Input untuk kode produk (contoh: A001, A002)
- Keyboard type: text
- Validasi: tidak boleh kosong
- Pesan error: "Kode Produk harus diisi"

#### `_namaProdukTextField()`
- Input untuk nama produk
- Keyboard type: text
- Validasi: tidak boleh kosong
- Pesan error: "Nama Produk harus diisi"

#### `_hargaProdukTextField()`
- Input untuk harga produk
- Keyboard type: number (angka saja)
- Validasi: tidak boleh kosong
- Pesan error: "Harga harus diisi"

#### `_buttonSubmit()`
- Tombol dengan label dinamis ("SIMPAN" atau "UBAH")
- Melakukan validasi form saat ditekan
- Menggunakan `OutlinedButton` style

---

### 5. ui/produk_detail.dart

<img width="144" height="308" alt="image" src="https://github.com/user-attachments/assets/74816ffe-d9d8-4a04-b3cd-4fa8b5c53c31" />
<img width="144" height="308" alt="image" src="https://github.com/user-attachments/assets/adee8972-36ba-4a5b-98f4-c06b5c569ccb" />
<img width="144" height="308" alt="image" src="https://github.com/user-attachments/assets/2eb800e7-a163-4de8-8add-161d8de1dfb7" />

**Fungsi Utama:**
Halaman untuk menampilkan detail lengkap dari satu produk dan menyediakan aksi edit atau hapus.

**Properti:**
- `produk` (Produk?) - Object produk yang akan ditampilkan detailnya

**Widget Components:**

#### Body Content
Menampilkan informasi produk dalam Column:
- **Kode**: Menampilkan kode produk (font size 20.0)
- **Nama**: Menampilkan nama produk (font size 18.0)
- **Harga**: Menampilkan harga dengan format "Rp. " (font size 18.0)

#### `_tombolHapusEdit()`
Row berisi dua tombol aksi:

**1. Tombol EDIT:**
- Style: `OutlinedButton`
- Fungsi: Navigasi ke `ProdukForm` dengan mengirim data produk
- Passing object `widget.produk!` agar form ter-populate dengan data

**2. Tombol DELETE:**
- Style: `OutlinedButton`
- Fungsi: Memanggil method `confirmHapus()`
- Menampilkan dialog konfirmasi sebelum menghapus

#### `confirmHapus()`
Method untuk menampilkan dialog konfirmasi hapus:

**Dialog Content:**
- Pesan: "Yakin ingin menghapus data ini?"

**Action Buttons:**
1. **Tombol "Ya":**
   - Kode untuk delete masih dikomentari
   - Seharusnya memanggil `ProdukBloc.deleteProduk()`
   - Navigasi kembali ke `ProdukPage` setelah berhasil
   - Error handling dengan `WarningDialog`

2. **Tombol "Batal":**
   - Menutup dialog dengan `Navigator.pop(context)`
   - Membatalkan proses hapus
