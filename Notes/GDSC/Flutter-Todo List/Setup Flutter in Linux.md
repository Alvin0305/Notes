### Download Required Files
1. **Download Android Studio (Linux version)**  
    [https://developer.android.com/studio](https://developer.android.com/studio)
2. **Download Flutter SDK (Linux)**  
    https://storage.googleapis.com/flutter_infra_release/releases/stable/linux/flutter_linux_3.35.6-stable.tar.xz
---
### Install the Files
#### 1. **Extract Flutter SDK**
Open your terminal and run:

```bash
cd ~ 
mkdir development
cd development 
tar xf ~/Downloads/flutter_linux_3.35.6-stable.tar.xz
```

>This will extract Flutter to: `~/development/flutter`

#### 2. **Add Flutter to PATH**

Temporarily add Flutter to your PATH:

`export PATH="$PATH:$HOME/development/flutter/bin"`

To make it permanent, add the same line to your shell config file:
- For **Bash** users:

```bash
echo 'export PATH="$PATH:$HOME/development/flutter/bin"' >> ~/.bashrc source ~/.bashrc
```
  
- For **Zsh** users:

```zsh
echo 'export PATH="$PATH:$HOME/development/flutter/bin"' >> ~/.zshrc source ~/.zshrc
```
#### 3. **Verify Flutter Installation**

Run:
`flutter doctor`
You’ll see checks for:
- Flutter SDK
- Android toolchain
- Android Studio
- Connected devices
> If you see warnings, don’t worry — we’ll fix them below.
---
### Install Android Studio

1. Extract the downloaded **Android Studio** file and move it to **development** folder in home
2. Start Android Studio:    

```bash
~/development/android-studio/bin/studio
```

3. Follow the setup wizard:    
    - Press **Next** to install all required SDK components.
    - Accept the **License Agreement** when prompted.
    - Wait for it to finish downloading dependencies.
4. Once the **Welcome to Android Studio** page appears:
    - Go to **Plugins → Marketplace**.
    - Search and install **Flutter**.
    - Search and install **Dart**.
    - Click **Restart IDE** when prompted.
---
### Create Your First Flutter Project
1. After restarting, click **New Flutter Project**.
2. In the **New Project** window:
    - Choose **Flutter** on the left sidebar.
    - For **Flutter SDK Path**, set it to:
        `/home/<your-username>/development/flutter`
    - Click **Next**.
3. Enter your **project name** and **location**, then click **Create**.
4. Wait for the IDE to finish setting up the project.
---
### Connect Your Android Device
1. **Enable Developer Options** on your phone:
    - Go to **Settings → About phone → Build number** (or **Version number**).
    - Tap it **7 times** to enable **Developer Options**.
2. **Enable USB Debugging:**
    - In **Settings**, search **USB Debugging** and enable it.
3. **Connect the Phone via USB:**
    - Use a good-quality USB cable.
    - Ensure it’s **not in charging-only mode**.
    - When prompted on the phone, tap **Allow USB debugging**.
4. **Check if Device is Detected:**

```bash
flutter devices
```

> If you see your device name, it’s connected successfully.
---
### Run the Flutter App
1. In Android Studio, locate the **device dropdown** (top toolbar).  
    You should see your **phone name** along with other options like Chrome or Linux.
2. Select your **phone**.
3. If your phone isn’t visible:
    - Click **Restart Flutter Daemon** in Android Studio, **or**
    - Run this in the terminal:

```bash
flutter doctor --android-licenses
```    

	- and accept all licenses    
4. Click the **Run (▶)** button.
5. Wait for the build to complete — the first time may take a bit longer.
6. Once it finishes, you’ll see the **default Flutter counter app** on your phone!
