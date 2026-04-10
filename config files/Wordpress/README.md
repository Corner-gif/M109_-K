# Docker Compose Analyse – WordPress Stack

## Ausgangslage

Der Docker-Compose-Stack besteht aus einem WordPress-Service und einer MariaDB-Datenbank.

---
## Aufgabe 1

### 1. Welche Services existieren?

Es existieren zwei Services:

- `wordpress`
- `db`

Der Service `wordpress` stellt die Webanwendung bereit, während `db` die Datenbank (MariaDB) ist.

### 2. Welcher Service ist zustandslos (stateless)?

Der Service **`wordpress`** ist zustandslos.

Begründung:  
Der Container speichert keine persistenten Daten lokal. Alle relevanten Daten werden in der Datenbank gespeichert. Der Container kann jederzeit ersetzt werden, ohne dass Daten verloren gehen.

### 3. Welcher Service ist zustandsbehaftet (stateful)?

Der Service **`db`** ist zustandsbehaftet.

Begründung:  
Die Datenbank speichert dauerhaft Daten wie Inhalte und Benutzerinformationen. Dafür wird ein Volume verwendet:

```yaml
volumes:
  - db_data:/var/lib/mysql
```


---

## Aufgabe 2: Mapping zu Kubernetes

| Docker Compose | Kubernetes |
|---|---|
| service | Deployment + Service |
| ports | containerPort (Pod) + Service (port / targetPort / nodePort) |
| environment | Config Map, Secret |
| volumes | PersistentVolumeClaim |
| depends_on | kein direktes Pendant |

### Zusatzfrage: Warum gibt es depends_on in Kubernetes nicht?

Kubernetes arbeitet nicht mit Startreihenfolgen, sondern mit Zuständen.  
Ein Container kann zwar gestartet sein, aber noch nicht bereit. Deshalb nutzt Kubernetes Mechanismen wie automatische Wiederholungen und Health Checks statt einer festen Reihenfolge.

---

## Aufgabe 3: WordPress Deployment erstellen

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: wordpress
spec:
  replicas: 1
  selector:
    matchLabels:
      app: wordpress
  template:
    metadata:
      labels:
        app: wordpress
    spec:
      containers:
        - name: wordpress
          image: wordpress:6.4-apache
          ports:
            - containerPort: 80
          env:
            - name: WORDPRESS_DB_HOST
              value: "db:3306"
            - name: WORDPRESS_DB_USER
              value: "wordpress"
            - name: WORDPRESS_DB_PASSWORD
              value: "wordpress"
            - name: WORDPRESS_DB_NAME
              value: "wordpress"
```

---

## Aufgabe 4 WordPress Service erstellen

```yaml
apiVersion: v1
kind: Service
metadata:
  name: wordpress
spec:
  type: NodePort
  selector:
    app: wordpress
  ports:
    - port: 80
      targetPort: 80
```

Die Anwendung ist über ```http://<NODE-IP>:<NODE-PORT>``` erreichbar. 

Den Port kann man mit dem Befehl ```kubectl get svc wordpress``` herausfinden. Um es zu erreichen, muss man zuerst noch einen Portforward machen (in der Anleitung weiter unten).


---

## Aufgabe 5 MariaDB Deployment

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: db
spec:
  replicas: 1
  selector:
    matchLabels:
      app: db
  template:
    metadata:
      labels:
        app: db
    spec:
      containers:
        - name: mariadb
          image: mariadb:10.6
          ports:
            - containerPort: 3306
          env:
            - name: MYSQL_DATABASE
              value: "wordpress"
            - name: MYSQL_USER
              value: "wordpress"
            - name: MYSQL_PASSWORD
              value: "wordpress"
            - name: MYSQL_ROOT_PASSWORD
              value: "rootpassword"
          volumeMounts:
            - name: db-storage
              mountPath: /var/lib/mysql
      volumes:
        - name: db-storage
          persistentVolumeClaim:
            claimName: db-pvc
```

---

## Aufgabe 6 Persistente Speicherung

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: db-pvc
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 1Gi
```

### Warum ist Persistenz notwendig?

Die Datenbank speichert alle wichtigen Inhalte wie Benutzer, Beiträge und Einstellungen.
Ohne Persistenz wären diese Daten bei jedem Neustart des Pods verloren.

---

## Aufgabe 7 Datenbank Service

```yaml
apiVersion: v1
kind: Service
metadata:
  name: db
spec:
  selector:
    app: db
  ports:
    - port: 3306
      targetPort: 3306
```

