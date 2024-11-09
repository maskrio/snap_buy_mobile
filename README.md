# Snap Buy Mobile

## Tugas 7

### Stateless Widget and StateFul Widget

Stateless Widget adalah Widget yang tidak memiilki State dan tidak dapat dirubah setelah dibuat. Tampilan dirender sekali saat pertama kali dibuat, dan tidak akan ada perubahan tampilan lagi.

Stateful Widget adalah Widget yang memiliki state dan dapat berubah seiring waktu dengan interaksi pengguna. Tampilan dapat berubah saat state berubah dengan pemanggilan `setState()`. 

---
### Widget yang digunakan
- MaterialApp : container struktur aplikasi sebagai root widget.
- Scaffold : menyediakan struktur dasar halaman dengan AppBar dan body.
- AppBar : bagian atas halaman yang menampilkan judul.
- Padding : Body halaman dengan padding di sekelilingnya.
- Column : Menyusun widget secara vertikal dalam sebuah kolom.
- Row : Row untuk menampilkan 3 InfoCard secara horizontal
- Center : Menempatkan widget berikutnya di tengah halaman.
- InkWell : menambahkan efek pada widget lain ketika ditekan.
- SnackBar : Menampilkan popup berisi massage sesuai argumen.
---
### Fungsi `setState()`

Guna pemanggilan `setState()` adalah untuk memberi tahu flutter bahwa ada perubahan data atau variable dalam state widget yang perlu ditampilkan, sehingga memicu pembaruan tampilan. Variabel yang terpengaruh oleh `setState()` adalah variabel yang digunakan dalam tampilan, status sementara aplikasi (seperti toggle/input pengguna), dan variabel-variabel lain yang dapat bertambah, berkurang, atau berubah berdasarkan interaksi pengguna.

---
### `final` vs `const`
Keduanya digunakan untuk mendeklarasikan variabel yang tidak dapat dirubah. 

Final digunakan bila masih belum tahu nilai dari variabel tersebut saat dikompilasi, sedangkkan const nilainya harus ditentukan saat kompilasi, bukan saat runtime.
```dart
final now = DateTime.now(); 
final name = getName(); 
const pi = 3.14159; 
const greeting = "Hello, World!"; 
```
--- 
### Implementasi Step by step

1. Membuat program flutter baru
``` 
flutter create snap_buy_mobile
```

2. Membuat 3 tombol sederhana, pada directory `lib/menu.dart`
- mendefinisikan dahulu ItemHomePage(tombol)
```dart
class ItemHomepage {
  final String name;
  final IconData icon;
  final Color color;

  ItemHomepage(this.name, this.icon, this.color);
}
```

- lalu pada menambahkan list of items pada home page

```dart
class MyHomePage extends StatelessWidget  {
    ...
    final List<ItemHomepage> items = [
        ItemHomepage("Lihat Mood", Icons.mood, Colors.blue),
    		ItemHomepage("Tambah Mood", Icons.add, Colors.green),
    		ItemHomepage("Logout", Icons.logout, Colors.red),
    ];
    ...
```

- lalu mendefinisikan tampilan dari setiap item dengan class `itemCard`, ini termasuk ke pendefinisian warna dan snackbar.
```dart
class ItemCard extends StatelessWidget {
  // Menampilkan kartu dengan ikon dan nama.

  final ItemHomepage item; 
  
  const ItemCard(this.item, {super.key}); 

  @override
  Widget build(BuildContext context) {
    return Material(
      // Menentukan warna latar belakang dari tema aplikasi.
      color: item.color,
      // Membuat sudut kartu melengkung.
      borderRadius: BorderRadius.circular(12),
      
      child: InkWell(
        // Aksi ketika kartu ditekan.
        onTap: () {
          // Menampilkan pesan SnackBar saat kartu ditekan.
          ScaffoldMessenger.of(context)
            ..hideCurrentSnackBar()
            ..showSnackBar(
              SnackBar(content: Text("Kamu telah menekan tombol ${item.name}!"))
            );
        },
        // Container untuk menyimpan Icon dan Text
        child: Container(
          padding: const EdgeInsets.all(8),
          child: Center(
            child: Column(
              // Menyusun ikon dan teks di tengah kartu.
              mainAxisAlignment: MainAxisAlignment.center,
              children: [
                Icon(
                  item.icon,
                  color: Colors.white,
                  size: 30.0,
                ),
                const Padding(padding: EdgeInsets.all(3)),
                Text(
                  item.name,
                  textAlign: TextAlign.center,
                  style: const TextStyle(color: Colors.white),
                ),
              ],
            ),
          ),
        ),
      ),
    );
  }
}
```

