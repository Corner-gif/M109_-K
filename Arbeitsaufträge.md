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
### Aufgabe 1
* **Welche Services existieren?**
- Wordpress
- Mariadb
* **Welcher Service ist zustandslos (stateless)?**
- Wordpress
* **Welcher Service ist zustandsbehaftet (stateful)?**
- Mariadb
* **Wie kommunizieren die Services miteinander?**
- ```WORDPRESS_DB_HOST: db:3306```
* **Welche Daten müssen persistent gespeichert werden?**
-  db_data:/var/lib/mysql

### Aufgabe 2: Mapping zu Kubernetes

| Docker Compose | Kubernetes |
|---|---|
| `service` | Deployment + Service |
| `ports` | Service (ClusterIP / NodePort / LoadBalancer) |
| `environment` | ConfigMap / Secret |
| `volumes` | PersistentVolumeClaim |
- Kubernetes startet alle Pods gleichzeitig und erwartet, dass jede App selbst wartet bis ihre Abhängigkeiten bereit sind.


# Auftrag 6.3: Appuio Tech Labs
## Lab 4: Ein Container Image deployen
1. Container Image deployen<br>
```
oc new-app quay.io/appuio/example-spring-boot --as-deployment-config
```
```
C:\Users\cleue\M109_-K\config files\Aufgabe6.1>oc get pods -w
NAME                                  READY   STATUS      RESTARTS   AGE
example-spring-boot-1-deploy          0/1     Completed   0          92s
example-spring-boot-1-dfrqt           1/1     Running     0          91s
html-openshift-app-794874775c-k9p8d   1/1     Running     0          103m
```


## Lab 5: Routen erstellen
1. routen prüfen
```
oc get routes
```
2. service namen holen
```
oc get services
```
3. ungesichert
```
oc expose service example-spring-boot
```
4. gesichert
```
oc create route edge example-spring-boot-secure --service=example-spring-boot
```

## Lab 6: Pod Scaling, Readiness Probe und Self Healing
1. neue Applikation
```
oc new-app appuio/example-php-docker-helloworld --name=appuio-php-docker --as-deployment-config
```