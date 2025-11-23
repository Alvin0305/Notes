- add dependencies in pubspec.yaml file
```yaml
deoendencies:
	hive: ^2.2.3
	hive_flutter: ^1.1.0
	
dev_dependencies:
	hive_generator: ^2.0.1
	build_runner: ^2.4.13
```

- run this command to get the dependencies
```bash
flutter pub get
```

>[!tip] 
>- what are we gonna save
>- how are we gonna save
>- initialize Hive

- Update the task.dart file in lib/models
```dart
import 'package:hive/hive.dart';

part 'task.g.dart';

@HiveType(typeId: 0)
class Task {
	@HiveField(0)
	String name;
	
	@HiveField(1)
	bool completed;
	
	Task({required this.name, required this.completed});
}
```

- run this command to create task.g.dart file
```bash
flutter packages pub run build_runner build
```

- Update the main.dart file
```dart
import 'package:hive_flutter/hive_flutter.dart';
import 'package:todo_list/models/task.dart';

void main() async {
	WidgetsFlutterBinding.ensureInitialized();
	
	await Hive.initFlutter();
	Hive.registerAdapter(TaskAdapter());
	await Hive.openBox<Task>('tasksBox');
	
	runApp(const MyApp());
}
```

- Create the database_services.dart file in lib/services
```dart
import 'package:hive/hive.dart';
import 'package:todo_list/models/task.dart';

final Box<Task> taskBox = Hive.box('tasksBox');

Future<void> addTask(Task task) async {
	await taskBox.add(task);
}

List<Task> getTasks() {
	return taskBox.values.toList();
}

Future<void> updateTask(int index, Task task) async {
	await taskBox.putAt(index, task);
}

Future<void> deleteTask(int index) async {
	await taskBox.deleteAt(index);
}
```

#### WidgetsFlutterBinding.ensureInitialized();
- Before Flutter runs the app, it needs to connect the Flutter engine with the framework. 
- Some code like Hive.initFlutter etc needs flutter to be ready before it runs

#### await Hive.initFlutter();
- Initializes Hive and tells it to store data inside the app's local folder
- Hive needs a file system path (where to save .hive files)
- Hive.initFlutter() automatically finds the correct folder on Android, iOS and web
- "booting up Hive"

#### why getTask is not returning Future\<List\<Task>>
- Reading from an already opened box is instant, because it will be cached

>[!warning] 
>Next session is only at the end of the workshop

#### Adding icon
- add the following dependency to **dev_dependencies**
```yaml
flutter_launcher_icons: ^0.13.1
```
- put an icon.png file in lib/assets folder
- add the following at the end of the file
```yaml
flutter_icons:  
  android: true  
  ios: true  
  image_path: "lib/assets/icon.png"
```
- run
```bash
flutter pub get
flutter pub run flutter_launcher_icons
```
#### Building the App
##### Android
- In Android Studio 
	- Build -> Flutter -> APK
- In VS Code
	- Type this in terminal

```bash
flutter build apk --release
```
 
- We will be getting a app-release.apk file in the build/app/outputs/apk/release folder
- run the following to install it in the phone

```bash
flutter install
```

