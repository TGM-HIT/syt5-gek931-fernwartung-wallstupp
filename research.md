# SYT5 GEK931 Taschenmesser 
**Autor:** Melissa Wallpach, Markus Stuppnig


SSH-Tunneling, Fernwartung und VPN-Verbindungen

## Fragenstellung
### 1. **Vorteile und Nachteile der Verwendung von Passwörtern im Key-Authentication-Verfahren:**

**Vorteile:**
  - **Einfachheit**: Die Verwendung von Passwörtern ist eine der einfachsten Formen der Authentifizierung, da Benutzer keine speziellen Schlüssel generieren oder speichern müssen.
  - **Kein externer Schlüssel nötig**: Benutzer benötigen keinen zusätzlichen privaten Schlüssel, sondern verwenden nur das Passwort, das sie sich merken müssen.

**Nachteile:**
  - **Sicherheit**: Passwörter sind im Vergleich zu SSH-Keys weniger sicher, da sie leichter durch Brute-Force-Angriffe kompromittiert werden können. Außerdem können schwache oder wiederverwendete Passwörter das Risiko erhöhen.
  - **Keine Automatisierung**: Passwörter erschweren die Automatisierung, da bei jeder SSH-Verbindung das Passwort manuell eingegeben werden muss.
  - **Phishing-Risiko**: Passwörter können durch Phishing-Angriffe abgefangen werden.

### 2. **Verwendung verschiedener Keys bei verschiedenen SSH-Verbindungen:**

Um verschiedene SSH-Keys bei verschiedenen Verbindungen zu verwenden, kann die Datei `~/.ssh/config` genutzt werden. Diese Konfigurationsdatei ermöglicht es, Host-spezifische Einstellungen festzulegen, wie z.B. den Pfad zu einem bestimmten Schlüssel. Ein Beispiel für den Einsatz mehrerer Schlüssel:

```bash
Host server1.example.com
    HostName server1.example.com
    User yourusername
    IdentityFile ~/.ssh/id_rsa_server1

Host server2.example.com
    HostName server2.example.com
    User yourusername
    IdentityFile ~/.ssh/id_rsa_server2
```

Jede SSH-Verbindung nutzt dann automatisch den zugewiesenen Schlüssel für den jeweiligen Host.

### 3. **Was ist VNC und wie kommt es zum Einsatz?**

**Virtual Network Computing (VNC)** ist ein Remote-Desktop-Protokoll, das es ermöglicht, die grafische Benutzeroberfläche eines entfernten Computers zu steuern. VNC überträgt die Inhalte des Bildschirms von einem Host auf einen Client über das Netzwerk und ermöglicht Benutzern die Steuerung der Maus und Tastatur, als wären sie physisch am Host-Computer.

- **Einsatz von VNC**:
  - Fernadministration von Computern oder Servern, die keine direkte physische Eingabe benötigen.
  - Zugriff auf Desktops von entfernten Benutzern für Support oder administrative Aufgaben.
  - VNC kann auch über SSH getunnelt werden, um die Verbindung sicher zu verschlüsseln.

### 4. **Wie kann man mit einem Browser eine Desktop-Umgebung einsetzen?**

Um eine Desktop-Umgebung über einen Browser zu nutzen, kommen Technologien wie **Apache Guacamole** zum Einsatz. Apache Guacamole ist ein Client-loses Remote-Desktop-Gateway, das den Zugriff auf Desktop-Umgebungen (VNC, RDP oder SSH) über eine Web-Oberfläche ermöglicht. Der Benutzer öffnet einfach eine HTTPS-Verbindung zu dem Server, auf dem Guacamole läuft, und kann von dort aus auf die Desktop-Umgebung zugreifen.

Schritte:
1. Der Guacamole-Server wird auf einem Remote-Computer installiert.
2. Der Benutzer greift mit einem Webbrowser auf die Guacamole-Oberfläche zu.
3. Guacamole leitet die Verbindung zu einem VNC-, RDP- oder SSH-Server weiter, der die Desktop-Umgebung bereitstellt.

### 5. **Welche Kryptographie-Protokolle kommen bei Wireguard zum Einsatz?**

Wireguard verwendet modernste Kryptographie-Protokolle und ist darauf ausgelegt, schnell und sicher zu sein. Zu den verwendeten Kryptographie-Protokollen gehören:

