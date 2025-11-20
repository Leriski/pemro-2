Galeri Buku

kelompok :Leriski,Giral,Sandi

link drive :https://drive.google.com/drive/folders/18FPx2fF_p_VS8PSDA_76aZ2O7r5bwfQl?usp=drive_link


import 'package:flutter/material.dart';

void main() => runApp(GaleriBukuApp());

class GaleriBukuApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Galeri Buku',
      home: GaleriBukuPage(),
      debugShowCheckedModeBanner: false,
    );
  }
}

class GaleriBukuPage extends StatefulWidget {
  @override
  _GaleriBukuPageState createState() => _GaleriBukuPageState();
}

class _GaleriBukuPageState extends State<GaleriBukuPage> {
  List<Buku> daftarBuku = [
    Buku(judul: 'Atomic Habits', penulis: 'James Clear'),
    Buku(judul: 'Deep Work', penulis: 'Cal Newport'),
    Buku(judul: 'Clean Code', penulis: 'Robert C. Martin'),
    Buku(judul: 'The Pragmatic Programmer', penulis: 'Andrew Hunt'),
    Buku(judul: 'Flutter for Beginners', penulis: 'Alessandro Biessek'),
  ];

  void toggleFavorit(int index) {
    setState(() {
      daftarBuku[index].favorit = !daftarBuku[index].favorit;
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Galeri Buku')),
      body: Padding(
        padding: const EdgeInsets.all(8.0),
        child: GridView.builder(
          itemCount: daftarBuku.length,
          gridDelegate: SliverGridDelegateWithFixedCrossAxisCount(
            crossAxisCount: 2, childAspectRatio: 0.75, crossAxisSpacing: 10, mainAxisSpacing: 10),
          itemBuilder: (context, index) {
            return KartuBuku(
              buku: daftarBuku[index],
              onTap: () => toggleFavorit(index),
            );
          },
        ),
      ),
    );
  }
}

class Buku {
  final String judul;
  final String penulis;
  bool favorit;

  Buku({required this.judul, required this.penulis, this.favorit = false});
}

class KartuBuku extends StatefulWidget {
  final Buku buku;
  final VoidCallback onTap;

  const KartuBuku({required this.buku, required this.onTap});

  @override
  _KartuBukuState createState() => _KartuBukuState();
}

class _KartuBukuState extends State<KartuBuku> with SingleTickerProviderStateMixin {
  late AnimationController _controller;
  late Animation<double> _skala;

  @override
  void initState() {
    super.initState();
    _controller = AnimationController(duration: Duration(milliseconds: 200), vsync: this);
    _skala = Tween<double>(begin: 1.0, end: 0.95).animate(_controller);
  }

  void _tekanBawah(TapDownDetails details) => _controller.forward();
  void _tekanLepas(TapUpDetails details) => _controller.reverse();

  @override
  Widget build(BuildContext context) {
    return GestureDetector(
      onTap: widget.onTap,
      onTapDown: _tekanBawah,
      onTapUp: _tekanLepas,
      child: ScaleTransition(
        scale: _skala,
        child: Card(
          elevation: 4,
          shape: RoundedRectangleBorder(borderRadius: BorderRadius.circular(12)),
          child: Padding(
            padding: const EdgeInsets.all(12.0),
            child: Column(
              mainAxisAlignment: MainAxisAlignment.center,
              children: [
                Icon(Icons.menu_book, size: 50, color: Colors.indigo),
                SizedBox(height: 10),
                Text(widget.buku.judul, style: TextStyle(fontWeight: FontWeight.bold), textAlign: TextAlign.center),
                SizedBox(height: 5),
                Text(widget.buku.penulis, style: TextStyle(color: Colors.grey), textAlign: TextAlign.center),
                SizedBox(height: 10),
                Icon(
                  widget.buku.favorit ? Icons.favorite : Icons.favorite_border,
                  color: widget.buku.favorit ? Colors.red : Colors.grey,
                ),
              ],
            ),
          ),
        ),
      ),
    );
  }

  @override
  void dispose() {
    _controller.dispose();
    super.dispose();
  }
}
