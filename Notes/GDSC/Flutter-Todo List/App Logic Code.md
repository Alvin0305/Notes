Good afternoon guys. So today we will be learning how to build a basic Todo list app in Flutter. So by this workshop you guys will be able to build basic apps in Flutter. I hope you all have already setup flutter in your devices. So lets get started. So, first, we can create a flutter app
- create the flutter app
- might take some time
```bash
flutter create todo_list
```

>[!question]
>- What is a widget?
>- What is stateless and stateful widgets?
#### Widgets
- Flutter is 100% widget based
- Visible Widgets
	- Text, Image, Icon, Button, etc
- Invisible/Layout Widgets
	- Row, Column, Container, Padding, etc
- Everything in flutter is a widget
- We can build UI using widgets and its children by nesting them.
#### Stateless Widgets
- Widgets which don't change after it is built
- example: label, image, etc
- UI is static
#### Stateful Widgets
- Widgets which changes its appearance dynamically when the state changes
- use setState() to update the state
- example: counters, forms, animations, toggles

>[!question] 
>- What is a MaterialApp?
>- What is a Scaffold?
#### Material App
- Main container of a Flutter App. 
- Tells flutter, that we are using Material UI
#### Scaffold
- Wrapper for the App
- Ready made layout
- Provides places to put
	- App Bar
	- Body
	- Floating Action Button
	- Drawer (side bar)

#### Teach how the widget tree work
```dart
import 'package:flutter/material.dart';
  
void main() {
	runApp(MyApp());
}
  
class MyApp extends StatelessWidget {
	const MyApp({super.key});
  
	@override
	Widget build(BuildContext context) {
		return MaterialApp(
			debugShowCheckedModeBanner: false,
			home: Scaffold(
				appBar: AppBar(
					title: const Text(
						"Todo List App",
						style: TextStyle(color: Colors.white),
					),
					backgroundColor: Colors.deepPurpleAccent,
				),
				body: Center(
					child: TextButton(onPressed: () {}, child: const Text("Button")),
				),
				floatingActionButton: FloatingActionButton(
					onPressed: () {},
					child: Icon(Icons.add),
				),
			),
		);
	}
}
```
- in this way we can build UI by nesting widgets inside on another

#### Useful Widgets
######  Column
>[!warning] 
>- Will take the entire height of the app
>- Don't use CrossAxisAlignment.baseline 
>- Show CrossAxisAlignment.start and end only for text and icon, not for containers.
	 - MainAxisAlignment.start, end, spaceEvenly, spaceBetween
	 - CrossAxisAlignment.start, end, stretch
	 - Main Axis Size
```dart
import 'package:flutter/material.dart';

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      debugShowCheckedModeBanner: false,
      home: Scaffold(
        appBar: AppBar(
          title: const Text(
            "Todo List App",
            style: TextStyle(color: Colors.white),
          ),
          backgroundColor: Colors.deepPurpleAccent,
        ),
        body: Center(
          child: Column(
            mainAxisAlignment: MainAxisAlignment.center,
            crossAxisAlignment: CrossAxisAlignment.start,
            children: [Text("Hello"), Text("World"), Icon(Icons.add)],
          ),
        ),
        floatingActionButton: FloatingActionButton(
          onPressed: () {},
          child: Icon(Icons.add),
        ),
      ),
    );
  }
}
```

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      debugShowCheckedModeBanner: false,
      home: Scaffold(
        appBar: AppBar(
          title: const Text(
            "Todo List App",
            style: TextStyle(color: Colors.white),
          ),
          backgroundColor: Colors.deepPurpleAccent,
        ),
        body: Center(
          child: Column(
            mainAxisAlignment: MainAxisAlignment.spaceEvenly,
            crossAxisAlignment: CrossAxisAlignment.stretch,
            children: [
              Container(color: Colors.red, width: 100, height: 100),
              Container(color: Colors.green, width: 100, height: 100),
              Container(color: Colors.blue, width: 100, height: 100),
            ],
          ),
        ),
        floatingActionButton: FloatingActionButton(
          onPressed: () {},
          child: Icon(Icons.add),
        ),
      ),
    );
  }
}
```
###### ROW
- change the Column to Row
- change CrossAxisAlignment from stretch to center
```dart
import 'package:flutter/material.dart';

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      debugShowCheckedModeBanner: false,
      home: Scaffold(
        appBar: AppBar(
          title: const Text(
            "Todo List App",
            style: TextStyle(color: Colors.white),
          ),
          backgroundColor: Colors.deepPurpleAccent,
        ),
        body: Center(
          child: Row(
            mainAxisAlignment: MainAxisAlignment.spaceEvenly,
            crossAxisAlignment: CrossAxisAlignment.center,
            children: [
              Container(color: Colors.red, width: 100, height: 100),
              Container(color: Colors.green, width: 100, height: 100),
              Container(color: Colors.blue, width: 100, height: 100),
            ],
          ),
        ),
        floatingActionButton: FloatingActionButton(
          onPressed: () {},
          child: Icon(Icons.add),
        ),
      ),
    );
  }
}
```
#### Explain Stateful using a counter app
main.dart
```dart
import 'package:flutter/material.dart';
import 'package:todo_list/screens/tasks_screen.dart';

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(debugShowCheckedModeBanner: false, home: TasksScreen());
  }
}

