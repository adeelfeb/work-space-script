# Ubuntu Workspace Automation (One-Click Layouts)

This setup allows you to **launch predefined workspace layouts** (apps + order) on Ubuntu using a **single click from the Dock**.
It is ideal for **Work / Gaming / Study** profiles.

The solution is based on:

* `wmctrl` (window & workspace control)
* simple Bash scripts
* `.desktop` launchers (Dock integration)

---

## 1. Prerequisites

Install the required tool once:

```bash
sudo apt update
sudo apt install wmctrl -y
```

> `wmctrl` is used to switch workspaces and launch apps in sequence.

---

## 2. Folder Structure (Recommended)

Create a dedicated scripts directory:

```bash
mkdir -p ~/scripts
```

This is where **all future workspace scripts** should live.

Example:

```
~/scripts/
├── work-layout.sh
├── gaming-layout.sh
├── study-layout.sh
```

---

## 3. Creating a Workspace Script

Create a new script:

```bash
nano ~/scripts/work-layout.sh
```

Example script (Workspace 3 → 2 → 1 order):

```bash
#!/bin/bash

# Ensure enough workspaces
wmctrl -n 4

############################
# Workspace 3 — Chrome + Edge
############################
wmctrl -s 2
google-chrome --new-window --profile-directory="Profile 3" &
sleep 6
microsoft-edge --new-window &
sleep 6

############################
# Workspace 2 — Cursor + Chrome
############################
wmctrl -s 1
cursor &
sleep 5
google-chrome --new-window --profile-directory="Profile 1" &
sleep 6

############################
# Workspace 1 — Brave
############################
wmctrl -s 0
brave-browser --new-window &
sleep 5

exit 0
```

### Notes

* Workspace numbers start from **0**
* `sleep` is required so GNOME has time to open each app
* GNOME may switch workspaces while apps open (this is expected behavior)

---

## 4. Make the Script Executable

After saving the file:

```bash
chmod +x ~/scripts/work-layout.sh
```

Test it:

```bash
~/scripts/work-layout.sh
```

If apps open in the correct workspaces, the script is ready.

---

## 5. Creating a Dock (Taskbar) Launcher

To run the script with **one click**, create a `.desktop` file.

```bash
nano ~/.local/share/applications/work-layout.desktop
```

Paste the following (adjust username if needed):

```ini
[Desktop Entry]
Type=Application
Name=Work Layout
Exec=/home/ali-mobile/scripts/work-layout.sh
Icon=utilities-terminal
Terminal=false
Categories=Utility;
```

Save and refresh:

```bash
update-desktop-database ~/.local/share/applications
```

---

## 6. Add Script to the Ubuntu Dock

1. Press **Super**
2. Search for **Work Layout**
3. Right-click → **Add to Favorites**

🎯 Now your entire workspace layout launches with **one click**.

---

## 7. Adding New Layouts in the Future

To add another setup (example: Gaming):

```bash
cp ~/scripts/work-layout.sh ~/scripts/gaming-layout.sh
nano ~/scripts/gaming-layout.sh
```

Modify the apps inside.

Then create a launcher:

```bash
nano ~/.local/share/applications/gaming-layout.desktop
```

Example:

```ini
[Desktop Entry]
Type=Application
Name=Gaming Layout
Exec=/home/ali-mobile/scripts/gaming-layout.sh
Icon=applications-games
Terminal=false
Categories=Game;
```

Refresh and add it to the Dock the same way.

---

## 8. Best Practices

* Close existing app windows before running a layout for clean results
* Keep all scripts in `~/scripts`
* Use clear names: `work`, `gaming`, `study`
* Duplicate scripts instead of rewriting from scratch

---

## 9. Known GNOME Limitations (Important)

* GNOME **will switch workspaces while apps open**
* Background-only opening without switching is **not supported**
* This script provides **correct final placement**, which is the most stable approach on GNOME

---

## 10. Result

✔ One-click workspace setup
✔ Reusable & expandable
✔ No third-party daemons
✔ Safe for daily use
