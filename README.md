# eshop_pbp

A new Flutter project.

## Getting Started

This project is a starting point for a Flutter application.

A few resources to get you started if this is your first Flutter project:

- [Lab: Write your first Flutter app](https://docs.flutter.dev/get-started/codelab)
- [Cookbook: Useful Flutter samples](https://docs.flutter.dev/cookbook)

For help getting started with Flutter development, view the
[online documentation](https://docs.flutter.dev/), which offers tutorials,
samples, guidance on mobile development, and a full API reference.

# TUGAS 7

### Jelaskan apa yang dimaksud dengan stateless widget dan stateful widget, dan jelaskan perbedaan dari keduanya.

Stateless widget adalah widget yang bersifat statis/tidak berubah (immutable), sedangkan stateful widget adalah widget yang memiliki sifat dinamis dan dapat berubah (mutable).

Perbedaan lebih lanjut mengenai stateless dan stateful widget:
* Stateless widget
  * Tidak memiliki state internal yang bisa berubah selama widget hidup
  * Cocok untuk UI yang tidak perlu __update__ (seperti text, icon, atau layout statis)
  * Lebih ringan karena tidak perlu meng--__handle__ perubahan state
  * Contoh: `Text`, `Icon`, `Container` dengan properti tetap
* Stateful widget
  * Memiliki state internal yang bisa diupdate
  * Cocok untuk UI yang interaktif dan perlu __update__
  * Menggunakan `setState()` untuk memperbarui UI
  * Contoh: `Form`, `Counter`, `Checkbox`

### Sebutkan widget apa saja yang kamu gunakan pada proyek ini dan jelaskan fungsinya.

* `MaterialApp`: Widget root yang menyediakan konfigurasi material design
* `Scaffold`: Menyediakan struktur dasar halaman dengan AppBar dan body
* `AppBar`: Menampilkan bar atas aplikasi.
* `Column`: Mengatur children secara vertikal
* `Row`: Mengatur children secara horizontal
* `Center`: Memposisikan widget di tengah
* `Padding`: Memberikan padding pada widget child
* `Container`: Membungkus widget dengan customizable properties
* `GridView`: Menampilkan items dalam bentuk grid
* `SizedBox`: Memberikan space kosong dengan ukuran tertentu
* `Text`: Menampilkan text (judul, konten, dll)
* `Icon`: Menampilkan icons
* `Card`: Container dengan style Material Design
* `Material`: Memberikan visual Material Design pada ItemCard
* `InkWell`: Memberikan efek ripple dan handling tap pada ItemCard
* `MyApp`: Widget root aplikasi
* `MyHomePage`: Halaman utama aplikasi
* `InfoCard`: Card untuk menampilkan informasi (NPM, Name, Class)
* `ItemCard`: Card untuk menampilkan menu item dengan icon
* `ItemHomepage`: Class untuk menyimpan data item (bukan widget)
* `SnackBar`: Menampilkan pesan feedback ketika ItemCard ditekan
* `ScaffoldMessenger`: Mengatur tampilan SnackBar

### Apa fungsi dari setState()? Jelaskan variabel apa saja yang dapat terdampak dengan fungsi tersebut.
* `setState()` digunakan untuk memberitahu framework bahwa state internal widget telah berubah
* Ketika dipanggil, framework akan menjadwalkan rebuild widget tersebut
* Hanya dapat digunakan di Stateful Widget
* Contoh implementasi serta variabel yang terdampak

```dart
class _MyHomePageState extends State<MyHomePage> {
  int _counter = 0;  // Variabel state
  String _text = ""; // Variabel state
  
  void _incrementCounter() {
    setState(() {
      _counter++;    // Variabel ini akan memicu rebuild
      _text = "New"; // Variabel ini juga akan memicu rebuild
    });
  }
}
```

### Jelaskan perbedaan antara const dengan final.
* `const`:
  * Harus diinisiasi pada saat __compile-time__
  * Nilai harus diketahui sebelum program dijalankan
  * Tidak dapat diubah sama sekali
* `final`:
  * Dapat diinisialisasi pada __runtime__
  * Setelah diinisialisasi tidak dapat diubah (__write-once__)
  * Lebih fleksibel dibanding const

Contoh implementasi:
```dart
const int x = 10; // Valid
const int y = DateTime.now().year; // Invalid, karena nilai ditentukan saat runtime
final int x = 10; // Valid
final int y = DateTime.now().year; // Valid
final String api = fetchFromServer(); // Valid
```

Perbedaan utama adalah const memerlukan nilai yang sudah diketahui saat compile-time, sementara final bisa diinisialisasi dengan nilai yang ditentukan saat runtime, tapi setelah diset tidak bisa diubah lagi.

### Jelaskan bagaimana cara kamu mengimplementasikan checklist-checklist di atas.

1. Membuat tiga tombol dengan ikon dan teks:

```dart
// Membuat class ItemHomepage untuk menyimpan data tombol
class ItemHomepage {
  final String name;
  final IconData icon;

  ItemHomepage(this.name, this.icon);
}

// 2. Mendefinisikan data tombol dalam Map
final Map<ItemHomepage, ColorScheme> colorCardMap = {
  ItemHomepage("Lihat Daftar Produk", Icons.panorama_fish_eye): // Tombol 1
      ColorScheme.fromSwatch(primarySwatch: Colors.red),
  ItemHomepage("Tambah Produk", Icons.add): // Tombol 2
      ColorScheme.fromSwatch(primarySwatch: Colors.blue),
  ItemHomepage("Logout", Icons.logout): // Tombol 3
      ColorScheme.fromSwatch(primarySwatch: Colors.yellow)
};
```

2. Mengimplementasikan warna berbeda untuk setiap tombol:

```dart
// Menggunakan ColorScheme yang berbeda untuk setiap tombol dalam colorCardMap
ColorScheme.fromSwatch(primarySwatch: Colors.red),    // Merah untuk Lihat Daftar Produk
ColorScheme.fromSwatch(primarySwatch: Colors.blue),   // Biru untuk Tambah Produk
ColorScheme.fromSwatch(primarySwatch: Colors.yellow)  // Kuning untuk Logout

// Warna diterapkan dalam ItemCard widget
class ItemCard extends StatelessWidget {
  final ColorScheme colorScheme;
  ...
  return Material(
    color: colorScheme.secondary, // Menggunakan warna dari ColorScheme
    ...
  );
}
```

3. Implementasi Snackbar:

```dart
// Dalam class ItemCard, menambahkan onTap handler yang menampilkan Snackbar
child: InkWell(
  onTap: () {
    // Menampilkan Snackbar dengan pesan yang sesuai
    ScaffoldMessenger.of(context)
      ..hideCurrentSnackBar()  // Menyembunyikan Snackbar yang sedang tampil
      ..showSnackBar(SnackBar(
          content: Text("Kamu telah menekan tombol ${item.name}!")
      ));
  },
  ...
)
```

4. Menampilkan tombol dalam GridView:

```dart
GridView.count(
  crossAxisCount: 3,  // Menampilkan 3 tombol per baris
  children: colorCardMap.entries.map((entry) {
    return ItemCard(entry.key, entry.value);  // Membuat ItemCard untuk setiap tombol
  }).toList()
)
```


