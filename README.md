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

- Stateless widget
  - Tidak memiliki state internal yang bisa berubah selama widget hidup
  - Cocok untuk UI yang tidak perlu **update** (seperti text, icon, atau layout statis)
  - Lebih ringan karena tidak perlu meng--**handle** perubahan state
  - Contoh: `Text`, `Icon`, `Container` dengan properti tetap
- Stateful widget
  - Memiliki state internal yang bisa diupdate
  - Cocok untuk UI yang interaktif dan perlu **update**
  - Menggunakan `setState()` untuk memperbarui UI
  - Contoh: `Form`, `Counter`, `Checkbox`

### Sebutkan widget apa saja yang kamu gunakan pada proyek ini dan jelaskan fungsinya.

- `MaterialApp`: Widget root yang menyediakan konfigurasi material design
- `Scaffold`: Menyediakan struktur dasar halaman dengan AppBar dan body
- `AppBar`: Menampilkan bar atas aplikasi.
- `Column`: Mengatur children secara vertikal
- `Row`: Mengatur children secara horizontal
- `Center`: Memposisikan widget di tengah
- `Padding`: Memberikan padding pada widget child
- `Container`: Membungkus widget dengan customizable properties
- `GridView`: Menampilkan items dalam bentuk grid
- `SizedBox`: Memberikan space kosong dengan ukuran tertentu
- `Text`: Menampilkan text (judul, konten, dll)
- `Icon`: Menampilkan icons
- `Card`: Container dengan style Material Design
- `Material`: Memberikan visual Material Design pada ItemCard
- `InkWell`: Memberikan efek ripple dan handling tap pada ItemCard
- `MyApp`: Widget root aplikasi
- `MyHomePage`: Halaman utama aplikasi
- `InfoCard`: Card untuk menampilkan informasi (NPM, Name, Class)
- `ItemCard`: Card untuk menampilkan menu item dengan icon
- `ItemHomepage`: Class untuk menyimpan data item (bukan widget)
- `SnackBar`: Menampilkan pesan feedback ketika ItemCard ditekan
- `ScaffoldMessenger`: Mengatur tampilan SnackBar

### Apa fungsi dari setState()? Jelaskan variabel apa saja yang dapat terdampak dengan fungsi tersebut.

- `setState()` digunakan untuk memberitahu framework bahwa state internal widget telah berubah
- Ketika dipanggil, framework akan menjadwalkan rebuild widget tersebut
- Hanya dapat digunakan di Stateful Widget
- Contoh implementasi serta variabel yang terdampak

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

- `const`:
  - Harus diinisiasi pada saat **compile-time**
  - Nilai harus diketahui sebelum program dijalankan
  - Tidak dapat diubah sama sekali
- `final`:
  - Dapat diinisialisasi pada **runtime**
  - Setelah diinisialisasi tidak dapat diubah (**write-once**)
  - Lebih fleksibel dibanding const

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

# TUGAS 8

### Apa kegunaan const di Flutter? Jelaskan apa keuntungan ketika menggunakan const pada kode Flutter. Kapan sebaiknya kita menggunakan const, dan kapan sebaiknya tidak digunakan?

- Kegunaan `const` di Flutter:

`const` di Flutter digunakan untuk mengoptimalkan performa dengan membuat widget immutable pada compile-time.
Keuntungan menggunakan `const`:

Meningkatkan performa karena widget `const` hanya dibuat sekali saat compile-time
Menghemat memory karena instance yang sama digunakan kembali
Membantu Flutter melakukan widget rebuilding lebih efisien

- Kapan menggunakan `const`:

Untuk widget yang tidak berubah (static)
Widget tanpa dependencies ke state yang berubah
Nilai yang diketahui saat compile-time

Contoh dari kode:

```dart
const LeftDrawer({super.key});
const Text(
  'Sutashop',
  style: TextStyle(...),
);
```

- Kapan tidak menggunakan `const`:

  - Widget yang bergantung pada state yang berubah
  - Widget yang menggunakan data dinamis
  - Nilai yang hanya diketahui saat runtime

### Jelaskan dan bandingkan penggunaan Column dan Row pada Flutter. Berikan contoh implementasi dari masing-masing layout widget ini!

