### Download Required Files
1. **Download Android Studio (macOS)**  
    [https://developer.android.com/studio](https://developer.android.com/studio)
2. **Download Flutter SDK (macOS)**  
    https://storage.googleapis.com/flutter_infra_release/releases/stable/macos/flutter_macos_3.35.6-stable.zip
---
### Install the Files
#### 1. **Extract Flutter SDK**
Open the **Terminal** and run:
```bash
cd ~ 
mkdir development 
cd development 
unzip ~/Downloads/flutter_macos_3.35.6-stable.zip
```

> This extracts Flutter to: `~/development/flutter`
---
#### 2. **Add Flutter to PATH**
Temporarily add Flutter to your PATH:
```bash
export PATH="$PATH:$HOME/development/flutter/bin"
```

To make it permanent, add the same line to your shell config:
- For **zsh** (default on macOS):

```bash
echo 'export PATH="$PATH:$HOME/development/flutter/bin"' >> ~/.zshrc source ~/.zshrc
```
- For **bash**:

```bash
echo 'export PATH="$PATH:$HOME/development/flutter/bin"' >> ~/.bash_profile source ~/.bash_profile
```  
---
#### 3. **Verify Installation**
Run:

```bash
flutter doctor
```
You’ll see checks for:
- Flutter SDK
- Android toolchain
- Xcode (for iOS builds)
- Android Studio
- Connected devices
> If you see warnings (like missing Android Studio or SDK), we’ll fix them next.
---
### Install Android Studio
1. Open the downloaded `.dmg` file for Android Studio.
2. Drag **Android Studio** into your **Applications** folder.
3. Launch it from **Launchpad** or **Spotlight (Cmd + Space → Android Studio)**.
4. Follow the setup wizard:
    - Click **Next** and install all SDK components.
    - Accept the **License Agreement**.
    - Wait for the installation to complete.
5. On the **Welcome to Android Studio** page:
    - Go to **Plugins → Marketplace**.
    - Search and install **Flutter**.
    - Search and install **Dart**.
    - Click **Restart IDE** when prompted.
---
### Create Your First Flutter Project
1. After restarting, click **New Flutter Project**.
2. In the **New Project** window:
    - On the left sidebar, select **Flutter**.
    - In **Flutter SDK Path**, set:
        `/Users/<your-username>/development/flutter`
    - Click **Next**.
3. Enter your **project name** and **location**, then click **Create**.
4. Wait for Android Studio to finish setting up the project.
---
### Connect Your Android Device
1. **Enable Developer Options** on your phone:
    - Go to **Settings → About phone → Build number (or Version number)**.
    - Tap it **7 times** to enable **Developer Options**.
2. **Enable USB Debugging**:
    - In **Settings**, search for **USB Debugging** and enable it.
3. **Connect your phone via USB**:
    - Use a reliable cable.
    - Make sure it’s **not in charging-only mode**.
    - Allow permissions when prompted on your phone.
4. **Check if the device is detected**:

```bash
flutter devices
```

> If you see your phone name, it’s connected successfully.
---
### Run the Flutter App
1. In Android Studio, locate the **device dropdown** (top toolbar).
    - You’ll see Chrome, macOS, or your Android device.
2. Select your **Android phone**.
3. If it doesn’t appear:
    - Click **Restart Flutter Daemon** in Android Studio, or
    - Run:

```bash
flutter doctor --android-licenses
```        

	and accept all the licenses.    

4. Click the **Run (▶)** button on the top toolbar.
5. Wait for the build to finish.
6. You’ll see the **default Flutter counter app** on your phone!