- lalu untuk setiap item ditampilkan (pada tempat yang sesuai)
```dart
GridView.count(
  primary: true,
  padding: const EdgeInsets.all(20),
  crossAxisSpacing: 10,
  mainAxisSpacing: 10,
  crossAxisCount: 3,
  // Agar grid menyesuaikan tinggi kontennya.
  shrinkWrap: true,
  // Menampilkan ItemCard untuk setiap item dalam listitems.
  children: items.map((ItemHomepage item) {
    return ItemCard(item);
  }).toList(),
),
```

## Tugas 8

### Penggunaan `const` 

`const` digunakan untuk setiap nilai/widget yang bersifat tetap dan tidak berubah. Keuntungannya adalah Flutter akan mencache nilai/widget saat pertama kali dibuat dan tidak perlu alokasi memory tambahan. Ini meningkatkan performa dari program itu sendiri. `const` digunakan apabila nilai/widget yang dipakai tidak akan berubah, sedangkan jangan digunakan apabila nilai/widget masih akan berubah.


### Column vs Row

Column menyusun item secara berurutan ke bawah, sedangkan Row ke samping. 

Contoh implementasi: 
```dart 
Column(
  mainAxisAlignment: MainAxisAlignment.center,
  crossAxisAlignment: CrossAxisAlignment.center,
  children: [
    Text(
      'Item akan tersusun secara vertikal',
      style: TextStyle(fontSize: 24),
    ),
    SizedBox(height: 20), // Spacer untuk memberi jarak
    ElevatedButton(
      onPressed: () {},
      child: Text('Klik Saya'),
    ),
  ],
),
```

```dart
Row(
  mainAxisAlignment: MainAxisAlignment.spaceAround,
  children: [
    Text(
      'Item akan tersusun secara horizontal',
      style: TextStyle(fontSize: 24),
    ),
    ElevatedButton(
      onPressed: () {},
      child: Text('Tombol 1'),
    ),
    ElevatedButton(
      onPressed: () {},
      child: Text('Tombol 2'),
    ),
    ElevatedButton(
      onPressed: () {},
      child: Text('Tombol 3'),
    ),
  ],
),
```

### Input yang digunakan & tidak digunakan

Jenis input yang digunakan :
- TextFormField
- ElevatedButton

Jenis input yang belum digunakan :
- CheckBox
- Radio
- Switch
- DropdownButtonFormField
- Slider
- DatePicker

### Mengatur tema flutter 

Mendefinisikan tema global di `MainApp` pada `lib/main.dart`, definisikan `theme`nya, lalu membuat seluruh colorScheme pada komponen lainnya mengikuti Theme global tersebut. 

Contohnya seperti menerapkan 
```dart
AppBar(
  backgroundColor: Theme.of(context).colorScheme.primary,
),
```

### Navigator 

Dalam flutter, navigasi yang digunakan adalah stack. Stack ini akan menyimpan lokasi-lokasi yang diakses oleh pengguna. 

Apabila mengklik sesuatu, perlu di navigate ke page lain, maka harus push()
```dart
    if (item.name == "Tambah Mood") {
        Navigator.push(context,
            MaterialPageRoute(builder: (context) => const MoodEntryFormPage()));
    }
// pergi ke Page baru dan push ke stack halaman sekarang
```

Kenapa perlu stack? 

Karena untuk fungsionalitas misalkan tombol back, maka harus pergi ke page sebelumnya.
```dart 
    onPressed: () {
        Navigator.pop(context);
    },
```

Apabila tidak mau disimpan page yang sebelumnya, maka gunakan pushReplacement untuk mereplace stack teratas.
```dart 
    onTap: () {
        Navigator.pushReplacement(
        context,
        MaterialPageRoute(
            builder: (context) => MyHomePage(),
        ));
    },
```