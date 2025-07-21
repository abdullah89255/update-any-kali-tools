# update-any-kali-tools
Great question! ✅ Keeping Kali tools up to date is essential for accurate scanning, fresh exploits, and fewer bugs.

Here’s how to **update any tool on Kali Linux**, depending on how it was installed.

---

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
