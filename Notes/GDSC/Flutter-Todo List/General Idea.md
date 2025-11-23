### 🧠 Flutter Hive Workshop Notes

#### 🏁 Introduction
- In most apps, we need to **store data permanently** — like a to-do list, user preferences, or saved notes.
- When we close and reopen the app, that data should **still be there**.
- This is where a **database** helps.
#### 💾 Why We Need a Database
- When we use **variables** or **lists** in Flutter, data is stored **in memory**.
- Once the app is closed, all that data is lost.
- A **database** stores data permanently on the **device storage**.
- Databases let us:
    - Save, read, update, and delete data easily (CRUD operations)
    - Handle large amounts of structured data
    - Retrieve data quickly without using files manually

💡 Example:  
If you’re making a **To-Do app**, you want tasks to remain even after the app is closed — a database makes that possible.

#### 🐝 What is Hive?
- **Hive** is a **lightweight, fast, NoSQL database** for Flutter.
- It works completely **offline** — no internet or external server needed.
- Stores data in the app’s **local file system** as binary files.
- Perfect for small to medium apps that don’t need a complex database like SQLite.
#### ⚡ Why choose Hive?
- Super fast (written in pure Dart)
- Works on Android, iOS, Web, and Desktop
- No SQL queries needed — just use boxes like simple maps or lists
- Easy to use and integrate into any Flutter app
#### 🧩 How Hive Works (Simple View)
- **Box:** A storage container (like a table or folder).  
    Example: A `tasksBox` for storing Task objects.
- **Adapter:** A translator that tells Hive how to save and read custom Dart objects.
- **Model:** The Dart class that represents your data (like `Task`).   
- **Hive File:** The file saved on the device which contains all the stored data.

💡 Think of it like this:
> You put your Dart objects inside a Box → Hive Adapter converts them → and saves them in a file.

- **Difference between Hive and SQLite**
    - Hive is simpler, schema-free (NoSQL)
    - SQLite uses SQL queries, structured data model
    - Hive is faster for small data and key-value storage
- **Offline capability**
    - Hive works offline by default — data is saved locally
    - Perfect for apps like notes, to-do lists, user preferences
- **Security**
    - Hive supports **encryption** using `HiveAesCipher` for sensitive data

### 🧰 Other Database Options in Flutter

| Database                     | Type                    | When to Use                                    | Pros                        | Cons                              |
| ---------------------------- | ----------------------- | ---------------------------------------------- | --------------------------- | --------------------------------- |
| **Hive**                     | NoSQL (key–value)       | Small apps, offline storage                    | Fast, simple, pure Dart     | Not ideal for complex queries     |
| **SQLite (sqflite package)** | SQL (Relational)        | Apps needing relationships (users–posts, etc.) | Powerful querying           | More setup, manual queries        |
| **Drift (formerly Moor)**    | SQL + ORM               | Apps needing both SQL and Dart features        | Type-safe queries, reactive | Bit advanced for beginners        |
| **ObjectBox**                | NoSQL                   | High performance, reactive data                | Fast, auto-updates UI       | Setup can be tricky               |
| **Firebase Firestore**       | Cloud database          | Online apps needing sync between devices       | Real-time sync, scalable    | Needs internet and Firebase setup |
| **SharedPreferences**        | Key–value (lightweight) | For storing simple data like theme or username | Simple                      | Not for structured or big data    |
