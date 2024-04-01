import 'package:flutter/material.dart';

class CGPACalculatorApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      debugShowCheckedModeBanner: false,
      theme: ThemeData(
        primaryColor: Colors.lightBlueAccent,
        primarySwatch: Colors.lightBlue,
        textTheme: TextTheme(
          headline6: TextStyle(
            fontFamily: 'Roboto',
            fontSize: 20.0,
            fontWeight: FontWeight.bold,
          ),
          bodyText1: TextStyle(
            fontFamily: 'Roboto',
            fontSize: 16.0,
          ),
        ),
      ),
      home: CGPACalculator(),
    );
  }
}

class CGPACalculator extends StatefulWidget {
  const CGPACalculator();

  @override
  _CGPACalculatorState createState() => _CGPACalculatorState();
}

class _CGPACalculatorState extends State<CGPACalculator> {
  final TextEditingController _subController = TextEditingController();
  final TextEditingController _credController = TextEditingController();
  final TextEditingController _graController = TextEditingController();

  List<int> arr = [];
  List<int> grade = [];
  List<int> gra_points = [];
  Map<String, int> dic = {'S': 10, 'A': 9, 'B': 8, 'C': 7, 'D': 6, 'E': 5};

  void calculateCGPA() {
    try {
      int sub = int.parse(_subController.text);
      List<int> cred = _credController.text.split(' ').map(int.parse).toList();
      List<String> gra = _graController.text.split(' ');

      if (cred.length != sub || gra.length != sub) {
        throw Exception(
            'Invalid input. Please enter valid data for all subjects.');
      }

      for (int l = 0; l < cred.length; l++) {
        int gra_point = cred[l] * (dic[gra[l]] ?? 0);
        gra_points.add(gra_point);
      }

      int summ = cred.fold(0, (sum, element) => sum + element);
      arr.add(summ);

      double cgpa = gra_points.fold(0, (sum, element) => sum + element) /
          arr.fold(0, (sum, element) => sum + element);

      showDialog(
        context: context,
        builder: (context) => AlertDialog(
          title: Text('CGPA Calculator'),
          content: Text('Your CGPA is: ${cgpa.toStringAsFixed(2)}'),
          actions: [
            TextButton(
              onPressed: () => Navigator.pop(context),
              child: Text('OK'),
            ),
          ],
        ),
      );
    } catch (e) {
      showDialog(
        context: context,
        builder: (context) => AlertDialog(
          title: Text('Error'),
          content: Text('$e'),
          actions: [
            TextButton(
              onPressed: () => Navigator.pop(context),
              child: Text('OK'),
            ),
          ],
        ),
      );
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        leading: Icon(Icons.book),
        title: Text('CGPA Calculator'),
      ),
      body: SingleChildScrollView(
        child: Padding(
          padding: const EdgeInsets.all(16.0),
          child: Column(
            children: [
              TextField(
                controller: _subController,
                decoration: InputDecoration(
                  labelText: 'Number of subjects',
                  border: OutlineInputBorder(
                    borderRadius: BorderRadius.circular(10.0),
                  ),
                ),
              ),
              SizedBox(height: 16.0),
              TextField(
                controller: _credController,
                decoration: InputDecoration(
                  labelText: 'Credits (separated by space)',
                  border: OutlineInputBorder(
                    borderRadius: BorderRadius.circular(10.0),
                  ),
                ),
              ),
              SizedBox(height: 16.0),
              TextField(
                controller: _graController,
                decoration: InputDecoration(
                  labelText: 'Grades (in caps, separated by space)',
                  border: OutlineInputBorder(
                    borderRadius: BorderRadius.circular(10.0),
                  ),
                ),
              ),
              SizedBox(height: 16.0),
              ElevatedButton(
                onPressed: calculateCGPA,
                style: ElevatedButton.styleFrom(
                  shape: RoundedRectangleBorder(
                    borderRadius: BorderRadius.circular(10.0),
                  ),
                ),
                child: Text('Calculate CGPA'),
              ),
            ],
          ),
        ),
      ),
    );
  }
}

void main() {
  runApp(CGPACalculatorApp());
}
