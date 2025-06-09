Instalacja Zabbix na Debianie 

1.	Dodanie oficjalnego repozytorium Zabbix

Przykład dla Zabbix 6.0 LTS:

wget https://repo.zabbix.com/zabbix/6.0/debian/pool/main/z/zabbix-release/zabbix-release_6.0-5+debian12_all.deb

sudo dpkg -i zabbix-release_6.0-5+debian12_all.deb

sudo apt update


________________________________________

2.	Zainstalowanie pakietów Zabbix

sudo apt install zabbix-server-mysql zabbix-frontend-php zabbix-apache-conf zabbix-sql-scripts zabbix-agent

To zainstaluje:

•	Zabbix Server

•	Zabbix Agent (opcjonalny, lokalny)

•	Frontend Zabbix (PHP + Apache)

•	Skrypty do utworzenia bazy danych

________________________________________

3.	Instalacja i konfiguracja MariaDB / MySQL

sudo apt install mariadb-server mariadb-client

Zaloguj się do MySQL:

sudo mysql

Utwórz bazę danych i użytkownika:

CREATE DATABASE zabbix CHARACTER SET utf8mb4 COLLATE utf8mb4_bin;

CREATE USER zabbix@localhost IDENTIFIED BY 'wpisać_tu_hasło';

GRANT ALL PRIVILEGES ON zabbix.* TO zabbix@localhost;

FLUSH PRIVILEGES;

EXIT;

________________________________________

4.	Import bazy danych

zcat /usr/share/zabbix-sql-scripts/mysql/server.sql.gz | mysql -uzabbix -p zabbix

Wprowadź wcześniej ustawione hasło dla zabbix.

________________________________________

5.	Konfiguracja serwera Zabbix

Edytuj plik:

sudo nano /etc/zabbix/zabbix_server.conf

Ustaw:

DBPassword=wpisać_tu_hasło

________________________________________

6.	Uruchomienie usługi

sudo systemctl restart zabbix-server zabbix-agent apache2

sudo systemctl enable zabbix-server zabbix-agent apache2

________________________________________

7.	Webowy instalator Zabbix (frontend)

Otwórz przeglądarkę i przejdź do:

http://<adres_IP_komputera>/zabbix

Przykład: http://192.168.1.100/zabbix

Przejdź przez instalator Zabbixa:

1.	Weryfikacja wymagań PHP

2.	Wprowadź dane bazy:

o	DB host: localhost

o	DB name: zabbix

o	DB user: zabbix

o	DB password: (twoje hasło)

5.	Nazwij instancję Zabbix (dowolnie)
   
________________________________________

8.	Logowanie do panelu Zabbix
   
•	Login: Admin

•	Hasło: zabbix



Konfiguracja hosta (z agentem Zabbix)

1. Dodanie nowego hosta
   
2.	Przejdź do: Konfiguracja → Hosty
   
3.	Kliknij: Utwórz hosta
   
________________________________________

2.Wypełnienie podstawowych danych hosta

Zakładka Ogólne:

•	Nazwa hosta (Host name): np. webserver01

•	Grupy hostów (Groups): wybierz istniejącą (np. Linux servers) lub utwórz nową

•	Widoczna nazwa (Visible name) (opcjonalnie): np. Serwer WWW

•	Interfejsy (Interfaces):

o	Typ: Agent Zabbix

o	Adres: IP lub hostname hosta (np. 192.168.1.100)

o	Port: 10050 (domyślny)

o	Włącz: zaznaczone (checkbox „Włączone”)

________________________________________

3.Połączenie szablona monitorującego (template)

1.	Przejdź do zakładki Szablony (Templates)
   
2.	Kliknij Połącz szablony
   
3.	Wybierz np.:
   
o	Template OS Linux by Zabbix agent

4.	Zatwierdź przyciskiem Wybierz
________________________________________

4.Zapisanie konfiguracji

Kliknij Dodaj na dole formularza.

________________________________________

5.Weryfikacja działania

1.	Przejdź do: Monitorowanie → Hosty
   
2.	Sprawdź status:
   
o	Zielona ikona ✅ = agent działa i host jest monitorowany

o	Czerwona ikona ❌ = problem z połączeniem (sprawdź IP, zaporę, usługę agenta)



Przydatne polecenia



Agent Zabbix

•	systemctl status zabbix-agent - Pokazuje aktualny status usługi Zabbix Agent.

•	nano /etc/zabbix/zabbix_agentd.conf - Otwiera plik konfiguracyjny Zabbix Agent w edytorze nano.

•	tail -n 10 /var/log/zabbix/zabbix_agentd.log - Wyświetla 10 ostatnich linii logu agenta Zabbix.

•	sudo systemctl restart zabbix-agent - Restartuje usługę agenta Zabbix.



Server Zabbix

•	tail -n 10 /var/log/zabbix/zabbix_server.log - Pokazuje 10 ostatnich linii logu serwera Zabbix

•	systemctl restart zabbix-server - Restartuje serwer Zabbix (proces backendowy, który zbiera dane i zarządza nimi).

•	nano /etc/zabbix/zabbix_server.conf - Otwiera główny plik konfiguracyjny serwera Zabbix.

•	systemctl status zabbix-server - Pokazuje status serwera Zabbix.