Column dan Row adalah layout widgets yang mengatur children secara vertikal dan horizontal.

- Column: Mengatur widgets secara vertikal (atas ke bawah)

Contoh dari kode:

```dart
Column(
  children: [
    Text(title),
    const SizedBox(height: 8.0),
    Text(content)
  ]
)
```

- Row: Mengatur widgets secara horizontal (kiri ke kanan)
  Contoh dari kode:

```dart
Row(
  mainAxisAlignment: MainAxisAlignment.spaceEvenly,
  children: [
    InfoCard(title: 'NPM', content: npm),
    InfoCard(title: 'Name', content: name),
    InfoCard(title: 'Class', content: className),
  ],
)
```

### Sebutkan apa saja elemen input yang kamu gunakan pada halaman form yang kamu buat pada tugas kali ini. Apakah terdapat elemen input Flutter lain yang tidak kamu gunakan pada tugas ini? Jelaskan!

- Elemen input yang sudah digunakan dalam form:

  - TextFormField untuk nama produk
  - TextFormField untuk harga (dengan validasi numerik)
  - TextFormField untuk deskripsi
  - TextFormField untuk stok (dengan validasi numerik)

- Elemen input Flutter lain yang tidak digunakan:

  - Checkbox: Untuk pilihan boolean yang bisa dipilih multiple
  - Radio Button: Untuk pilihan yang mutually exclusive dari beberapa opsi
  - Dropdown: Untuk memilih satu opsi dari daftar yang panjang
  - Switch: Untuk toggle settings on/off
  - Slider: Untuk input numerik dalam range tertentu
  - DatePicker: Untuk memilih tanggal dengan UI kalender
  - TimePicker: Untuk memilih waktu dengan UI jam

### Bagaimana cara kamu mengatur tema (theme) dalam aplikasi Flutter agar aplikasi yang dibuat konsisten? Apakah kamu mengimplementasikan tema pada aplikasi yang kamu buat?

Dalam implementasi saya, tema diatur di `main.dart` menggunakan `ThemeData`:

```dart
theme: ThemeData(
  colorScheme: ColorScheme.fromSwatch(
    primarySwatch: Colors.deepPurple,
  ).copyWith(secondary: Colors.deepPurple[400]),
)
```

- Tema ini diimplementasikan di berbagai tempat:

  - AppBar menggunakan Theme.of(context).colorScheme.primary
  - Buttons menggunakan tema warna yang konsisten
  - Drawer menggunakan warna tema yang sama

### Bagaimana cara kamu menangani navigasi dalam aplikasi dengan banyak halaman pada Flutter?

Dalam aplikasi saya, navigasi ditangani dengan beberapa cara:

- Menggunakan Navigator.push untuk halaman baru:

```dart
Navigator.push(
  context,
  MaterialPageRoute(builder: (context) => const ProductFormPage())
);
```

- Menggunakan Navigator.pushReplacement untuk mengganti halaman:

```dart
Navigator.pushReplacement(
  context,
  MaterialPageRoute(builder: (context) => MyHomePage())
);
```

- Menggunakan Drawer untuk navigasi lateral:

```dart
ListTile(
  onTap: () {
    Navigator.pushReplacement(
      context,
      MaterialPageRoute(builder: (context) => MyHomePage())
    );
  },
)
```

# TUGAS 9

### Jelaskan mengapa kita perlu membuat model untuk melakukan pengambilan ataupun pengiriman data JSON? Apakah akan terjadi error jika kita tidak membuat model terlebih dahulu?

Model sangat penting dalam Flutter ketika bekerja dengan JSON karena:

- Type Safety: Dart adalah bahasa yang type-safe. Model membantu memastikan data memiliki struktur dan tipe yang benar
- Auto-completion: IDE dapat memberikan saran kode yang lebih baik
- Error Prevention: Mengurangi runtime errors karena struktur data sudah didefinisikan
- Code Maintenance: Lebih mudah memelihara dan mengubah kode

