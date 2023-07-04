# CadastroAula
# Cadastro de Aula através do Flluter



import 'package:flutter/material.dart';

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Cadastro de Aula',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      debugShowCheckedModeBanner: false,
      home: Scaffold(
        appBar: AppBar(
          title: Text(
            'Cadastro de Aula',
            style: TextStyle(fontSize: 20.0),
          ),
        ),
        body: Center(
          child: CronogramasForm(),
        ),
      ),
    );
  }
}

class CronogramasForm extends StatefulWidget {
  @override
  _CronogramasFormState createState() => _CronogramasFormState();
}

class _CronogramasFormState extends State<CronogramasForm> {
  String dropdownValue = 'Pendente';
  DateTime? selectedDateTime;

  Future<void> _selectDateTime(BuildContext context) async {
    final DateTime? picked = await showDatePicker(
      context: context,
      initialDate: selectedDateTime ?? DateTime.now(),
      firstDate: DateTime(2000),
      lastDate: DateTime(2100),
    );

    if (picked != null) {
      final TimeOfDay? pickedTime = await showTimePicker(
        context: context,
        initialTime: TimeOfDay.fromDateTime(selectedDateTime ?? DateTime.now()),
      );

      if (pickedTime != null) {
        setState(() {
          selectedDateTime = DateTime(
            picked.year,
            picked.month,
            picked.day,
            pickedTime.hour,
            pickedTime.minute,
          );
        });
      }
    }
  }

  Color getStatusColor(String status) {
    switch (status) {
      case 'Pendente':
        return Colors.amber;
      case 'Ministrada':
        return Colors.green;
      case 'Cancelada':
        return Colors.red;
      default:
        return Colors.black;
    }
  }

  @override
  Widget build(BuildContext context) {
    return Container(
      padding: EdgeInsets.all(16.0),
      child: Column(
        crossAxisAlignment: CrossAxisAlignment.start,
        children: [
          Text(
            'ID da Aula',
            style: TextStyle(fontSize: 18.0, fontWeight: FontWeight.bold),
          ),
          TextFormField(
            decoration: InputDecoration(
              hintText: 'Digite o ID da aula',
            ),
          ),
          SizedBox(height: 16.0),
          Text(
            'Data e Hora da Aula',
            style: TextStyle(fontSize: 18.0, fontWeight: FontWeight.bold),
          ),
          GestureDetector(
            onTap: () => _selectDateTime(context),
            child: Container(
              decoration: BoxDecoration(
                borderRadius: BorderRadius.circular(4.0),
                border: Border.all(
                  color: Colors.grey,
                  width: 1.0,
                ),
              ),
              padding: EdgeInsets.symmetric(vertical: 12.0, horizontal: 16.0),
              child: Row(
                children: [
                  Icon(Icons.calendar_today),
                  SizedBox(width: 8.0),
                  Text(
                    selectedDateTime != null
                        ? selectedDateTime!.toString()
                        : 'Selecione a data e hora da aula',
                    style: TextStyle(fontSize: 16.0),
                  ),
                ],
              ),
            ),
          ),
          SizedBox(height: 16.0),
          Text(
            'Status da Aula',
            style: TextStyle(fontSize: 18.0, fontWeight: FontWeight.bold),
          ),
          DropdownButton<String>(
            value: dropdownValue,
            onChanged: (String? newValue) {
              setState(() {
                dropdownValue = newValue!;
              });
            },
            items: <String>['Pendente', 'Ministrada', 'Cancelada']
                .map<DropdownMenuItem<String>>(
              (String value) {
                return DropdownMenuItem<String>(
                  value: value,
                  child: Container(
                    decoration: BoxDecoration(
                      color: getStatusColor(value),
                      borderRadius: BorderRadius.circular(4.0),
                    ),
                    padding:
                        EdgeInsets.symmetric(vertical: 8.0, horizontal: 12.0),
                    child: Text(
                      value,
                      style: TextStyle(
                        fontSize: 16.0,
                        color: Colors.white,
                      ),
                    ),
                  ),
                );
              },
            ).toList(),
          ),
          SizedBox(height: 16.0),
          Text(
            'Turma',
            style: TextStyle(fontSize: 18.0, fontWeight: FontWeight.bold),
          ),
          TextFormField(
            decoration: InputDecoration(
              hintText: 'Digite a turma da aula',
            ),
          ),
          SizedBox(height: 16.0),
          Text(
            'Unidade Curricular',
            style: TextStyle(fontSize: 18.0, fontWeight: FontWeight.bold),
          ),
          TextFormField(
            decoration: InputDecoration(
              hintText: 'Digite a unidade curricular da aula',
            ),
          ),
          SizedBox(height: 24.0),
          Align(
            alignment: Alignment.center,
            child: ElevatedButton(
              onPressed: () {},
              child: Text(
                'Salvar',
                style: TextStyle(fontSize: 18.0),
              ),
              style: ElevatedButton.styleFrom(
                padding: EdgeInsets.symmetric(horizontal: 24.0, vertical: 12.0),
                textStyle: TextStyle(fontSize: 18.0),
              ),
            ),
          ),
        ],
      ),
    );
  }
}