- **ChaCha20**: Symmetrisches Verschlüsselungsprotokoll für die Datenübertragung. Es ist besonders effizient auf Geräten mit geringer Rechenleistung und sorgt für hohe Geschwindigkeit bei gleichzeitig hoher Sicherheit.
- **Poly1305**: Ein MAC (Message Authentication Code), der in Kombination mit ChaCha20 verwendet wird, um die Authentizität der übertragenen Daten sicherzustellen.
- **Curve25519**: Ein asymmetrisches Schlüsselaustauschverfahren, das für den sicheren Austausch von Verschlüsselungsschlüsseln zwischen den Parteien verantwortlich ist.
- **BLAKE2s**: Ein schneller und sicherer Hash-Algorithmus, der für Hashing-Anforderungen in Wireguard verwendet wird.
- **HKDF**: Ein Schlüsselaustauschmechanismus, der für das Ableiten von Schlüsseln aus einem vorherigen Schlüsselaustausch verwendet wird.

## Software und Technologien

- **VMware Fusion**: Zum Aufsetzen der virtuellen Maschinen (VMs).
- **Ubuntu Server**: Als Betriebssystem für die VMs.
- **OpenSSH**: Für die SSH-Kommunikation.
- **VNC**: Für grafischen Fernzugriff.
- **Apache Guacamole**: Für die Bereitstellung des Desktops über das Web.
- **AnyDesk/TeamViewer**: Für Fernwartung.
- **Wireguard**: Für die Einrichtung eines VPNs in einem Docker-Container.
- **No-IP**: Für dynamisches DNS.

## Anwendung der Technologien im Detail

### Virtualisierung:
- **VMware**: Wir nutzen VMware, um virtuelle Maschinen (VMs) für die Server- und Client-Konfiguration einzurichten. Dabei simulieren wir ein Netzwerk mit SSH-Servern und Tunneln.

### SSH (Secure Shell):
- **OpenSSH**: SSH wird verwendet, um sichere Verbindungen zwischen den virtuellen Maschinen herzustellen. Dabei wird ein SSH-Tunnel eingerichtet, um den Zugriff auf entfernte Server zu ermöglichen.

### VPN:
- **Wireguard**: Ein modernes, sicheres und performantes VPN, das auf einfacher Konfiguration und starker Verschlüsselung basiert.

### Remote Desktop:
- **VNC** (Virtual Network Computing): VNC wird verwendet, um den Desktop von entfernten Computern oder Servern anzuzeigen und zu steuern.
- **Apache Guacamole**: Ein client-loses Remote-Desktop-Gateway, das über einen Webbrowser den Zugriff auf VNC, RDP und SSH-Verbindungen ermöglicht.
- **TeamViewer & AnyDesk**: Diese Tools werden getestet, um die Effizienz und Sicherheit von Remote-Desktop-Anwendungen zu vergleichen.

### Dynamic DNS:
- **No-IP**: wird verwendet, um dynamische IP-Adressen eines Heimnetzwerks mit einem DNS-Namen zu verknüpfen, sodass entfernte Verbindungen trotz wechselnder IP-Adressen möglich sind.

## Glossar

erkläre:

* Key-Authentication-Verfahren
### sshd_config
`sshd_config` ist die Konfigurationsdatei für den OpenSSH-Server-Dienst (`sshd`). Diese Datei enthält eine Liste von Anweisungen (Schlüsselwort-Wert-Paaren), die die Arbeitsweise des SSH-Servers steuern. Hier werden alle sicherheitsrelevanten Einstellungen, Netzwerkparameter und Zugriffsrechte festgelegt.

Auszug aus der Manpage:

```
SSHD_CONFIG(5)

NAME
     sshd_config — OpenSSH SSH daemon configuration file

DESCRIPTION
     `sshd_config` is the configuration file for `sshd(8)`. The file contains keyword-argument pairs, one per line. For full details, see the `sshd` manual page. This file must be readable only by the owner and root.

     The possible keywords and their meanings are as follows (note that keywords are case-insensitive and arguments are case-sensitive):

     AcceptEnv
             Specifies what environment variables sent by the client will be copied into the session's `environ(7)`. See `AcceptEnv` in `sshd_config(5)`.

     AddressFamily
             Specifies which address family should be used by sshd. Valid arguments are: `any`, `inet` (IPv4 only), or `inet6` (IPv6 only).

     AllowAgentForwarding
             Specifies whether `ssh-agent(1)` forwarding is permitted. The default is `yes`.

     AllowGroups
             This keyword can be followed by a list of group names, separated by spaces. If specified, login is allowed only for users whose primary group or supplementary group list matches one of the patterns.

     AllowTcpForwarding
             Specifies whether TCP forwarding is permitted. The available options are `yes` (the default) or `no`.

uvm...
```