```dart
// Contoh tanpa model (rawan error):
var data = jsonDecode(response.body);
print(data['name']); // Bisa error jika 'name' tidak ada

// Dengan model (lebih aman):
class User {
  final String name;
  User({required this.name});

  factory User.fromJson(Map<String, dynamic> json) {
    return User(name: json['name']);
  }
}
```

### Jelaskan fungsi dari library http yang sudah kamu implementasikan pada tugas ini

Library http menyediakan fungsi-fungsi untuk melakukan HTTP requests:

- get(): Mengambil data dari server
- post(): Mengirim data ke server
- put(): Memperbarui data yang ada
- delete(): Menghapus data
- Handling headers dan cookies
- Error handling untuk network requests

```dart
import 'package:http/http.dart' as http;

// Contoh penggunaan
Future<void> fetchData() async {
  final response = await http.get(
    Uri.parse('https://your-api.com/data'),
    headers: {'Content-Type': 'application/json'},
  );
}
```

### Jelaskan fungsi dari CookieRequest dan jelaskan mengapa instance CookieRequest perlu untuk dibagikan ke semua komponen di aplikasi Flutter.

- CookieRequest berfungsi untuk:

  - Menyimpan dan mengelola session cookies
  - Menjaga state autentikasi
  - Mengirim cookies pada setiap request

Perlu dibagikan ke semua komponen karena:

- Memastikan konsistensi session di seluruh aplikasi
- Menghindari multiple login
- Efisiensi penggunaan memori
  State management yang lebih baik

```dart
// Menggunakan Provider untuk berbagi instance
void main() {
  runApp(
    Provider(
      create: (context) => CookieRequest(),
      child: MyApp(),
    ),
  );
}
```

### Jelaskan mekanisme pengiriman data mulai dari input hingga dapat ditampilkan pada Flutter.

```dart
// 1. Input data dari form
class MyForm extends StatefulWidget {
  final _formKey = GlobalKey<FormState>();
  String _inputData = '';

  // 2. Validasi dan persiapan data
  void _submitForm() async {
    if (_formKey.currentState!.validate()) {
      // 3. Konversi ke JSON
      final data = {'field': _inputData};

      // 4. Kirim ke server
      final response = await http.post(
        Uri.parse('your-api-url'),
        body: jsonEncode(data),
        headers: {'Content-Type': 'application/json'},
      );

      // 5. Process response
      if (response.statusCode == 200) {
        // 6. Update UI
        setState(() {
          // Update state untuk menampilkan data
        });
      }
    }
  }
}
```

### Jelaskan mekanisme autentikasi dari login, register, hingga logout. Mulai dari input data akun pada Flutter ke Django hingga selesainya proses autentikasi oleh Django dan tampilnya menu pada Flutter.

```dart
// Login Process
class LoginPage extends StatefulWidget {
  Future<void> login() async {
    // 1. Input validation
    if (_formKey.currentState!.validate()) {
      // 2. Prepare login data
      final data = {
        'username': _usernameController.text,
        'password': _passwordController.text,
      };

      // 3. Send to Django
      final response = await http.post(
        Uri.parse('${baseUrl}/auth/login/'),
        body: jsonEncode(data),
        headers: {'Content-Type': 'application/json'},
      );

      // 4. Process response
      if (response.statusCode == 200) {
        // 5. Save token/cookie
        final cookieRequest = context.read<CookieRequest>();
        await cookieRequest.saveToken(response.body);

        // 6. Navigate to home
        Navigator.pushReplacement(
          context,
          MaterialPageRoute(builder: (context) => HomePage()),
        );
      }
    }
  }
}

// Register Process
// Mirip dengan login, tapi dengan endpoint yang berbeda
// dan mungkin memerlukan data tambahan

// Logout Process
void logout() async {
  final cookieRequest = context.read<CookieRequest>();

  // 1. Send logout request to Django
  final response = await http.post(
    Uri.parse('${baseUrl}/auth/logout/'),
    headers: {
      'Authorization': 'Bearer ${cookieRequest.token}',
    },
  );

  // 2. Clear local token/cookie
  await cookieRequest.clearToken();

  // 3. Navigate to login
  Navigator.pushReplacement(
    context,
    MaterialPageRoute(builder: (context) => LoginPage()),
  );
}
```

Di sisi Django:

