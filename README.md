# SYT5 GEK931 Fernwartung
**Author:** Markus Stuppnig 5DHIT, Melissa Wallpach 5BHIT

## Projektübersicht - SSH-Tunneling, Fernwartung und VPN-Verbindungen 
Dieses Projekt enthält grundlegende Technologien zur Fernwartung, wie **SSH-Tunneling**, **Remote Desktop** und **VPN-Verbindungen**. Dabei werden verschiedene Techniken gegenübergestellt und deren Einsatzmöglichkeiten überprüft. 
In diesem Projekt geht es um die Einrichtung einer sicheren Kommunikationsverbindung zwischen einem Host, einem Client und einem Server mithilfe von SSH-Tunneling. 


## Ziele
- Einrichtung von SSH-Tunneling, um die Kommunikation zwischen Host und Server über einen Client zu ermöglichen.
- Installation und Konfiguration von Remote-Desktop-Tools (VNC, Apache Guacamole) zur grafischen Fernsteuerung.
- Einrichtung eines Wireguard-VPNs für sichere Verbindungen zwischen den Systemen.
- Implementierung von Dynamic DNS mit No-IP für den Zugriff auf Geräte mit dynamischen IP-Adressen.


## Aufgabenstellung
Die einzelnen Aufgaben umfassen:
- Einrichtung von **SSH**-Verbindungen und **Tunneling** für eine sichere Fernwartung.
- Konfiguration von Webtop, um auf grafische Benutzeroberflächen zuzugreifen.
- Implementierung eines **Wireguard-VPNs** zur sicheren Verbindung zwischen Netzwerken.
- Nutzung von **Apache Guacamole**, um Desktop-Umgebungen über den Browser zugänglich zu machen.
- Vergleich und Test von **Remote Desktop Tools** wie TeamViewer und AnyDesk.
- Implementierung von **Dynamic DNS** für den Zugriff auf Heimnetzwerke mit dynamischen IP-Adressen.

## Software und Technologien

- **VMware Fusion**: Zum Aufsetzen der virtuellen Maschinen (VMs).
- **Ubuntu Server**: Als Betriebssystem für die VMs.
- **OpenSSH**: Für die SSH-Kommunikation.
- **Webtop**: Für grafischen Fernzugriff.
- **AnyDesk/TeamViewer**: Für Fernwartung.
- **Wireguard**: Für die Einrichtung eines VPNs in einem Docker-Container.
- **No-IP**: Für dynamisches DNS.


## Aufgabenübersicht

### 1. SSH-Tunneling:
- **Ziel**: Sichere Verbindung zwischen zwei Systemen über SSH herstellen und Dienste über die SSH-Verbindung tunneln.
- **Vorgehen**: 
  1. Einrichtung von zwei VMs als Server und Client mit einem laufenden SSH-Server.
  2. Erstellen eines SSH-Tunnels (tunneling über den Clieunt), um den SSH-Server des Servers vom Host aus zu erreichen.
  3. Automatisierte Verbindung mithilfe von Systemd-Services.

### 2. Fernwartung mit Remote Desktop:
- **Ziel**: Zugriff auf eine entfernte Desktop-Umgebung mit Webtop
- **Vorgehen**:
  1. Docker Image pullen
  2. leeren “config” folder im Downloads directory erstellen, damit dort files abgespeichert werden könnnen
  3. Docker-compose.yaml erstellen
  4. docker-compose up und im browser http://localhost:3000 aufrufen

### 3. VPN mit Wireguard:
- **Ziel**: Einrichten eines schnellen und sicheren VPNs zwischen den VMs.
- **Vorgehen**:
  1. Konfiguration einer Docker-Instanz für den Wireguard-VPN-Server.
  2. Verbindung des Clients zum VPN mittels QR-Code oder Konfigurationsdatei.

### 4. Dynamic DNS:
- **Ziel**: Erstellen einer dynamischen DNS-Lösung für ein Heimnetzwerk, um trotz wechselnder IP-Adressen auf das Netzwerk zuzugreifen.
- **Vorgehen**:
  1. Installation des No-IP-Clients auf einem Raspberry Pi.
  2. Konfiguration einer Portweiterleitung am Router, um den Wireguard-Server zugänglich zu machen.


## Installation und Konfiguration

### SSH-Tunneling
1. Auf beiden VMs (Server und Client) **OpenSSH** installieren und konfigurieren:
   ```bash
   sudo apt-get install openssh-server
   ```
2. Schlüsselgenerierung für die automatisierte Authentifizierung:
   ```bash
   ssh-keygen -t rsa
   ```
3. Public-Key auf den Server kopieren:
   ```bash
   ssh-copy-id user@server_ip
   ```
4. SSH-Tunnel erstellen:
   ```bash
   ssh -L local_port:localhost:remote_port user@server_ip
   ```

### Wireguard VPN
1. **Docker** installieren und den Wireguard-Container konfigurieren.
2. Wireguard-Client mit der Konfigurationsdatei oder dem QR-Code verbinden.

### Remote Desktop mit VNC
1. **VNC** auf dem Server installieren und konfigurieren:
   ```bash
   sudo apt-get install tightvncserver
   ```
2. Optional: **Apache Guacamole** für den Browserzugriff auf die Desktopumgebung einrichten.

### Dynamic DNS
1. **No-IP** Client installieren:
   ```bash
   sudo apt-get install noip2
   ```
2. Systemd-Service für No-IP einrichten, um den Client automatisch zu starten.

