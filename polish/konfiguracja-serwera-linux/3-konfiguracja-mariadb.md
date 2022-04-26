# 3. Konfiguracja MariaDB

### 3.1 Domyślna konfiguracja

Domyślna konfiguracja MariaDB zazwyczaj używa około 400 MB RAM.

Jest ustawione wiele limitów, które nie pozwalają na wykonanie obliczeń w RAM i
wiele zapytań do bazy kończy się utworzeniem 'tymczasowego pliku na dysku' -
co bardzo spowalnia działanie MariaDB.
Dodatkowo z domyślną konfiguracją dane są zazwyczaj ładowane z dysku.

Jeśli masz na serwerze trochę wolnego RAMu, to warto to zmienić.

### 3.2 Optymalizacja - edycja konfiguracji

Optymalizacja konfiguracji MariaDB jest bardzo złożona.
Poniżej przykładowa konfiguracja opracowana dla dużych OTSów.

__UWAGA!__
Jeśli chcesz optymalizować bazę danych już działającego OTSa,
to najpierw zrób kopię zapasową!

Konfiguracja MariaDB w Ubuntu 20.04 jest w pliku:
```
/etc/mysql/mariadb.conf.d/50-server.cnf
```
Przykład zmian w konfiguracji, po których MariaDB powinna używać 4-8 GB RAM.
Zalecana dla każdego serwera z 16 GB RAM i więcej.

#### 3.2.1 Zmiana podstawowych limitów

Znajdź:
```
#key_buffer_size        = 16M
#max_allowed_packet     = 16M
#thread_stack           = 192K
#thread_cache_size      = 8
```
Odkomentuj i zamień wartości na:
```
key_buffer_size        = 512M
max_allowed_packet     = 512M
thread_stack           = 1M
thread_cache_size      = 128
```

#### 3.2.2 Zmiana limitów otwartych połączeń i tabel

Ustawiamy wysokie wartości, których serwer nie powinien nigdy osiągnąć. 

Znajdź:
```
#max_connections        = 100
#table_cache            = 64
```
Odkomentuj i zamień wartości na:
```
max_connections        = 50000
table_cache            = 50000
```

#### 3.2.3 Zmiana limitów zapytań przetwarzanych w RAM

Dodaj:
```
max_heap_table_size     = 1G
tmp_table_size          = 1G
bulk_insert_buffer_size = 1G
```

#### 3.2.4 Zmiana limitów dla InnoDB

InnoDB to silnik MariaDB, który jest używany przez bazę danych OTSa.

Te zmiany mają największy wpływ na czas zapisu OTSa.

Dodaj:
```
innodb_file_per_table = 1
innodb_buffer_pool_size = 4G
innodb_buffer_pool_instances = 8
innodb_log_file_size = 512M
innodb_log_buffer_size = 256M
innodb_flush_log_at_trx_commit = 2
```

#### 3.2.5 SQL mode - problemy z acc. makerami

Acc. makery, np. Gesior2012 nie trzymają się standardu SQL.
Przez wiele lat nie było to problemem,
bo domyślne ustawienia pozwalały łamać niektóre zasady SQLa.
W nowym MySQL i MariaDB to się zmieniło.

Na szczęście można przestawić tryb działania, na taki jak był wcześniej,
dodając w konfiguracji linijkę:
```
sql_mode=''
```
#### 3.2.6 Restart MariaDB

Aby zmiany w konfiguracji zaczęły działać, należy zrestartować MariaDB:
```
systemctl restart mysql
```
(tak, piszemy `mysql`, a restartuje się MariaDB)

### 3.3 Dostęp do bazy

Domyślnie baza MariaDB jest dostępna z konsoli z użytkownika `root` bez hasła.
Musimy zmienić dostęp do użytkownika tak, żeby móc logować się hasłem.

W przykładzie ustawimy hasło do użytkownika `root` na `secretpass`.

#### 3.3.1 Logowanie hasłem

Odpal:
```
mysql
```
Odpali sie konsola serwera MariaDB.

Odpal po kolei:
1. Wybierz bazę zawierającą użytkowników:
```
use mysql;
```
2. Zmienia typ logowania z `unix_socket`
(logowanie na podstawie użytkownika z konsoli)
na logowanie hasłem:
```
UPDATE `user` SET `plugin` = 'mysql_native_password' WHERE `user` = 'root' AND `host` = 'localhost';
```
3. Ustaw hasło użytkownika `root` na `secretpass`:
```
SET PASSWORD FOR 'root'@'localhost' = PASSWORD('secretpass');
```
4. Przeładuj ustawienia użytkowników MariaDB:
```
flush privileges;
```
5. Wyłącz konsole MariaDB (można też kliknąć CTRL+D)
```
exit
```

#### 3.3.2 Automatycznie logowanie z konsoli

Po ustawieniu hasła, żeby zalogować się do MariaDB z konsoli, trzeba wpisać:
```
mysql -p
```
a następnie wpisać hasło.

Hasło można wpisać w konfiguracji MariaDB.
Po tej zmianie komendy `mysql` i `mysqldump` nie będą wymagały wpisywania hasła.

Edytuj plik:
```
/etc/mysql/mariadb.conf.d/50-client.cnf
```
Pod:
```
[client]
```
Dodaj linijkę:
```
password=secretpass
```
Po tej zmianie znowu można używać `mysql` (bez ` -p`) i nie podawać hasła.