### Warum muss der Service genau "db" heissen?

Weil WordPress über die Variable WORDPRESS_DB_HOST=db:3306 auf die Datenbank zugreift.
Der Service-Name ist gleichzeitig der DNS-Name im Kubernetes-Cluster.

---

## Aufgabe 8 Verbindung testen

Um die ganzen yml Files nun anzuwenden werden die folgenden Befehle ausgeführt:

```bash
kubectl apply -f db-pvc.yaml
kubectl apply -f mariadb-deployment.yaml
kubectl apply -f db-service.yaml
kubectl apply -f wordpress-deployment.yaml
kubectl apply -f wordpress-service.yaml
```
**Danach müssen die Ports noch forwarded werden mit dem Befehl ```kubectl port-forward svc/wordpress 8080:80```**

Nun kann man das ganze mit diesen Befehlen Überprüfen:

```bash
kubectl get pods
kubectl get svc
kubectl get pvc
```

## Aufgabe 9 Debugging

```bash
kubectl get pods
kubectl describe pod <name>
kubectl logs <name>
```

**Typische Probleme**

- Pod startet nicht → falsche Environment-Variablen
- Keine Datenbankverbindung → Service-Name falsch
- Datenbank verliert Daten → kein Volume vorhanden
- Kein Zugriff von außen → falscher NodePort oder IP

---

## Zusatzaufträge

db-secret.yaml:

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: db-secret
type: Opaque
stringData:
  MYSQL_DATABASE: wordpress
  MYSQL_USER: wordpress
  MYSQL_PASSWORD: wordpress
  MYSQL_ROOT_PASSWORD: rootpassword
  WORDPRESS_DB_PASSWORD: wordpress
```

wordpress-configmap.yaml:

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: wordpress-config
data:
  WORDPRESS_DB_HOST: "db:3306"
  WORDPRESS_DB_USER: "wordpress"
  WORDPRESS_DB_NAME: "wordpress"
```

mariadb-deployment.yaml:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: db
spec:
  replicas: 1
  selector:
    matchLabels:
      app: db
  template:
    metadata:
      labels:
        app: db
    spec:
      containers:
        - name: mariadb
          image: mariadb:10.6
          ports:
            - containerPort: 3306
          env:
            - name: MYSQL_DATABASE
              valueFrom:
                secretKeyRef:
                  name: db-secret
                  key: MYSQL_DATABASE
            - name: MYSQL_USER
              valueFrom:
                secretKeyRef:
                  name: db-secret
                  key: MYSQL_USER
            - name: MYSQL_PASSWORD
              valueFrom:
                secretKeyRef:
                  name: db-secret
                  key: MYSQL_PASSWORD
            - name: MYSQL_ROOT_PASSWORD
              valueFrom:
                secretKeyRef:
                  name: db-secret
                  key: MYSQL_ROOT_PASSWORD
          volumeMounts:
            - name: db-storage
              mountPath: /var/lib/mysql
      volumes:
        - name: db-storage
          persistentVolumeClaim:
            claimName: db-pvc
```

wordpress-deployment.yaml:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: wordpress
spec:
  replicas: 2
  selector:
    matchLabels:
      app: wordpress
  template:
    metadata:
      labels:
        app: wordpress
    spec:
      containers:
        - name: wordpress
          image: wordpress:6.4-apache
          ports:
            - containerPort: 80
          env:
            - name: WORDPRESS_DB_HOST
              valueFrom:
                configMapKeyRef:
                  name: wordpress-config
                  key: WORDPRESS_DB_HOST
            - name: WORDPRESS_DB_USER
              valueFrom:
                configMapKeyRef:
                  name: wordpress-config
                  key: WORDPRESS_DB_USER
            - name: WORDPRESS_DB_NAME
              valueFrom:
                configMapKeyRef:
                  name: wordpress-config
                  key: WORDPRESS_DB_NAME
            - name: WORDPRESS_DB_PASSWORD
              valueFrom:
                secretKeyRef:
                  name: db-secret
                  key: WORDPRESS_DB_PASSWORD
```

- Die sensiblen Zugangsdaten wurden aus den Deployments entfernt und in einem Kubernetes Secret gespeichert. 

- Allgemeine Konfigurationswerte wurden in einer ConfigMap abgelegt. 
  - Dadurch sind Konfiguration und geheime Daten sauber getrennt. 

- Zusätzlich wurde das WordPress-Deployment von 1 auf 2 Replicas skaliert, damit mehrere Instanzen der Anwendung gleichzeitig laufen können.