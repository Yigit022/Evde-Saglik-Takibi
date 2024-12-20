# Evde-Saglik-Takibi (Arayüz)
Açılacak olan bu
import 'package:flutter/material.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Evde Sağlık Takibi',
      theme: ThemeData(
        primarySwatch: Colors.blue,
        useMaterial3: true,
      ),
      home: const MyHomePage(),
    );
  }
}

class MyHomePage extends StatelessWidget {
  const MyHomePage({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Evde Sağlık Takibi'),
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: GridView.count(
          crossAxisCount: 2,
          crossAxisSpacing: 10,
          mainAxisSpacing: 10,
          children: <Widget>[
            _buildCard(context, 'E-Reçete', Icons.receipt, Colors.blue, const PrescriptionPage()),
            _buildCard(context, 'İlaç Takibi', Icons.medication, Colors.green, const MedicationTrackingPage()),
            _buildCard(context, 'Doktor Talebi', Icons.local_hospital, Colors.orange, const DoctorRequestPage()),
            _buildCard(context, 'Tıbbi Cihaz Takibi', Icons.devices, Colors.purple, const DeviceTrackingPage()),
            _buildCard(context, 'Randevu Başvuru', Icons.app_registration, Colors.yellow, const AppointmentFormPage()),
            _buildCard(context, 'Sağlık Verileri', Icons.health_and_safety, Colors.red, const HealthDataPage()),
          ],
        ),
      ),
    );
  }

  Widget _buildCard(BuildContext context, String title, IconData icon, Color color, Widget targetPage) {
    return Card(
      color: color.withOpacity(0.3),
      child: InkWell(
        onTap: () {
          Navigator.push(
            context,
            MaterialPageRoute(builder: (context) => targetPage),
          );
        },
        child: Center(
          child: Column(
            mainAxisAlignment: MainAxisAlignment.center,
            children: [
              Icon(icon, size: 40, color: color),
              const SizedBox(height: 10),
              Text(title, style: const TextStyle(fontSize: 18)),
            ],
          ),
        ),
      ),
    );
  }
}

// E-Reçete Sayfası
class PrescriptionPage extends StatelessWidget {
  const PrescriptionPage({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('E-Reçete'),
      ),
      body: ListView(
        children: [
          ListTile(
            title: const Text('Geçmiş Reçeteler'),
            leading: const Icon(Icons.history),
            onTap: () {
              // Geçmiş reçetelere yönlendirme
            },
          ),
          ListTile(
            title: const Text('Yeni Reçeteler'),
            leading: const Icon(Icons.add),
            onTap: () {
              // Yeni reçeteler sayfası
            },
          ),
        ],
      ),
    );
  }
}

// İlaç Takibi Sayfası
class MedicationTrackingPage extends StatelessWidget {
  const MedicationTrackingPage({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('İlaç Takibi'),
      ),
      body: ListView(
        children: [
          ListTile(
            title: const Text('İlaç İsmi: Parol'),
            subtitle: const Text('Günde 3 kez: 08:00, 14:00, 20:00'),
            trailing: Checkbox(
              value: false,
              onChanged: (bool? value) {
                // İlacın alındığını işaretle
              },
            ),
          ),
          // Diğer ilaçlar buraya eklenebilir
        ],
      ),
    );
  }
}

// Doktor Talebi Sayfası
class DoctorRequestPage extends StatelessWidget {
  const DoctorRequestPage({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Doktor Talebi'),
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          children: [
            TextField(
              decoration: const InputDecoration(labelText: 'Şikayetiniz'),
            ),
            DropdownButtonFormField<String>(
              items: const [
                DropdownMenuItem(value: 'Acil', child: Text('Acil')),
                DropdownMenuItem(value: 'Normal', child: Text('Normal')),
              ],
              onChanged: (value) {},
              decoration: const InputDecoration(labelText: 'Aciliyet Durumu'),
            ),
            TextField(
              decoration: const InputDecoration(labelText: 'Tarih ve Saat'),
            ),
            const SizedBox(height: 20),
            ElevatedButton(
              onPressed: () {
                // Talebi kaydet
              },
              child: const Text('Talep Oluştur'),
            ),
          ],
        ),
      ),
    );
  }
}

// Tıbbi Cihaz Takibi Sayfası
class DeviceTrackingPage extends StatelessWidget {
  const DeviceTrackingPage({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Tıbbi Cihaz Takibi'),
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          children: [
            TextField(
              decoration: const InputDecoration(labelText: 'Cihaz Kodu'),
            ),
            const SizedBox(height: 20),
            ElevatedButton(
              onPressed: () {
                // Bilgileri getir
              },
              child: const Text('Bilgileri Görüntüle'),
            ),
          ],
        ),
      ),
    );
  }
}

// Randevu Başvuru Sayfası
class AppointmentFormPage extends StatelessWidget {
  const AppointmentFormPage({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Randevu Başvuru'),
      ),
      body: ListView.builder(
        itemCount: 20, // 20 soru
        itemBuilder: (context, index) {
          return ListTile(
            title: Text('Soru ${index + 1}'),
            subtitle: const Text('Bu sorunun açıklaması burada.'),
          );
        },
      ),
    );
  }
}

// Sağlık Verileri Sayfası
class HealthDataPage extends StatelessWidget {
  const HealthDataPage({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Sağlık Verileri'),
      ),
      body: ListView(
        children: [
          ListTile(
            title: const Text('Kan Şekeri'),
            subtitle: const Text('Son Ölçüm: 120 mg/dL'),
          ),
          ListTile(
            title: const Text('Tansiyon'),
            subtitle: const Text('Son Ölçüm: 120/80 mmHg'),
          ),
          // Diğer sağlık verileri
        ],
      ),
    );
  }
}
