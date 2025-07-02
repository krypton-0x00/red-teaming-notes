
## 🧰 Basic Debian Package Management Commands (Using APT)

### 📦 Installing Packages

```bash
sudo apt update
sudo apt install <package-name>
```

### 🔍 Searching for Packages

```bash
apt search <package-name>
```

### 📋 Listing Installed Packages

```bash
dpkg -l | grep <package-name>
```

### 📤 Removing Packages

```bash
sudo apt remove <package-name>
```

### 🧹 Removing Package + Config Files

```bash
sudo apt purge <package-name>
```

### 🔄 Upgrading Packages

```bash
sudo apt update
sudo apt upgrade      # Upgrades installed packages
sudo apt full-upgrade # Also handles dependencies that require removing packages
```

### 🧼 Cleaning Unused Packages

```bash
sudo apt autoremove
sudo apt clean
```

---

## ⚠️ Common Errors and How to Fix Them

### ❌ 1. **"Could not get lock /var/lib/dpkg/lock"**

**Cause**: Another process (like Software Updater or APT) is using the package database.

**Fix**:

```bash
sudo rm /var/lib/dpkg/lock-frontend
sudo rm /var/lib/dpkg/lock
sudo dpkg --configure -a
```

---

### ❌ 2. **"E: Unable to locate package "**

**Cause**: The package name is incorrect, or the sources list is outdated.

**Fix**:

1. Update your package index:
    

```bash
sudo apt update
```

2. Double-check the package name.
    
3. If it’s a third-party package, add the appropriate repository or use a `.deb` file.
    

---

### ❌ 3. **"The following packages have unmet dependencies"**

**Cause**: Conflict or missing required packages.

**Fix**:

```bash
sudo apt install -f
```

This will try to fix broken dependencies.

---

### ❌ 4. **"dpkg was interrupted, you must manually run 'dpkg --configure -a'"**

**Fix**:

```bash
sudo dpkg --configure -a
```

---

### ❌ 5. **GPG Errors When Adding PPAs or Updating**

Example:

```
NO_PUBKEY ABC123XYZ...
```

**Fix**:

```bash
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys ABC123XYZ
sudo apt update
```

---

### ❌ 6. **Broken Package Errors**

```bash
E: Broken packages
```

**Fix**:

```bash
sudo apt --fix-broken install
```
