# 8. Konfiguracja limitu otwartych plików

W Linux otwarte połączenia (między graczem a serwerem) są liczone jako otwarte pliki.
Domyślny limit otwartych plików to 1024.
Wystarczy, że ktoś otworzy 1024 połączenia do serwera gry/serwera www i
już nikt inny nie będzie mógł dołączyć/otworzyć strony.

Ewentualnie Twój serwer stanie się super popularny i
nagle przy 1000 online zacznie wracać 'server offline', gdy ktoś spróbuje się zalogować.

Ilość otwartych plików przez daną aplikację jest limitowana przez 3 konfiguracje:
- `/etc/security/limits.conf` - ogranicza ilość otwartych plików przez danego użytkownika Linux
- `/etc/systemd` - ogranicza ilość otwartych plików przez daną aplikację
- jest też ogólny limit otwartych plików w systemie, ale na nowych Linuxach jest domyślnie wysoki 

Ja podnoszę limit do __300000__.
Szybciej RAM się skończy/CPU będzie obciążony na 100%, 
niż któraś z aplikacji osiągnie taką ilość połączeń.

## 8.1 Edycja limitu otwartych plików użytkownika

Edytujemy limit ilości otwartych plików 3 użytkowników:
- `root` - z niego odpalasz OTS
- `mysql` - na tym użytkowniku działa MariaDB
- `www-data` - na tym użytkowniku działa `nginx` i `php`

Jeśli odpalasz OTS z innego użytkownika niż `root`, to dla niego też ustaw wysokie limity.

Limity są ustawiane w pliku:
```
/etc/security/limits.conf
```

Na końcu pliku dodaj:
```
mysql           hard    nofile          300000
mysql           soft    nofile          150000

www-data        hard    nofile          300000
www-data        soft    nofile          150000

root            hard    nofile          300000
root            soft    nofile          150000
```
(`nofile` to skrót od `number of files`)

## 8.2 Edycja limitu otwartych plików przez daną aplikację

Każda aplikacja ma swoją konfigurację gdzieś w `/etc/systemd`.
Trzeba znaleźć pliki z konfiguracją danej aplikacji i edytować

### 8.2.1 Edytuj limity aplikacji nginx

Odpal, aby znaleźć pliki z konfiguracją `nginx`:
```
find /etc/systemd -name 'nginx*'
```
Otwórz każdy znaleziony plik.

Znajdź w nim linijkę zaczynającą się od:
```
LimitNOFILE=
```
Jeśli jest, to zamień na:
```
LimitNOFILE=300000
```
Jeśli nie ma, to znajdź:
```
[Service]
```
i linijkę niżej dodaj:
```
LimitNOFILE=300000
```

### 8.2.2 Zrób to samo dla PHP i MariaDB

Konfigurację PHP znajdziesz, odpalając:
```
find /etc/systemd -name 'php*'
```
Konfigurację MariaDB znajdziesz, odpalając:
```
find /etc/systemd -name 'mariadb*'
```
### 8.2.3 Zrestartuj serwer

Można kombinować i przeładować wszystkie zmienione uprawnienia przy pomocy komend,
ale dużo łatwiej to zrobić restartując Linuxa:
```
reboot
```
