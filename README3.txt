(Giriş Ekranı)
import 'package:flutter/material.dart';

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Health Tracking App',
      home: LoginScreen(),
    );
  }
}

class LoginScreen extends StatefulWidget {
  @override
  _LoginScreenState createState() => _LoginScreenState();
}

class _LoginScreenState extends State<LoginScreen> {
  final TextEditingController tcController = TextEditingController();
  final TextEditingController passwordController = TextEditingController();
  String selectedUserType = 'Doktor';
  String errorMessage = '';

  final Map<String, String> doctorCredentials = {
    'tc': '12345678909',
    'password': 'A1212',
    'userType': 'Doktor',
  };

  final List<Map<String, String>> patientCredentials = [
    {'tc': '12345678908', 'password': 'AB1212', 'userType': 'Hasta', 'name': 'Hasta 1'},
    {'tc': '12345678907', 'password': 'BA1212', 'userType': 'Hasta', 'name': 'Hasta 2'},
  ];

  void validateLogin() {
    String enteredTc = tcController.text;
    String enteredPassword = passwordController.text;

    if (selectedUserType == 'Doktor') {
      if (enteredTc == doctorCredentials['tc'] && enteredPassword == doctorCredentials['password']) {
        Navigator.push(
          context,
          MaterialPageRoute(builder: (context) => DoctorScreen(patients: patientCredentials)),
        );
      } else {
        setState(() {
          errorMessage = 'Geçersiz TC veya şifre!';
        });
      }
    } else if (selectedUserType == 'Hasta') {
      bool validPatient = false;
      for (var patient in patientCredentials) {
        if (enteredTc == patient['tc'] && enteredPassword == patient['password']) {
          validPatient = true;
          break;
        }
      }
      if (validPatient) {
        Navigator.push(
          context,
          MaterialPageRoute(builder: (context) => PatientScreen()),
        );
      } else {
        setState(() {
          errorMessage = 'Geçersiz TC veya şifre!';
        });
      }
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Giriş Ekranı'),
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            TextField(
              controller: tcController,
              decoration: InputDecoration(labelText: 'TC Kimlik Numarası'),
              keyboardType: TextInputType.number,
            ),
            TextField(
              controller: passwordController,
              decoration: InputDecoration(labelText: 'Şifre'),
              obscureText: true,
            ),
            DropdownButton<String>(
              value: selectedUserType,
              onChanged: (String? newValue) {
                setState(() {
                  selectedUserType = newValue!;
                });
              },
              items: <String>['Doktor', 'Hasta']
                  .map<DropdownMenuItem<String>>((String value) {
                return DropdownMenuItem<String>(
                  value: value,
                  child: Text(value),
                );
              }).toList(),
            ),
            SizedBox(height: 20),
            ElevatedButton(
              onPressed: validateLogin,
              child: Text('Giriş Yap'),
            ),
            if (errorMessage.isNotEmpty)
              Padding(
                padding: const EdgeInsets.all(8.0),
                child: Text(
                  errorMessage,
                  style: TextStyle(color: Colors.red),
                ),
              ),
          ],
        ),
      ),
    );
  }
}

class DoctorScreen extends StatefulWidget {
  final List<Map<String, String>> patients;

  DoctorScreen({required this.patients});

  @override
  _DoctorScreenState createState() => _DoctorScreenState();
}

class _DoctorScreenState extends State<DoctorScreen> {
  final TextEditingController nameController = TextEditingController();
  final TextEditingController tcController = TextEditingController();

  void _showAddPatientDialog(BuildContext context) {
    showDialog(
      context: context,
      builder: (context) {
        return AlertDialog(
          title: Text('Yeni Hasta Ekle'),
          content: Column(
            mainAxisSize: MainAxisSize.min,
            children: [
              TextField(
                controller: nameController,
                decoration: InputDecoration(labelText: 'Hasta Adı'),
              ),
              TextField(
                controller: tcController,
                decoration: InputDecoration(labelText: 'Hasta TC'),
                keyboardType: TextInputType.number,
              ),
            ],
          ),
          actions: [
            TextButton(
              onPressed: () {
                String name = nameController.text;
                String tc = tcController.text;
                if (name.isNotEmpty && tc.isNotEmpty) {
                  setState(() {
                    widget.patients.add({'name': name, 'tc': tc});
                  });
                  Navigator.of(context).pop();
                }
              },
              child: Text('Ekle'),
            ),
            TextButton(
              onPressed: () {
                Navigator.of(context).pop();
              },
              child: Text('Vazgeç'),
            ),
          ],
        );
      },
    );
  }

  void _navigateToPatientPage(BuildContext context, String patientName) {
    Navigator.push(
      context,
      MaterialPageRoute(
        builder: (context) => PatientPage(patientName: patientName),
      ),
    );
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Doktor Ekranı'),
        actions: [
          IconButton(
            icon: Icon(Icons.add),
            onPressed: () => _showAddPatientDialog(context),
          ),
        ],
      ),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            Text('Hastalarım', style: TextStyle(fontSize: 24)),
            Expanded(
              child: ListView.builder(
                itemCount: widget.patients.length,
                itemBuilder: (context, index) {
                  return ListTile(
                    title: Text(widget.patients[index]['name']!),
                    subtitle: Text('TC: ${widget.patients[index]['tc']}'),
                    onTap: () {
                      _navigateToPatientPage(
                          context, widget.patients[index]['name']!);
                    },
                  );
                },
              ),
            ),
            SizedBox(height: 20),
            ElevatedButton(
              onPressed: () {
                _navigateToPatientPage(context, 'Hasta 1');
              },
              child: Text('Hasta 1'),
            ),
            ElevatedButton(
              onPressed: () {
                _navigateToPatientPage(context, 'Hasta 2');
              },
              child: Text('Hasta 2'),
            ),
          ],
        ),
      ),
    );
  }
}

class PatientPage extends StatelessWidget {
  final String patientName;

  PatientPage({required this.patientName});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('$patientName Bilgileri'),
      ),
      body: Center(
        child: Text('$patientName sayfası'),
      ),
    );
  }
}

class PatientScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Hasta Ekranı')),
      body: Center(
        child: Text('Hasta Ekranı', style: TextStyle(fontSize: 24)),
      ),
    );
  }
}