```

screens/tasks_screen.dart
```dart
import 'package:flutter/material.dart';

class TasksScreen extends StatefulWidget {
  const TasksScreen({super.key});

  @override
  State<TasksScreen> createState() => _TasksScreenState();
}

class _TasksScreenState extends State<TasksScreen> {
  int count = 0;

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text("Counter", style: TextStyle(color: Colors.white)),
        backgroundColor: Colors.deepPurpleAccent,
      ),
      body: Center(child: Text("$count")),
      floatingActionButton: FloatingActionButton(
        onPressed: () {
          setState(() {
            count++;
          });
        },
        child: Icon(Icons.add),
      ),
    );
  }
}
```

#### Start building the Todo App
- Create the task.dart file
```dart
class Task {
  String name;
  bool completed;

  Task({required this.name, required this.completed});
}
```

- Update the task_screen.dart file with hard coded tasks
```dart
import 'package:flutter/material.dart';
import 'package:todo_list/models/task.dart';

class TasksScreen extends StatefulWidget {
  const TasksScreen({super.key});

  @override
  State<TasksScreen> createState() => _TasksScreenState();
}

class _TasksScreenState extends State<TasksScreen> {
  List<Task> tasks = [
    Task(name: "Brush Teeth", completed: false),
    Task(name: "Bath", completed: false),
    Task(name: "Go to School", completed: false),
  ];

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text("Todo List", style: TextStyle(color: Colors.white)),
        backgroundColor: Colors.deepPurpleAccent,
      ),
      body: ListView.builder(
        itemCount: tasks.length,
        itemBuilder: (context, index) {
          Task task = tasks[index];

          return ListTile(
            leading: Checkbox(
              value: task.completed,
              onChanged: (value) {
                print("pressed check box");
              },
            ),
            title: Text(task.name),
            trailing: IconButton(onPressed: () {}, icon: Icon(Icons.delete)),
          );
        },
      ),
      floatingActionButton: FloatingActionButton(
        onPressed: () {},
        child: Icon(Icons.add),
      ),
    );
  }
}
```

- add delete method and hard coded add method
```dart
trailing: IconButton(
  onPressed: () {
	setState(() {
	  tasks.remove(task);
	});
  },
  icon: Icon(Icons.delete),
),
```

```dart
floatingActionButton: FloatingActionButton(
        onPressed: () {
          setState(() {
            tasks.add(Task(name: "New Task", completed: false));
          });
        },
        child: Icon(Icons.add),
      ),
```

- add the dialog
- handle closing of dialog box
```dart
floatingActionButton: FloatingActionButton(
        onPressed: () {
          showDialog(
            context: context,
            builder: (context) {
              return AlertDialog(
                title: const Text("Add a new Task"),
                content: TextField(controller: taskNameController),
                actions: [
                  TextButton(
                    onPressed: () {
                      taskNameController.clear();
                      Navigator.of(context).pop();
                    },
                    child: const Text("Cancel"),
                  ),
                  ElevatedButton(
                    onPressed: () {
                      String taskName = taskNameController.text.trim();
                      if (taskName.isEmpty) return;

                      setState(() {
	                    Task newTask = Task(name: taskName, complete: false);
                        tasks.add(newTask);
                      });

                      taskNameController.clear();
                      Navigator.of(context).pop();
                    },
                    child: const Text("OK"),
                  ),
                ],
              );
            },
          );
        },
        child: Icon(Icons.add),
      ),
```

- explain null safety and add the update method for check box
	- explain ! and ??
```dart
leading: Checkbox(
              value: task.completed,
              onChanged: (value) {
                Task updatedTask = Task(
                  name: task.name,
                  completed: value ?? false,
                );

                setState(() {
                  tasks[index] = updatedTask;
                });
              },
            ),
```

- Now the app is working. Next we wanna add a database
