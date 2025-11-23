```dart
import 'package:flutter/material.dart';  
  
class CurrencyConverter extends StatefulWidget {  
  const CurrencyConverter({super.key});  
  
  @override  
  State<CurrencyConverter> createState() => _CurrencyConverterState();  
}  
  
class _CurrencyConverterState extends State<CurrencyConverter> {  
  double value = 0;  
  TextEditingController currencyController = TextEditingController();  
  
  @override  
  Widget build(BuildContext context) {  
    return Scaffold(  
      appBar: AppBar(  
        title: Text("Currency Converter"),  
        backgroundColor: Colors.lightBlue,  
      ),  
      body: Center(  
        child: SingleChildScrollView(  
          child: Padding(  
            padding: const EdgeInsets.all(10.0),  
            child: Column(  
              children: [  
                Card(  
                  color: Colors.blueAccent,  
                  child: Container(  
                    padding: EdgeInsets.all(20),  
                    child: Column(  
                      children: [  
                        Text("Converted Amount"),  
                        Text("Rs. ${value.toStringAsFixed(2)}"),  
                      ],  
                    ),  
                  ),  
                ),  
  
                SizedBox(height: 20),  
                TextField(controller: currencyController),  
                SizedBox(height: 20),  
                ElevatedButton(  
                  onPressed: () {  
                    String text = currencyController.text;  
                    double usdValue = double.parse(text);  
  
                    setState(() {  
                      value = usdValue * 85.67;  
                    });  
                  },  
                  child: Text("Convert to INR"),  
                ),  
                SizedBox(height: 20),  
                Text("Exchange Rate 1 USD = 85.67 INR"),  
              ],  
            ),  
          ),  
        ),  
      ),  
    );  
  }  
}
```