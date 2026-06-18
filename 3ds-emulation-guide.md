# Dual-Screen 3DS Emulation Setup Guide (TV + iPad Touchscreen)

This guide covers how to set up a dual-display 3DS emulation environment using a TV for the top screen, an iPad Pro (with Apple Pencil) as the bottom touch screen, and a Python script to handle window management automatically.

---

## 1. Hardware Dumps & Prerequisites

You need a physical 3DS with custom firmware to get the necessary system files. If your console isn't modded yet, use the standard [3ds.hacks.guide](https://3ds.hacks.guide/).

### Dumping the Home Menu Applet
To boot straight into the official 3DS Home Menu on your PC, you have to dump the system files from your console.
1. Power off the 3DS. Hold **START** and power it back on to boot into **GodMode9**.
2. Go to `[1:] SYSNAND CTRNAND` → `title` → `00040030`.
3. Find your region's Home Menu Title ID folder:
   * **USA:** `00008f02` (or uppercase `00008F02`)
   * **EUR:** `00009802`
   * **JPN:** `00008202`
4. Press **A** on the folder, choose **Build CIA from file/folder**, and let it finish.
5. Take the SD card to your PC and copy the resulting `.cia` file.

---

## 2. Game Library & Conversions

The 3DS Home Menu only recognizes games installed in **.CIA** format. If your backups are currently `.cci` or `.3ds` files, they need to be handled first.

* **Sourcing Games:** If you are downloading backups, updates, or DLC directly, using **hShop** (`https://hshop.erista.me`) is easiest because the files are already decrypted `.cia` packages ready to install.
* **Decrypting Encrypted `.CIA` Files:** If you run into an encrypted `.cia` file that won't install, drop it into the **Batch CIA 3DS Decryptor** folder on your PC and run `Batch CIA 3DS Decryptor.bat`. It decrypts the file in place without needing extra keys.
* **Converting Decrypted `.CCI` to `.CIA`:** If you have decrypted `.cci` files, install the AES python dependency (`pip install pyaes`), put **`3dsconv.py`** in your ROMs folder, and run:
  ```powershell
  python 3dsconv.py --ignore-encryption *.cci
  ```

---

## 3. Emulator & Display Configuration

### Step 1: Setting up Azahar
1. Open the **Azahar** emulator.
2. Go to **File → Install CIA** and select your dumped Home Menu `.cia` file to set up the system environment.
3. Install your game `.cia` files using the exact same method.
4. In the layout settings, make sure the screens are set to **Un-docked/Split** so they run as separate windows.

### Step 2: Spacedesk Configuration
To use an iPad as the bottom touch screen:
1. Install the **Spacedesk** driver on your PC and the Spacedesk app on the iPad.
2. Connect the iPad over your local network or via a USB cable for better stability.
3. Open Windows Display Settings and position the virtual iPad screen where it makes sense for your physical layout:
   * **Side-by-Side:** Position it to the right of your main monitor (X: 1920, Y: 0).
   * **Vertical / Stacked:** Position it directly below your main monitor (X: 0, Y: 1080).

---

## 4. Automation Script

Managing three separate emulator windows manually every time you boot up gets tedious. This script launches the Home Menu, bypasses the desktop UI, waits for the emulator to initialize, moves the screens to their correct displays, and fullscreens them.

Make sure you run `pip install pygetwindow pyautogui` before using this.

<details>
<summary>Click to expand Python Launch Script</summary>

```python
import os
import glob
import subprocess
import time
import pygetwindow as gw
import pyautogui

# ==============================================================================
# CONFIGURATION
# ==============================================================================
AZAHAR_PATH = r"C:\Program Files\Azahar\azahar.exe"

# Layout choices: "horizontal" (iPad to the right) or "vertical" (iPad below the TV)
LAYOUT_STYLE = "vertical" 

if LAYOUT_STYLE == "horizontal":
    TV_X, TV_Y = 0, 0
    LAPTOP_X, LAPTOP_Y = 1920, 0 
else:  # vertical
    TV_X, TV_Y = 0, 0
    LAPTOP_X, LAPTOP_Y = 0, 1080  # Assumes a standard 1080p main display
# ==============================================================================

# Locate the USA Home Menu system file automatically
AZAHAR_USER_DIR = os.path.expandvars(r"%APPDATA%\Azahar")
home_menu_dir = os.path.join(AZAHAR_USER_DIR, "nand", "00000000000000000000000000000000", "title", "00040030")
target_folder = "00008f02" if os.path.exists(os.path.join(home_menu_dir, "00008f02")) else "00008F02"
search_path = os.path.join(home_menu_dir, target_folder, "content", "*.app")
app_files = glob.glob(search_path)

if not app_files:
    print("Error: Could not find the USA 3DS Home Menu system file.")
    exit()

HOME_MENU_FILE = app_files[0]

# Launch the Home Menu OS
subprocess.Popen([AZAHAR_PATH, HOME_MENU_FILE])

# Flat 8-second delay to let the OS boot and windows settle completely
time.sleep(8)  

main_window = None
second_window = None

# Identify the active windows
for win in gw.getAllWindows():
    title = win.title.lower()
    if "azahar" in title:
        if title == "azahar":
            second_window = win
        elif "2125" in title:
            if win.width < 1200:  # Skips the background game manager window
                main_window = win

# Position and Fullscreen the Top Screen (TV)
if main_window:
    main_window.restore() 
    main_window.moveTo(TV_X, TV_Y)
    main_window.activate()
    time.sleep(0.5)
    pyautogui.press('f11')
    time.sleep(1.5)  # Wait for transition animation

# Position and Fullscreen the Bottom Screen (iPad Touchscreen)
if second_window:
    second_window.restore()
    second_window.moveTo(LAPTOP_X, LAPTOP_Y)
    second_window.activate()
    time.sleep(0.5)
    pyautogui.press('f11')
```

</details>

---
Save the script as `launch_home_menu.py`. You can point a custom shortcut in a front-end or launcher (like the Xbox App) directly to this file to load straight into the 3DS ecosystem hands-free.