1. Menerima request login/register
2. Memvalidasi credentials
3. Membuat session/token
4. Mengirim response ke Flutter
5. Untuk logout, menghapus session/token

Proses ini memastikan:

- Keamanan data pengguna
- Session management yang baik
- Pengalaman pengguna yang mulus
- Pemisahan concerns antara client dan server

### Jelaskan bagaimana cara kamu mengimplementasikan checklist di atas secara step-by-step! (bukan hanya sekadar mengikuti tutorial).

1. Mengimplementasikan Fitur Registrasi

```dart
// Membuat register.dart dengan form registrasi
class RegisterPage extends StatefulWidget {
// Menggunakan TextFormField untuk input username dan password
// Implementasi validasi form
// Mengirim data registrasi ke Django endpoint /auth/register/
// Navigasi ke LoginPage setelah registrasi berhasil
}
```

2. Membuat Halaman Login

```dart
// Implementasi di login.dart
class LoginPage extends StatefulWidget {
// Form login dengan username & password fields
// Menggunakan CookieRequest untuk autentikasi
// Navigasi ke HomePage jika login berhasil
// Menampilkan error dialog jika gagal
}
```

3. Integrasi Autentikasi Django-Flutter

```dart
// Di main.dart: Setup Provider untuk CookieRequest
class MyApp extends StatelessWidget {
@override
Widget build(BuildContext context) {
return Provider(
create: (\_) {
CookieRequest request = CookieRequest();
return request;
},
child: MaterialApp(...)
);
}
}
```

4. Model Kustom untuk JSON

```dart
// Di models/product.dart: Model untuk data produk
class Product {
String model;
String pk;
Fields fields;

// Implementasi fromJson dan toJson
factory Product.fromJson(Map<String, dynamic> json) => Product(
model: json["model"],
pk: json["pk"],
fields: Fields.fromJson(json["fields"]),
);
}
```

5. Halaman Daftar Item (Endpoint JSON)

```dart
// Di list_product.dart
class ProductPage extends StatefulWidget {
Future<List<Product>> fetchProduct(CookieRequest request) async {
// Fetch data dari Django endpoint
final response = await request.get('http://127.0.0.1:8000/json/');
// Parse JSON ke model Product
List<Product> listProduct = [];
for (var d in data) {
listProduct.add(Product.fromJson(d));
}
return listProduct;
}

// Display menggunakan ListView.builder
}
```

6. Form Tambah Item

```dart
// Di product_form.dart
class ProductFormPage extends StatefulWidget {
// Form fields untuk nama, harga, deskripsi, stok
// Validasi input
// Kirim data ke Django endpoint
// Navigasi kembali ke daftar setelah berhasil
}
```

7. Left Drawer untuk Navigasi

```dart
// Di left_drawer.dart
class LeftDrawer extends StatelessWidget {
// Menu items:
// - Halaman Utama
// - Tambah Produk
// - Daftar Produk
// Implementasi navigasi untuk setiap menu
}
```

8. Implementasi Logout

```dart
// Di product.dart (ItemCard)
if (item.name == "Logout") {
final response = await request.logout("http://[URL]/auth/logout/");
// Handle response
// Navigasi ke LoginPage
Navigator.pushReplacement(
context,
MaterialPageRoute(builder: (context) => const LoginPage()),
);
}
```

9. Filter Item Berdasarkan User

```dart
// Di Django views.py
@csrf_exempt
def get_products(request):
products = Product.objects.filter(user=request.user)
return JsonResponse([...], safe=False)
```

10. Bonus Item Detail

```dart
// Di list*product.dart
ListView.builder(
itemBuilder: (*, index) => InkWell(
onTap: () => Navigator.push(
context,
MaterialPageRoute(
builder: (context) => ProductDetailPage(product: snapshot.data![index])
)
),
child: ProductCard(...)
)
)
```

Implementasi di atas mencakup:

- Autentikasi pengguna (login/register/logout)
- State management dengan Provider
- CRUD operasi dengan Django backend
- Routing dan navigasi
- Form handling dan validasi
- Model untuk JSON serialization/deserialization
- UI components seperti drawer dan cards
- Error handling dan loading states
