# Auctionhouse-Vagrant
Über das Vagrantfile werden ein load balancer, beliebig viele Webserver (über das Vagrantfile konfigurierbar) und eine Datenbank erstellt.

Der Webserver wird mit einem NodeJS Server (https://github.com/NoRyyZ/Auktionshaus) konfiguriert.
Der Load balancer wird mit Haproxy realisiert.
Als Datenbank wird das PostgreSQL DBMS verwendet.

Der Haproxy erkennt bei einem vagrant provision automatisch, wie viele Webserver sich im Netz befinden und generiert die jeweiligen IP-Addressen in die lokale Haproxy konfiguration.
Die Standardkonfiguration des Haproxy wird über die Haproxy.cfg im "resources" Ordner verändert. Als Debug hilfe, gibt die Load Balancer maschine, die generierte Haproxy Konfiguration in den Ordner "resources" in der Datei "test" aus.
