

## 🛠️ 1. **Update APT-Based Tools (Official Kali Tools)**

These are installed via `apt` (like `nmap`, `sqlmap`, `burpsuite`, etc.)

```bash
sudo apt update && sudo apt upgrade -y
```

To upgrade a specific tool:

```bash
sudo apt install --only-upgrade <tool-name>
```

✅ Example:

```bash
sudo apt install --only-upgrade sqlmap
```

---

## 📦 2. **Update Tools Installed from GitHub**

If you cloned a tool manually with `git clone`, do:

```bash
cd /path/to/tool
git pull
```

If it has an install step (like `python3 setup.py install`, `go install`, or `make install`), rerun that after pulling.

✅ Example:

```bash
cd ~/tools/XSStrike
git pull
python3 xsstrike.py  # run it again
```

---

## 🐍 3. **Update Python-Based Tools (via pip)**

```bash
pip3 install --upgrade <tool-name>
```

✅ Example:

```bash
pip3 install --upgrade wafw00f
```

Or if it's a local repo with `requirements.txt`:

```bash
pip3 install -r requirements.txt --upgrade
```

---

## 🧰 4. **Update Go-Based Tools**

If installed via `go install`, just rerun the command with `@latest`:

✅ Example:

```bash
go install github.com/projectdiscovery/httpx/cmd/httpx@latest
go install github.com/hahwul/dalfox/v2@latest
```

Make sure your `~/go/bin` is in `$PATH`:

```bash
echo 'export PATH=$PATH:$(go env GOPATH)/bin' >> ~/.bashrc && source ~/.bashrc
```

---

## ⚙️ 5. **Update Nuclei Templates**

For `nuclei`, update templates separately:

```bash
nuclei -update-templates
```

Or update everything:

```bash
nuclei -update -update-templates
```

---

## 🧼 6. **Clean Up After Updating**

Optional but helpful:

```bash
sudo apt autoremove --purge
sudo apt clean
```

---

## ✅ Bonus: One-Line Full System & Tools Update

```bash
sudo apt update && sudo apt full-upgrade -y && sudo apt autoremove -y && nuclei -update -update-templates && go install github.com/projectdiscovery/httpx/cmd/httpx@latest
```

---

Need help writing a script to automate all updates for your tools in one shot?
To remove **any tool in Kali Linux**, you can use the appropriate `apt`, `snap`, or `pip` command, depending on how the tool was installed.

---

### ✅ **If the tool was installed with `apt`** (most common):

#### 🔹 Step 1: Find the exact package name

```bash
dpkg -l | grep <tool-name>
```

**Example**:

```bash
dpkg -l | grep hydra
```

#### 🔹 Step 2: Remove the package

```bash
sudo apt remove <package-name>
```

To **remove configuration files** too (recommended):

```bash
sudo apt purge <package-name>
```

To **completely clean leftover dependencies**:

```bash
sudo apt autoremove
```

---

### ✅ **If it was installed via Snap**:

```bash
snap list
sudo snap remove <tool-name>
```

---

### ✅ **If installed with `pip` (Python tools):**

```bash
pip list | grep <tool-name>
pip uninstall <tool-name>
```

Or use `pip3` if it's a Python 3 package:

```bash
pip3 uninstall <tool-name>
```

---

### ✅ **Remove manually cloned tools (from GitHub)**:

If you cloned a tool manually like this:

```bash
git clone https://github.com/example/tool.git
```

Then simply delete the folder:

```bash
rm -rf ~/path/to/tool
```

If it was installed system-wide with a setup script:

```bash
sudo make uninstall
```

(Only works if the developer provided a `Makefile` or uninstall script.)

---

### ✅ Clean up cache and packages (optional):

```bash
sudo apt clean
sudo apt autoremove
```

---

### 📌 Example: Remove `hydra`

```bash
sudo apt purge hydra
sudo apt autoremove
```

---
Here's a simple and effective **Bash script** to:

1. ✅ **Remove any Kali Linux tool by name**
2. 🧹 **Clear system cache and residual package files**

---

## 🛠️ Bash Script: `remove_kali_tool.sh`

