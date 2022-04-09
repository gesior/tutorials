# 2. Instalacja nginx, MariaDB, PHP i podstawowych narzędzi

### 2.1 Instalacja PHP

Instalacja PHP (`php-fpm` dla `nginx`) i bibliotek PHP dla Gesior2012:
```
apt install php-fpm php-bcmath php-common php-curl php-json php-gd php-mbstring php-mysql php-pdo php-xml php-zip
```
Na systemach z PHP w wersji starszej niż 8.0 trzeba jeszcze zainstalować `php-json`:
```
apt install php-json
```
Od wersji 8.0 `json` jest wbudowany w PHP i nie trzeba go instalować oddzielnie.

### 2.2 Instalacja nginx

Instalacja nginx:
```
apt install nginx
```

### 2.3 Instalacja MariaDB

MariaDB to zastępstwo dla MySQL. W wielu miejscach w Linux konfiguracje MariaDB mają w nazwie `mysql`.

Instalacja MariaDB:
```
apt install mariadb-server mariadb-client
```

### 2.4 Podstawowe narzędzia

Warto zainstalować kilka podstawowych narzędzi.
Przydają się przy diagnozowaniu problemów z serwerem i podczas ataków.
```
apt install fail2ban iptables-persistent htop nload tcpdump mc zip unzip git screen screenie net-tools moreutils traceroute
```
Podczas instalacji mogą się pojawić 2 pytania odnośnie zapisu `iptables`. Na oba odpowiadamy 'Nie'.

Co instalujemy:
- `fail2ban` - zabezpiecza SSH, blokuje IP po kilku próbach logowania ze złym hasłem
- `iptables-persistent` - pozwala zapisać reguły `iptables` do pliku i ładuje je po restarcie serwera
- `htop` - rozbudowana wersja `top`
- `nload` - pokazuje aktualne użycie łącza na serwerze
- `tcpdump` - zapisuje/wyświetla wszystkie przychodzące/wychodzące pakiety sieciowe
- `zip` + `unzip` - programy do pakowania/wypakowywania plików `.zip`
- `git` - klient GIT
- `mc` - graficzny manager plików działający w konsoli
- `screen` + `screenie` - aplikacja do zarządzania działającymi `screen`
- `net-tools` - zawiera `netstat` pozwalający sprawdzić na jakiś IP i portach nasłuchuje serwer 
- `moreutils` - zawiera `ts`, aplikację pozwalająca dodawać czas w milisekundach do logów (np. z konsoli OTSa)
- `traceroute` - pozwala śledzić trasę pakietów sieciowych (diagnostyka problemów z łączem) 