**Wichtige Parameter in `sshd_config`:**

1. **Port**: Gibt den Port an, auf dem der SSH-Daemon auf Verbindungen wartet (Standard ist 22).
2. **PermitRootLogin**: Bestimmt, ob sich der Benutzer `root` per SSH einloggen darf. Empfohlen ist, dies zu deaktivieren oder nur mit `prohibit-password` zuzulassen, um die Sicherheit zu erhöhen.
3. **PasswordAuthentication**: Steuert, ob Passwort-Authentifizierung erlaubt ist. Bei sichereren Umgebungen empfiehlt es sich, dies zu deaktivieren und ausschließlich Public-Key-Authentifizierung zu verwenden.
4. **PubkeyAuthentication**: Erlaubt oder verbietet die Authentifizierung über öffentliche Schlüssel (Standard ist `yes`).
5. **AllowUsers / DenyUsers**: Erlaubt oder verbietet den Zugriff auf bestimmte Benutzerkonten.
6. **ClientAliveInterval und ClientAliveCountMax**: Diese Einstellungen steuern das Timeout-Verhalten für inaktive SSH-Verbindungen, um die Stabilität und Sicherheit zu gewährleisten.
7. **AllowTcpForwarding**: Erlaubt oder verbietet das Weiterleiten von TCP-Verbindungen über SSH (z.B. für Tunneling).
8. **AuthorizedKeysFile**: Gibt den Pfad zur Datei an, die die öffentlichen Schlüssel enthält, die für die Authentifizierung eines Benutzers verwendet werden (Standard ist `.ssh/authorized_keys`).

Die Konfiguration wird in der Regel nach jeder Änderung mit dem Befehl `sudo systemctl restart sshd` neu geladen, damit die Änderungen wirksam werden.

### Key-Authentication-Verfahren

Das Key-Authentication-Verfahren (Schlüsselbasierte Authentifizierung) ist eine der sichersten Methoden, um sich über SSH zu authentifizieren. Anstatt ein Passwort zu verwenden, identifiziert sich der Benutzer durch ein asymmetrisches Schlüsselpaar, bestehend aus einem **privaten Schlüssel** und einem **öffentlichen Schlüssel**.

#### Wie funktioniert das?

1. **Schlüsselpaar erzeugen**: Der Benutzer erstellt ein Schlüsselpaar (private und öffentliche Schlüssel) mit einem Befehl wie `ssh-keygen`. Dies erzeugt zwei Dateien:
   - **Privater Schlüssel**: Dieser bleibt auf dem Client-System und sollte niemals an Dritte weitergegeben werden.
   - **Öffentlicher Schlüssel**: Dieser wird auf den SSH-Server kopiert und in die Datei `~/.ssh/authorized_keys` des Benutzers eingefügt.

2. **SSH-Authentifizierung**:
   - Wenn der Benutzer versucht, sich über SSH anzumelden, wird der Server den öffentlichen Schlüssel des Benutzers (den er in der `authorized_keys`-Datei hat) verwenden, um eine Herausforderung zu erstellen.
   - Der Client antwortet auf diese Herausforderung mit seinem privaten Schlüssel. Der Server verifiziert diese Antwort mit dem bereits vorhandenen öffentlichen Schlüssel.
   - Falls die Prüfung erfolgreich ist, wird die Authentifizierung abgeschlossen, ohne dass ein Passwort benötigt wird.

#### Vorteile der Schlüsselbasierten Authentifizierung:
- **Sicherer als Passwort-Authentifizierung**: Da der private Schlüssel nur auf dem Client gespeichert ist und durch ein starkes Passwort geschützt werden kann, ist es viel schwieriger, diesen zu kompromittieren als ein einfaches Passwort.
- **Automatisierung**: Viele Prozesse, die SSH verwenden (z.B. automatisierte Skripte, Tunneling), erfordern keine manuelle Eingabe eines Passworts, wenn Schlüssel-basierte Authentifizierung verwendet wird.
- **Erweiterte Sicherheit durch Passphrase**: Der private Schlüssel kann durch eine Passphrase zusätzlich abgesichert werden.

