### Download Required Files
1. **Download Android Studio**  [https://developer.android.com/studio](https://developer.android.com/studio)
2. **Download Flutter SDK (Windows)**[https://storage.googleapis.com/flutter_infra_release/releases/stable/windows/flutter_windows_3.35.6-stable.zip](https://storage.googleapis.com/flutter_infra_release/releases/stable/windows/flutter_windows_3.35.6-stable.zip)
---
### Install the Files
1. **Extract Flutter SDK**
    - Unzip the downloaded Flutter SDK file.
    - Move the extracted folder to:  `C:\Flutter\`
2. **Install Android Studio**
    - Run the downloaded Android Studio installer.
    - Continue the installation by pressing **Next** through the steps.
    - When the **License Agreement** page appears, click **Accept** and press **Next**.
    - Wait for all components to download and install — this may take a few minutes.
3. **Set Up Plugins**
    - Once installed, open **Android Studio**.
    - In the **Welcome to Android Studio** window:
        - Go to **Plugins → Marketplace**.
        - Search and install **Flutter**.
        - Search and install **Dart**.
    - After installation, click **Restart IDE** when prompted.
4. **Add Flutter to Environment Variables**   
    - Search for environment variables in Start. And open it
    - Under **System variables**, select **Path** → click **Edit** → click **New**.
    - Add the following path:
        `C:\Flutter\flutter\bin`
    - Click **OK** on all windows to save changes.
    - To verify, open **Command Prompt** and run:
        `flutter --version`
        You should see the Flutter version printed on the screen.
---
### Create Your First Flutter Project
1. After restarting, click **New Flutter Project** on the welcome screen.
2. In the **New Project** window:
    - On the left sidebar, select **Flutter**.
    - In the **Flutter SDK Path** field, browse to: `C:\Flutter\flutter`
    - Click **Next**.
3. Enter a **project name** and **location**, then click **Create**.
4. Wait for Android Studio to finish creating the project.
---
###  Connect Your Android Device
1. **Enable Developer Options**
    - Open **Settings → About phone → Software information**.
    - Tap **Build number** (or **Version number**) **7 times** to enable **Developer Options**.
2. **Enable USB Debugging**
    - In **Settings**, search for **USB Debugging** and enable it.
3. **Connect Your Phone**
	- Connect your phone to the laptop using a **USB cable**.
	- Ensure it is **not in “Charging only” mode**.
	- Allow all required permissions and trust prompts.
---
### Run the Flutter App
1. In Android Studio, locate the **device dropdown** at the top (it lists Chrome, Edge, Windows, and your phone name).
2. Select your **phone** from the list.
    - If your phone is not visible:
        - Click **Restart Flutter Daemon**.
        - Or **restart Android Studio** and try again.
        - If it still not works click the settings icon in the top right of Android Studio and select SDK Manager. 
        - From there you can see the Android SDK Location. 
        - Copy it and open cmd and enter the following and restart android studio
```bash
flutter config --android-sdk <path you copied>
```
1. Click the **Run (▶) button** on the top toolbar.
2. Wait for the build to complete — this may take some time on the first run.
3. Once done, the **default Flutter counter app** should appear on your phone screen.

