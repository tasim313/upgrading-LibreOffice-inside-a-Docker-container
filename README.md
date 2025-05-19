# 🚀 Upgrade LibreOffice in a Docker Container (Debian/Ubuntu-based)

This guide explains how to **upgrade LibreOffice** inside a Docker container (such as in a web-based application like Dolibarr or a custom app), using the official `.deb` packages from [The Document Foundation](https://www.libreoffice.org/).

> ✅ Works for Debian/Ubuntu-based containers  
> ❌ Avoid using `add-apt-repository` or PPA in Docker (Ubuntu only, and not recommended in minimal containers)

---

## 📂 Step 1: Download LibreOffice on Your Host Machine

Go to the official LibreOffice download page:

📎 [https://download.documentfoundation.org/libreoffice/stable/](https://download.documentfoundation.org/libreoffice/stable/)

Download the latest `.tar.gz` for **Linux (x86-64), DEB version**.  
Example:

```bash
wget https://download.documentfoundation.org/libreoffice/stable/25.2.2/deb/x86_64/LibreOffice_25.2.2_Linux_x86-64_deb.tar.gz -P ~/Downloads
```


📦 Step 2: Copy the File into the Docker Container
Replace <container-name> with the name of your running container (e.g., dolibarr-psql-web-1):

docker cp ~/Downloads/LibreOffice_25.2.2_Linux_x86-64_deb.tar.gz <container-name>:/tmp/

🐳 Step 3: Enter the Docker Container
```bash
   docker exec -it <container-name> bash
```

🔧 Step 4: Extract and Install LibreOffice
Inside the container:
```bash
  cd /tmp
  tar -xvzf LibreOffice_25.2.2_Linux_x86-64_deb.tar.gz
  cd LibreOffice_25.2.2.2_Linux_x86-64_deb/DEBS
  dpkg -i *.deb

```

LibreOffice will now be installed to:
/opt/libreoffice25.2

🔁 Step 5: Replace the Default libreoffice Command
Check current version:
```bash
   libreoffice --version
```

If it's still old (e.g., 6.x), update the binary path:
```bash
    mv /usr/bin/libreoffice /usr/bin/libreoffice-old
    ln -s /opt/libreoffice25.2/program/soffice /usr/bin/libreoffice
```
Optional: also link soffice directly:
```bash
    ln -s /opt/libreoffice25.2/program/soffice /usr/bin/soffice
````

✅ Step 6: Verify New Version

```bash
   libreoffice --version
```

Expected:
```bash
   LibreOffice 25.2.2.2 ...
```
