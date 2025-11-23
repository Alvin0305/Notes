
3 hour
\[2:00 -> 2:20] intro + setup -> 20 minutes
- how many didn't complete setup
- all should download the required files

\[2:20 -> 2:35] examples -> counter app -> 15 minute
\[2:35 -> 3:05] logic -> 30 minute
\[3:05 -> 3:25] database -> 20 minute
\[3:25 -> 3:35] integration -> 10 minute

\[3:35 -> 3:50] UI update -> 15 minutes
\[3:50 -> 3:55] app icon -> 5 minute
\[3:55 -> 4:05] app install -> 10 minute

\[4:05 -> 4:15] question answer session -> 10 minute

| Segment                                           | Duration                 | Lead   | Description / Activities                                                                                                                                                                                        | Notes                                                                                                    |
| ------------------------------------------------- | ------------------------ | ------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------- |
| **1. Introduction & Basics of Flutter**           | **0:00 – 0:25 (25 min)** | Friend | - What is Flutter, why Flutter- Pros & Cons- Architecture overview (Flutter Engine, Dart, Widgets)- What are Widgets?- Stateless vs Stateful Widgets (concept only)                                             | Keep slides simple and visual. Use 1–2 mini examples for clarity.                                        |
| **2. Creating a New Project & Exploring Widgets** | **0:25 – 0:45 (20 min)** | You    | - Create a new Flutter project live- Explain: `main.dart`, `MaterialApp`, `Scaffold`- Show basic widgets: `Center`, `Text`, `Row`, `Column`, `ListView.builder()`- Explain `runApp()` and `build()`             | Make it interactive — ask attendees what kind of layout they’d build.                                    |
| **3. Stateless vs Stateful (Hands-On)**           | **0:45 – 1:05 (20 min)** | You    | - Create a simple **static page** (StatelessWidget)- Then create a **counter app** (StatefulWidget)- Explain `setState()` clearly                                                                               | Keep examples very minimal but visual (like color or count changes).                                     |
| **4. Building the To-Do List App (Base Version)** | **1:05 – 1:35 (30 min)** | You    | - Create `Task` class (with `name` & `completed`)- Hardcode a few tasks- Display using `ListView.builder()`- Add check/uncheck & delete functionality- Add button to “add hardcoded task”                       | Attendees should code along here — key hands-on phase.                                                   |
| **5. Adding Input Dialog for New Tasks**          | **1:35 – 1:50 (15 min)** | You    | - Add `showDialog()` with `TextField` to input task name- Add to list dynamically                                                                                                                               | Keep code clean and explain `TextEditingController` briefly.                                             |
| **6. Short Break / Q&A**                          | **1:50 – 2:00 (10 min)** | Both   | Take questions or recap                                                                                                                                                                                         | Keeps attention fresh for the next (more technical) segment.                                             |
| **7. Introducing Local Databases (Theory)**       | **2:00 – 2:15 (15 min)** | You    | - Why we need persistent storage- Explain Hive, Firebase, SQLite- Why Hive fits here (lightweight, local, easy for beginners)                                                                                   | Use slides or diagrams to make it visual.                                                                |
| **8. Integrating Hive (Live Coding)**             | **2:15 – 2:40 (25 min)** | You    | - Add Hive dependency- Do `flutter pub get`- Annotate model with `@HiveType`, `@HiveField`- Explain `part` and code generation- Run build_runner & show generated `task.g.dart`- Initialize Hive in `main.dart` | Keep explaining **why** each step is needed, not just the code.                                          |
| **9. Creating Database Service (CRUD)**           | **2:40 – 2:55 (15 min)** | You    | - Create `database_service.dart`- Implement CRUD methods (add, get, update, delete)- Explain `async`/`await` briefly- Integrate service with UI                                                                 | This might run slightly over time if questions come up — be ready to shorten Hive intro a bit if needed. |
| **10. UI Enhancements & Styling**                 | **2:55 – 3:15 (20 min)** | Friend | - Polish UI: colors, layout, icons, spacing- Optional: Add divider or checkbox styling                                                                                                                          | Good visual closure — makes the app look complete and motivates attendees.                               |
| **11. Wrap-Up & QA**                              | **3:15 – 3:30 (15 min)** | Both   | - Recap what was learned- Mention next steps (Firebase, Provider, API integration)- Take final questions                                                                                                        | End with motivation + resource links                                                                     |
