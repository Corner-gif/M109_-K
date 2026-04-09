# Arbeitsaufträge
## Auftrag 3.2: Webserver mit Docker
* Erstellen Sie eine einfache HTML-Webseite (z. B. index.html). Die Erstellung kann selbstständig oder mit Unterstützung einer KI erfolgen.
![help](Rescources/html.png)<br>
[index.html](Rescources/index.html)

* Nutzen Sie ein geeignetes Container-Image von Docker Hub, z. B. das Image nginx: https://hub.docker.com/_/nginx.
![help](Rescources/nginx.png)
* Starten Sie einen Container auf Ihrer lokalen Container Engine (z. B. Docker), der Ihre HTML-Webseite ausliefert.
![help](Rescources/start.png)


## Auftrag 3.3: Python-Webserver und Dateiverwaltung in Containern
1. Python-Webserver erstellen:<br>




## Aufgabe 4.5: Migration von Docker Compose zu Kubernetes
```
version: '3.8'

services:
  wordpress:
    image: wordpress:6.4-apache
    ports:
      - "8080:80"
    restart: always
    environment:
      WORDPRESS_DB_HOST: db:3306
      WORDPRESS_DB_USER: wordpress
      WORDPRESS_DB_PASSWORD: wordpress
      WORDPRESS_DB_NAME: wordpress
    depends_on:
      - db

  db:
    image: mariadb:10.6
    restart: always
    environment:
      MYSQL_DATABASE: wordpress
      MYSQL_USER: wordpress
      MYSQL_PASSWORD: wordpress
      MYSQL_ROOT_PASSWORD: rootpassword
    volumes:
      - db_data:/var/lib/mysql

volumes:
  db_data:
```