#### Automatisierung mit Public-Key-Verfahren:
In vielen Szenarien (wie der oben genannten SSH-Tunneling-Aufgabe) wird das Public-Key-Verfahren für die Automatisierung genutzt. Dadurch kann eine SSH-Verbindung ohne Benutzereingriff und ohne die Gefahr von Passwortdiebstahl aufgebaut werden. Dies ist ideal für Szenarien, in denen Server automatisch untereinander kommunizieren müssen.

### Vorteile von **WireGuard**:

1. **Sicherheit**
   - **Starke Verschlüsselung**: Nutzt moderne Algorithmen wie ChaCha20 und Poly1305, die robust gegen Angriffe sind.
   - **Minimaler Code**: Mit weniger als 4.000 Zeilen ist WireGuard leichter zu überprüfen und sicherer.

2. **Einfache Konfiguration**
   - **Benutzerfreundlichkeit**: Intuitive Konfiguration mit nur wenigen Schritten (öffentliche/private Schlüssel).
   - **Einfache Integration**: Lässt sich leicht in bestehende Systeme einfügen.

3. **Hohe Performance**
   - **Geringer Overhead**: Effiziente Nutzung von Ressourcen, ideal für Geräte mit begrenzter Leistung.
   - **Schnelle Verbindungszeiten**: Rascher Verbindungsaufbau und Wiederherstellung bei Bedarf.

4. **Stabilität und Zuverlässigkeit**
   - **Moderne Architektur**: Unterstützt dynamische IPs und Mobilität, besonders nützlich für mobile Geräte.
   - **Persistente Verbindungen**: Verbindungen bleiben stabil, auch bei Netzwerkwechsel.

5. **Cross-Plattform Unterstützung**
   - **Verfügbarkeit**: Auf vielen Betriebssystemen (Linux, Windows, macOS, Android, iOS) nutzbar.

6. **Einfache Wartung**
   - **Weniger Komplexität**: Leichtere Wartung und schnellere Sicherheitsupdates.

### SSH Port-Forwarding

#### **ssh -L (Lokales Forwarding)**
- **Zweck:** Leitet einen lokalen Port zu einem Remote-Server-Port weiter.
- **Anwendungsfall:** Sie möchten auf einen Remote-Dienst (wie eine Datenbank oder einen Webserver) zugreifen, der von Ihrem lokalen Rechner aus nicht direkt erreichbar ist.
- **Syntax:**
  ```bash
  ssh -L [lokaler_port]:[remote_host]:[remote_port] [user]@[ssh_server]
  ```
- **Beispiel:**
  ```bash
  ssh -L 8080:localhost:80 user@example.com
  ```
  Dieses Kommando leitet den lokalen Port 8080 zum Port 80 auf localhost (dem Remote-Server) weiter. Sie können darauf über `http://localhost:8080` zugreifen.

---

#### **ssh -R (Remote Forwarding)**
- **Zweck:** Leitet einen Remote-Port zu einem lokalen Server-Port weiter.
- **Anwendungsfall:** Sie möchten einen Dienst, der auf Ihrem lokalen Rechner läuft, für den Remote-Server oder das Internet zugänglich machen.
- **Syntax:**
  ```bash
  ssh -R [remote_port]:[local_host]:[local_port] [user]@[ssh_server]
  ```
- **Beispiel:**
  ```bash
  ssh -R 9090:localhost:3000 user@example.com
  ```
  Dieses Kommando leitet den Remote-Port 9090 auf dem SSH-Server zu Port 3000 auf Ihrem lokalen Rechner weiter. Sie können darauf über `http://example.com:9090` zugreifen.

---

### **Zusammenfassung**
- **-L:** Leitet einen lokalen Port zum Remote-Host weiter (Ihr Rechner greift auf einen Dienst auf dem Remote-Server zu).
- **-R:** Leitet einen Remote-Port zum lokalen Host weiter (der Remote-Server greift auf einen Dienst auf Ihrem lokalen Rechner zu).