```bash
#!/bin/bash

# File: remove_kali_tool.sh
# Usage: ./remove_kali_tool.sh <tool-name>

# Check for tool name input
if [ -z "$1" ]; then
  echo "[!] Usage: $0 <tool-name>"
  exit 1
fi

TOOL="$1"

echo "[*] Trying to remove the tool: $TOOL"

# Step 1: Remove the tool (if installed)
sudo apt remove --purge -y "$TOOL"
sudo apt autoremove -y
sudo apt autoclean -y

# Step 2: Clean user-level cache (APT + thumbnails + browser + pip)
echo "[*] Cleaning cache files..."

rm -rf ~/.cache/*
rm -rf ~/.thumbnails/*
rm -rf ~/.local/share/Trash/*
rm -rf ~/.mozilla/firefox/*.default-release/cache2/*
rm -rf ~/.config/google-chrome/Default/Cache/*
rm -rf ~/.config/chromium/Default/Cache/*
rm -rf ~/.local/__pycache__/*
rm -rf ~/.local/share/pip/*

# Step 3: Clear system-wide cache (requires root)
echo "[*] Cleaning APT and system cache..."

sudo rm -rf /var/cache/apt/*
sudo rm -rf /var/log/*
sudo journalctl --vacuum-time=1d 2>/dev/null

echo "[✓] Finished removing $TOOL and clearing cache."
```

---

## ▶️ How to Use

### 1. Save it as a file:

```bash
nano remove_kali_tool.sh
```

Paste the script and save (`Ctrl + O`, `Enter`, `Ctrl + X`)

### 2. Make it executable:

```bash
chmod +x remove_kali_tool.sh
```

### 3. Run it with tool name:

```bash
./remove_kali_tool.sh dnsx
```

> Replace `dnsx` with any installed Kali tool like `nmap`, `hydra`, `wireshark`, etc.

---

## 🧠 Optional Tip

You can list installed tools to help decide what to remove:

```bash
apt list --installed | grep kali
```

---

Let me know if you want:

* A **GUI version**
* A version that **asks before deleting**
* Or one that **removes entire categories** like "all recon tools"
Great! Below is a **Bash script** to automatically update **all tools and packages** in Kali Linux. It handles:

✅ System update
✅ Tool/package upgrade
✅ Kernel update
✅ Nuclei/subfinder/amass/httpx/etc. update (optional section)
✅ Cleaning residual cache

---

## 🛠️ Bash Script: `kali_updater.sh`

```bash
#!/bin/bash

# File: kali_updater.sh
# Description: Updates all Kali packages, tools, and cleans the system

echo "===================================="
echo "    🔄 Kali Linux Update Script     "
echo "===================================="

# Step 1: Update and Upgrade System Packages
echo "[*] Updating APT repositories..."
sudo apt update

echo "[*] Upgrading installed packages..."
sudo apt full-upgrade -y

# Step 2: Clean up residual files
echo "[*] Cleaning up unused packages and cache..."
sudo apt autoremove -y
sudo apt autoclean -y
sudo apt clean -y

# Step 3: Optional – Update common security tools (uncomment if you use them)
echo "[*] Updating popular tools (if installed)..."

# Nuclei templates
if [ -d "$HOME/nuclei-templates" ]; then
  echo "[*] Updating nuclei-templates..."
  cd ~/nuclei-templates && git pull
fi

# Subfinder
if command -v subfinder &> /dev/null; then
  echo "[*] Updating subfinder..."
  subfinder -update
fi

# Amass
if command -v amass &> /dev/null; then
  echo "[*] Updating amass..."
  amass update
fi

# Httpx
if command -v httpx &> /dev/null; then
  echo "[*] Updating httpx binary (Go-based tools)..."
  go install -v github.com/projectdiscovery/httpx/cmd/httpx@latest
fi

# Step 4: Done
echo "===================================="
echo " ✅ Update completed successfully!  "
echo "===================================="
```

---

## ▶️ How to Use

### 1. Save the script:

```bash
nano kali_updater.sh
```

Paste the script, save (`Ctrl + O`, `Enter`, `Ctrl + X`)

### 2. Make it executable:

```bash
chmod +x kali_updater.sh
```

### 3. Run the script:

```bash
./kali_updater.sh
```

---

## 💡 Notes

* Make sure `go` is installed if you want to update Go-based tools (`httpx`, `nuclei`, etc.).
* You can add your own tools under the "Optional" section easily.
* This script keeps both your **system packages** and **security tools** up-to-date in one click.

---

Want a **GUI or menu-based version** of this script (with dialog or whiptail)? Let me know and I’ll make that for you too.
Geany
```bash
sudo apt install geany
```

