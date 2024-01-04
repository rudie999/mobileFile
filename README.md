# Nama : M Rohmatul Mauludi 
# Kelas : TI 3A
# NIM : 2141720062
# Praktikum : Week 12

## Dasar Manajemen State di Flutter
## Praktikum 1: Dasar State dengan Model-View

Langkah 1: Buat Project Baru
Buatlah sebuah project flutter baru dengan nama master_plan di folder src week-11 repository GitHub Anda. Lalu buatlah susunan folder dalam project seperti gambar berikut ini.


Langkah 2: Membuat model task.dart
Praktik terbaik untuk memulai adalah pada lapisan data (data layer). Ini akan memberi Anda gambaran yang jelas tentang aplikasi Anda, tanpa masuk ke detail antarmuka pengguna Anda. Di folder model, buat file bernama task.dart dan buat class Task. Class ini memiliki atribut description dengan tipe data String dan complete dengan tipe data Boolean, serta ada konstruktor. Kelas ini akan menyimpan data tugas untuk aplikasi kita. Tambahkan kode berikut:

Langkah 3: Buat file plan.dart
Kita juga perlu sebuah List untuk menyimpan daftar rencana dalam aplikasi to-do ini. Buat file plan.dart di dalam folder models dan isi kode seperti berikut.

import './task.dart';

class Plan {
final String name;
final List<Task> tasks;

const Plan({this.name = '', this.tasks = const []});
}
Langkah 4: Buat file data_layer.dart
Kita dapat membungkus beberapa data layer ke dalam sebuah file yang nanti akan mengekspor kedua model tersebut. Dengan begitu, proses impor akan lebih ringkas seiring berkembangnya aplikasi. Buat file bernama data_layer.dart di folder models. Kodenya hanya berisi export seperti berikut.

export 'plan.dart';
export 'task.dart';
Langkah 5: Pindah ke file main.dart
Ubah isi kode main.dart sebagai berikut.

import 'package:flutter/material.dart';
import './views/plan_screen.dart';

void main() => runApp(MasterPlanApp());

class MasterPlanApp extends StatelessWidget {
const MasterPlanApp({super.key});

@override
Widget build(BuildContext context) {
    return MaterialApp(
    theme: ThemeData(primarySwatch: Colors.purple),
    home: PlanScreen(),
    );
}
}
Langkah 6: buat plan_screen.dart
Pada folder views, buatlah sebuah file plan_screen.dart dan gunakan templat StatefulWidget untuk membuat class PlanScreen. Isi kodenya adalah sebagai berikut. Gantilah teks ‘Namaku' dengan nama panggilan Anda pada title AppBar.

import '../models/data_layer.dart';
import 'package:flutter/material.dart';

class PlanScreen extends StatefulWidget {
const PlanScreen({super.key});

@override
State createState() => _PlanScreenState();
}

class _PlanScreenState extends State<PlanScreen> {
Plan plan = const Plan();

@override
Widget build(BuildContext context) {
return Scaffold(
    // ganti ‘Namaku' dengan Nama panggilan Anda
    appBar: AppBar(title: const Text('Master Plan Namaku')),
    body: _buildList(),
    floatingActionButton: _buildAddTaskButton(),
);
}
}
Langkah 7: buat method _buildAddTaskButton()
Anda akan melihat beberapa error di langkah 6, karena method yang belum dibuat. Ayo kita buat mulai dari yang paling mudah yaitu tombol Tambah Rencana. Tambah kode berikut di bawah method build di dalam class _PlanScreenState.

Widget _buildAddTaskButton() {
    return FloatingActionButton(
        child: const Icon(Icons.add),
        onPressed: () {
            setState(() {
                plan = Plan(
                name: plan.name,
                tasks: List<Task>.from(plan.tasks)
                ..add(const Task()),
                );
            });
        },
    );
}
Langkah 8: buat widget _buildList()
Kita akan buat widget berupa List yang dapat dilakukan scroll, yaitu ListView.builder. Buat widget ListView seperti kode berikut ini.

Widget _buildList() {
    return ListView.builder(
        itemCount: plan.tasks.length,
        itemBuilder: (context, index) =>
        _buildTaskTile(plan.tasks[index], index),
    );
}
Langkah 9: buat widget _buildTaskTile
Dari langkah 8, kita butuh ListTile untuk menampilkan setiap nilai dari plan.tasks. Kita buat dinamis untuk setiap index data, sehingga membuat view menjadi lebih mudah. Tambahkan kode berikut ini.

Widget _buildTaskTile(Task task, int index) {
    return ListTile(
    leading: Checkbox(
        value: task.complete,
        onChanged: (selected) {
            setState(() {
            plan = Plan(
                name: plan.name,
                tasks: List<Task>.from(plan.tasks)
                ..[index] = Task(
                    description: task.description,
                    complete: selected ?? false,
                ),
            );
            });
        }),
    title: TextFormField(
        initialValue: task.description,
        onChanged: (text) {
        setState(() {
            plan = Plan(
            name: plan.name,
            tasks: List<Task>.from(plan.tasks)
                ..[index] = Task(
                description: text,
                complete: task.complete,
                ),
            );
        });
        },
    ),
    );
}
Run atau tekan F5 untuk melihat hasil aplikasi yang Anda telah buat. Capture hasilnya untuk soal praktikum nomor 4.
