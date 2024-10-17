# Protokoll Umsetzung GEK931

### 1. Einrichten der VMs

- Zwei VMs in VMware Fusion erstellen ("Client" und "Server").
- Linux-Betriebssystem (Ubuntu Server) auf beiden VMs installieren 
- Netzwerkeinstellungen so konfigurierieren (NAT - eigenes Netzwerk Client-Server), dass beide VMs miteinander kommunizieren können

### 2. Installation von OpenSSH
OpenSSH ist eine Standardoption, die bei der Erstellung der VM ausgewählt werden kann und muss nicht zusätzlich installiert werden.

### 3. SSH-Konfiguration am Server

1. Sicherheit des SSH-Servers zu erhöhen, Konfigurationsdatei `/etc/ssh/sshd_config` auf dem Server folgende Zeile hinzufügen. (-> nur bereits bestehende Verbindungen akzeptieren)
2. ListenAddress in Config-File /etc/ssh/sshd_config muss auf 127.0.0.1 geändert werden.

```bash
ListenAddress 127.0.0.1
AllowTcpForwarding yes
GatewayPorts yes
```

   **Erklärung**:
   - **AllowTcpForwarding**: Erlaubt TCP-Weiterleitung.
   - **GatewayPorts**: Ermöglicht den Zugriff auf die weitergeleiteten Ports.
   
3. Restart:
```
sudo systemctl restart ssh
```
### 4. Server SSH-Tunneling konfigurieren

1. Generiere ein Schlüsselpaar auf dem Server:
  ```bash
  ssh-keygen 
  ssh-copy-id user@client-ip
  ```

2. SSH-Tunnel einrichten mit File /etc/systemd/system/ssh_t.service:
```bash
[Unit]
Description=Reverse SSH Tunnel

[Service]
User=markus
ExecStart=ssh -N -R 192.168.74.190:2222:localhost:22 markus@192.168.74.190
Restart=always

[Install]
WantedBy=multi-user.target  
```
3. Danach restart:

```
sudo systemctl demon -reload
sudo systemctl start ssh_t
sudo systemctl enable ssh_t
```

### 5. Client sshd_config 
1. **Proxy-Konfiguration**: Auf dem Proxy-Server (192.168.56.7) muss ebenfalls `GatewayPorts clientspecified` in der Datei `/etc/ssh/sshd_config` hinzugefügt werden, um den Zugriff auf den Tunnel zu ermöglichen.

```bash
GatewayPorts clientspecified
```
Restart:
```
sudo systemctl restart ssh
```

2. Nach der Erstellung der Dienstdatei müssen die folgenden Befehle auf der **Client-VM** (192.168.56.7) ausgefürht werden:

* **Systemd-Konfiguration neu laden**:
   ```bash
   sudo systemctl daemon-reload
   ```
   *Dieser Befehl lädt die Systemd-Konfiguration neu, um die neue Dienstdatei zu erkennen.*

* **Dienst aktivieren**:
   ```bash
   sudo systemctl enable ssh-tunnel.service
   ```
   *Dieser Befehl sorgt dafür, dass der Dienst beim nächsten Systemstart automatisch ausgeführt wird.*

3. **Dienst manuell starten**:
   ```bash
   sudo systemctl start ssh-tunnel.service
   ```
   *Dieser Befehl startet den SSH-Tunnel-Dienst sofort.*

4. **Status des Dienstes überprüfen**:
   ```bash
   sudo systemctl status ssh-tunnel.service
   ```
   *Dieser Befehl zeigt den aktuellen Status des Dienstes an, einschließlich eventueller Fehler.*

### 6. Automatisierter Tunnel via systemd

1. Datei systemd-Service unter `/etc/systemd/system/ssh-tunnel.service`erstellen:
  ```ini
   [Unit]
   Description=SSH-Tunnel
   After=systemd-networkd-wait-online.service
   Requires=systemd-networkd-wait-online.service

   [Service]
   Type=simple
   ExecStart=/usr/bin/ssh -N -R 192.168.56.7:8888:192.168.56.6:22
   Restart=always
   RestartSec=5

   [Install]
   WantedBy=multi-user.target
   ```

   **Erklärung**:
   - **Description**: Beschreibt den Dienst.
   - **After**: zeigt dass der Dienst erst nach dem Netzwerkdienst starten soll.
   - **Requires**: prüft, dass der Netzwerkdienst verfügbar ist, bevor der Tunnel gestartet wird.
   - **Type**: Gibt an, dass der Dienst einfach ist und direkt läuft.
   - **ExecStart**: Hier wird der SSH-Befehl definiert, der den Tunnel aufbaut.
   - **Restart**: Stellt sicher, dass der Dienst bei einem Absturz neu gestartet wird.
   - **RestartSec**: Legt fest, wie lange der Dienst warten soll, bevor er neu gestartet wird.
   - **WantedBy**: Gibt an, dass der Dienst beim Systemstart aktiviert werden soll.

   2. Service aktivieren:
  ```bash
  sudo systemctl daemon-reload
  sudo systemctl start ssh-tunnel.service
  sudo systemctl enable ssh-tunnel.service
  ```

### 7. Fernwartung mit Webtop

1. Docker Image pullen

```
docker pull lscr.io/linuxserver/webtop:latest
```
2. leeren “config” folder im Downloads directory erstellen, damit dort files abgespeichert werden könnnen
3. Docker-compose.yaml erstellen

```yaml
services:
   webtop:
      image: ghcr.io/linuxserver/webtop:alpine-mate
      container_name: webtop
      environment:
         - PUID=1000
         - PGID=1000
         - TZ=Asia/India
      volumes:
         - /Users/markus/Downloads/config:/config
      ports:
         - 3000:3000
      shm_size: "1gb"
      restart: unless-stopped
```
4. docker-compose up und im browser http://localhost:3000 aufrufen

### 8. Remote-Desktop-Tools

* AnyDesk auf Iphone und  Mac installieren
* Fernwartungstests durchführen:
1. Der sich verbindende Nutzer gibt die ID des anderen Users (Mac) in das Feld „Remote-Adress“ ein. 
2. Wenn die Verbindungsanfrage gültig ist, wird das Accept-Fenster auf dem Remote-Gerät angezeigt. Durch die Annahme der Anfrage wird die Sitzung hergestellt. 
### 9. **Wireguard-VPN in Docker:**
   - Richte Wireguard in einem Docker-Container ein. Befolge die [Wireguard Docker-Installationsanleitung](https://www.wireguard.com/install/), um eine sichere VPN-Verbindung herzustellen.
   - Dokumentiere, wie ein Client über den QR-Code oder eine Konfigurationsdatei verbunden wird.

### 10. **Dynamic DNS mit No-IP:**
   - Installiere den No-IP-Client auf einem Raspberry Pi, um dynamische IP-Adressen zu aktualisieren:
     ```bash
     sudo apt-get install noip2
     sudo noip2 -C
     ```
   - Erstelle eine systemd-Service-Datei unter `/etc/systemd/system/noip2.service` und aktiviere sie wie in der Aufgabenstellung beschrieben.

### 11. **VPN@home mit Wireguard:**
   Richte eine Portweiterleitung auf deinem Heimrouter ein, um den Raspberry Pi mit Wireguard von außen erreichbar zu machen.

Mit diesen Schritten hast du SSH-Tunneling, Fernwartungs- und VPN-Technologien auf VMware und deinem MacBook eingerichtet und konfiguriert.

