# 🚀 Kali Linux Docker Setup with Networking Tools and Remote Desktop Access (XRDP)

This guide helps you set up a **Kali Linux container using Docker** with **networking tools**, a **lightweight GUI (XFCE)**, and **Remote Desktop Protocol (RDP)** support, all accessible from **localhost (127.0.0.1:3389)**.

---

## 🐳 Prerequisites

- ✅ Docker installed on Windows/Linux
- ✅ RDP client (e.g., `Remote Desktop Connection` on Windows or `Remmina` on Linux)
- ✅ Internet connection

---

## 🧱 Step 1: Pull Kali Linux Docker Image

```bash
docker pull kalilinux/kali-rolling
```
## 🐳 Step 2: Run the Kali Container with Port 3389 Exposed
```bash
docker run -it --name kali-rdp -p 3389:3389 kalilinux/kali-rolling /bin/bash
```
- --name kali-rdp: names the container

- -p 3389:3389: maps RDP port from container to host

## 🛠️ Step 3: Install Desktop Environment and XRDP
Inside the container:

```bash
apt update && apt upgrade -y
apt install -y xfce4 xrdp dbus-x11 sudo net-tools iproute2 iputils-ping dnsutils curl wget
```
## 🛠️ Step 4: Configure XRDP to Use XFCE
Replace XRDP's session startup script:

```bash
echo "startxfce4" > /etc/xrdp/startwm.sh
chmod +x /etc/xrdp/startwm.sh
```
## 🧪 Step 5: Start XRDP
Before starting, clean any stale PID files (if needed):

```bash
rm -f /var/run/xrdp/*.pid
/etc/init.d/xrdp start
```
Check status:

```bash
/etc/init.d/xrdp status
```
## 🔐 Step 6: Set Root or User Password
```bash
passwd
```
(Optional - add a new user):

```bash
useradd -m -s /bin/bash kaliuser
passwd kaliuser
usermod -aG sudo kaliuser
```
## 🛡️ Step 7: Install Common Networking Tools
Install useful security/networking tools:

```bash
apt install -y nmap netcat-openbsd tcpdump wireshark tshark hydra sqlmap whois traceroute aircrack-ng
```
Optional Kali metapackages:

```bash
apt install -y kali-tools-top10 kali-tools-networking
```
## 💻 Step 8: Connect via Remote Desktop (RDP)
On your Windows host:

- Open Remote Desktop Connection (mstsc)

Connect to:

```makefile
127.0.0.1:3389
```
Login using:

- Username: root or kaliuser

- Password: (the one you set)

✅ You should now see the Kali XFCE desktop inside your RDP window.

## 📦 Docker Tips
Restart container:
```bash
docker start -ai kali-rdp
```
Stop container:
```bash
docker stop kali-rdp
```
Exec into container:
```bash
docker exec -it kali-rdp /bin/bash
```
## 🐳 Optional: Save Custom Image
After setup, commit your container to reuse:

```bash
docker commit kali-rdp my-kali-xrdp
```
Then run any time with:

```bash
docker run -it -p 3389:3389 my-kali-xrdp
```
🧠 Notes
- Use Ctrl + P then Ctrl + Q to detach from the container without stopping it.

- You can run multiple containers with different port bindings (-p 3390:3389, etc.)

- For full GUI, XRDP works better than X11 forwarding for Docker containers.

## ✅ Summary of Installed Tools

<table>
  <thead>
    <tr>
      <th>Category</th>
      <th>Tools</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Network Utilities</td>
      <td><code>net-tools</code>, <code>iproute2</code>, <code>ping</code>, <code>dnsutils</code>, <code>curl</code>, <code>wget</code></td>
    </tr>
    <tr>
      <td>Scanners</td>
      <td><code>nmap</code>, <code>whois</code>, <code>traceroute</code></td>
    </tr>
    <tr>
      <td>Pentesting</td>
      <td><code>netcat</code>, <code>hydra</code>, <code>sqlmap</code>, <code>aircrack-ng</code>, <code>tcpdump</code>, <code>wireshark</code></td>
    </tr>
    <tr>
      <td>GUI + RDP</td>
      <td><code>xfce4</code>, <code>xrdp</code>, <code>dbus-x11</code></td>
    </tr>
  </tbody>
</table>

---

## 🔗 Credits

- **Kali Linux Docker Image**: [`kalilinux/kali-rolling`](https://hub.docker.com/r/kalilinux/kali-rolling) on Docker Hub  
- **Maintained by**: *Ankit Kushwaha*


```yaml
---

### ✅ Want to go further?

Let me know if you'd like:

- A `Dockerfile` to automate all the above
- Add SSH or VNC access
- Custom security tools added (Metasploit, Burp Suite, etc.)
- Full GUI-based ISO to Docker conversion
```

Just say the word!
