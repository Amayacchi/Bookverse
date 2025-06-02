# Bookverse
Public
/lib
  main.dart
  /screens
    login_screen.dart
    home_screen.dart
    write_screen.dart
  /widgets
    book_card.dart
  /models
    book.dart
import 'package:flutter/material.dart';
import 'screens/login_screen.dart';
import 'screens/home_screen.dart';
import 'screens/write_screen.dart';

void main() {
  runApp(BookverseApp());
}

class BookverseApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Bookverse',
      debugShowCheckedModeBanner: false,
      theme: ThemeData(
        brightness: Brightness.light,
        primarySwatch: Colors.deepPurple,
      ),
      home: LoginScreen(),
    );
  }
}
import 'package:flutter/material.dart';
import 'home_screen.dart';

class LoginScreen extends StatelessWidget {
  final TextEditingController emailCtrl = TextEditingController();
  final TextEditingController passCtrl = TextEditingController();

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Padding(
        padding: EdgeInsets.all(24),
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            Text("Bookverse'e Hoş Geldin", style: TextStyle(fontSize: 24)),
            SizedBox(height: 20),
            TextField(controller: emailCtrl, decoration: InputDecoration(labelText: "Email")),
            TextField(controller: passCtrl, decoration: InputDecoration(labelText: "Şifre"), obscureText: true),
            SizedBox(height: 20),
            ElevatedButton(
              child: Text("Giriş Yap"),
              onPressed: () {
                // Buraya Firebase Auth entegresi yapılacak
                Navigator.pushReplacement(
                  context,
                  MaterialPageRoute(builder: (context) => HomeScreen()),
                );
              },
            ),
          ],
        ),
      ),
    );
  }
}
import 'package:flutter/material.dart';
import '../widgets/book_card.dart';
import 'write_screen.dart';

class HomeScreen extends StatefulWidget {
  @override
  _HomeScreenState createState() => _HomeScreenState();
}

class _HomeScreenState extends State<HomeScreen> {
  int currentIndex = 0;

  final List<Widget> _pages = [
    RecommendedBooksPage(),
    Center(child: Text('Keşfet Sayfası')),
    Center(child: Text('Profil Sayfası')),
  ];

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Bookverse')),
      body: _pages[currentIndex],
      bottomNavigationBar: BottomNavigationBar(
        currentIndex: currentIndex,
        onTap: (i) => setState(() => currentIndex = i),
        items: [
          BottomNavigationBarItem(icon: Icon(Icons.home), label: "Ana Sayfa"),
          BottomNavigationBarItem(icon: Icon(Icons.explore), label: "Keşfet"),
          BottomNavigationBarItem(icon: Icon(Icons.person), label: "Profil"),
        ],
      ),
      drawer: Drawer(
        child: ListView(
          children: [
            DrawerHeader(child: Text("Menü")),
            ListTile(
              leading: Icon(Icons.edit),
              title: Text("Kitap Yaz"),
              onTap: () {
                Navigator.push(context, MaterialPageRoute(builder: (_) => WriteScreen()));
              },
            ),
          ],
        ),
      ),
    );
  }
}

class RecommendedBooksPage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return ListView(
      children: [
        BookCard(title: "Gölgelerin Efendisi", author: "John Flanagan"),
        BookCard(title: "Ateşten Kız", author: "Akilah Green"),
      ],
    );
  }
}
import 'package:flutter/material.dart';

class BookCard extends StatelessWidget {
  final String title;
  final String author;

  const BookCard({required this.title, required this.author});

  @override
  Widget build(BuildContext context) {
    return Card(
      margin: EdgeInsets.all(12),
      child: ListTile(
        title: Text(title),
        subtitle: Text("Yazar: $author"),
        trailing: Icon(Icons.book),
      ),
    );
  }
}
import 'package:flutter/material.dart';

class WriteScreen extends StatefulWidget {
  @override
  _WriteScreenState createState() => _WriteScreenState();
}

class _WriteScreenState extends State<WriteScreen> with SingleTickerProviderStateMixin {
  late TabController _tabController;

  @override
  void initState() {
    _tabController = TabController(length: 4, vsync: this);
    super.initState();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text("Kitap Yaz"),
        bottom: TabBar(
          controller: _tabController,
          tabs: [
            Tab(text: "Bilgiler"),
            Tab(text: "Notlar"),
            Tab(text: "Karakterler"),
            Tab(text: "Yerler"),
          ],
        ),
      ),
      body: TabBarView(
        controller: _tabController,
        children: [
          Center(child: Text("Kitap Başlığı, Açıklama, Tür vb.")),
          Center(child: Text("Notlarınızı buraya yazın")),
          Center(child: Text("Karakter oluşturun")),
          Center(child: Text("Yer bilgileri girin")),
        ],
      ),
    );
  }
}