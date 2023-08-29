
<h1 align="center">
  <br>
  <img src=".github/assets/logo-1.png" alt="Logo" width="200"></a>
  <br>
  Traefik + Vaultwarden + MariaDB
  <br>
</h1>

<h4 align="center">Ein Docker-Compose-Setup zum Einrichten eines Passwort-Managers mit eigener Domain.</h4>

<p align="center">
  <a href="#anleitung-zur-verwendung">Anleitung zur Verwendung</a> •
  <a href="#services">Services</a> •
  <a href="#traefik">Traefik</a> •
  <a href="#vaultwarden">Vaultwarden</a> •
  <a href="#mariadb">MariaDB</a> •
  <a href="#netzwerke-und-volumes">Netzwerke und Volumes</a> •
  <a href="#verwendete-technologien">Verwendete Technologien</a> •
  <a href="#autor">Autor</a>
</p>

# Schulprojekt: Docker-Projekt

Das folgende Docker-Compose-File wird für mein Schulprojekt verwendet, um eine Containerumgebung mit Traefik, Vaultwarden und MariaDB aufzusetzen. Es ermöglicht die Bereitstellung eines sicheren Passwortverwaltungsdienstes mit einer webbasierten Benutzeroberfläche.

## Anleitung zur Verwendung

Um dieses Projekt unter Windows mit WSL und Docker Desktop einzurichten und auszuführen, befolgen Sie die folgenden Schritte:

1. Stellen Sie sicher, dass Sie Git und Docker Desktop auf Ihrem Computer installiert haben. Sie können Git von der offiziellen Website herunterladen: [https://git-scm.com](https://git-scm.com) und Docker Desktop von der offiziellen Website: [https://www.docker.com/products/docker-desktop](https://www.docker.com/products/docker-desktop).

2. Öffnen Sie das Windows Subsystem für Linux (WSL) und navigieren Sie zum gewünschten Verzeichnis, in dem Sie das Projekt speichern möchten.

3. Klonen Sie das Repository, indem Sie den folgenden Befehl ausführen:

```bash
$ git clone https://bbbhub.bbbaden.ch/J.MoralesPerez.inf21/m169_lb_moralesperezjairo.git
```

4. Wechseln Sie in das Verzeichnis des Repositorys:

```bash
$ cd m169_lb_moralesperezjairo
```

5. Führen Sie den folgenden Befehl aus, um die Docker-Container zu starten:

```bash
$ docker-compose up -d
```
Dieser Befehl startet die Container im Hintergrund (-d für den Detached-Modus)

6. Sobald die Container gestartet sind, können Sie auf die Anwendung zugreifen. Öffnen Sie einen Webbrowser und navigieren Sie zu den folgenden URLs:

    Traefik-Dashboard: http://traefik.jmp.localhost:8080
    Vaultwarden: http://vaultwarden.jmp.localhost:8181

7. Um die Container zu stoppen, kehren Sie zur WSL-Konsole zurück und führen Sie den folgenden Befehl aus:

```bash
$ docker-compose down
```

Dadurch werden die Container gestoppt und entfernt.

> Note: Im `.env` sowie auch im `docker-compose.yml` können alle Werte bearbeitet werden um einen andere Resultat zu erhalten. Z.b kann man die Domain im `docker-compose.yml` ändern.

## Services

### Traefik

- **Image**: traefik:v2.10
- **Container-Name**: traefik
- **Ports**: 80:80, 8080:8080
- **Volumes**: /var/run/docker.sock:/var/run/docker.sock:ro
- **Netzwerke**: traefik_default

Der Traefik-Service fungiert als Reverse Proxy und ermöglicht den Zugriff auf andere Container über definierte Routen. Er ist über den Hostnamen `traefik.jmp.localhost` unter Port 80 erreichbar. Das Traefik-Dashboard ist unter `http://traefik.jmp.localhost:8080` verfügbar.

### Vaultwarden

- **Image**: vaultwarden/server:1.28.1
- **Container-Name**: vaultwarden
- **Ports**: 8181:80
- **Depends-On**: mariadb
- **Volumes**: ./vw-data:/data
- **Netzwerke**: traefik_default

Vaultwarden ist der eigentliche Passwortverwaltungsdienst. Er ist über den Hostnamen `vaultwarden.jmp.localhost` unter Port 8181 erreichbar. Die Umgebungsvariablen werden aus der `.env`-Datei geladen, die Konfiguration umfasst die Datenbankverbindung zu MariaDB.

### MariaDB

- **Image**: mariadb:10.11
- **Container-Name**: mariadb
- **Ports**: 3306:3306
- **Volumes**: ./mariadb-data:/var/lib/mysql, /etc/localtime:/etc/localtime:ro
- **Environment**: MYSQL_ROOT_PASSWORD, MYSQL_PASSWORD, MYSQL_DATABASE, MYSQL_USER
- **Netzwerke**: traefik_default

MariaDB dient als Datenbank für die Passwortdaten. Es werden Umgebungsvariablen aus der `.env`-Datei verwendet, um das Root-Passwort, das Datenbankpasswort und andere Konfigurationsdetails festzulegen.

## Netzwerke und Volumes

Das Docker-Compose-File verwendet das externe Netzwerk `traefik_default`, das für die Kommunikation zwischen den Containern genutzt wird.

Zusätzlich werden die Volumes `vw-data` und `mariadb-data` verwendet, um persistente Daten für den Vaultwarden-Service und die MariaDB-Datenbank zu speichern.

Dieses Docker-Compose-File ermöglicht es uns, unsere Containerumgebung schnell und einfach aufzusetzen und unseren Passwortverwaltungsdienst bereitzustellen.

## Verwendete Technologien

Dieser Abschnitt zeigt alle Technologien die ich für dieses Projekt verwendet habe:

- [Docker](https://www.docker.com/)
- [Docker-Compose](https://docs.docker.com/compose/)
- [Traefik](https://traefik.io/traefik/)
- [Vaultwarden](https://github.com/dani-garcia/vaultwarden)
- [MariaDB](https://mariadb.com/)
- [WSL2 - Ubuntu](https://learn.microsoft.com/de-de/windows/wsl/install)

## Autor

👤 **Jairo Morales**

Projekt ist auch auf dem Schul-GitLab erhältlich.
