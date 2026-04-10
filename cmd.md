# Commands
## Git

1. git innit --> repo Initialisieren
2. git clone <url>/<ssh> --> repo klonen
3. git status --> status anzeigen
4. git add. <verzeichnis> --> nimmt alle änderungen ins staging
5. git commit -m "nachricht" --> änderungen committen
6. git push --> alle committeten änderungen ins repo pushen
7. git pull --> alle änderungen aus dem repo holen<br>
## Docker Befehle
1. docker build -f <dockerfile> -t <cr-url z.b. ghcr.io/usrname/imgname:manifest>
```
docker build -f Dockerfile.web -t ghcr.io/corner-gif/web01:latest .
```
2. docker push <tag> --> pushen (ggfs. docker login)
3. docker tag <old_tag><new_tag> --> tag ändern
4. docker run -d(detach) -p 8080:80 (port weiterleitung) --name <name> <tag> --> container starten
```
docker run -d -p 8080:80 --name web01 ghcr.io/corner-gif/web01:latest
```
5. docker exec -it(interactive) <tag> /bi/sh oder /bin/bash 
6. docker logs <name> --> docker logs anzeigen
7. docker stop <id/name> --> container stoppen
8. docker rm <id/name> --> container löschen
9. docker rmi <id/name> --> images löschen
10. docker system prune -a(alles) --volumes --> alles weg
11. docker ps (-a) --> container anzeigen<br>


pots = Container
alles auslagern
immutable
besteht aus:
1. applikation
2. middleware (nginx, appache)
3. OS (alpine, coreos)
4. auslagern ins git
5. mw in container registry
6. dockerfile baut zusammen
7. pot kann gelöscht werden

Openshift:
1. oc projects --> alle projekte
2. oc project 264495-corleu --> projekt wechseln


Kubectl:
kubectl get <was auch immer>
kubectl apply -f <file>
kubectl describe service <servicename>

OC:
- oc get service <Service Name>
- oc get services <pod Name>
- oc get pods
- oc get services
- oc describe <pod/service> <pod name/service name>
- oc get imagestream <service name> -o json
- oc get deploymentConfig example-spring-boot -o json
- oc get routes
- oc expose service <service name> --> ungesicherte route
- oc create route edge example-spring-boot-secure --service=example-spring-boot --> gesicherte route<br>
- oc get rc --> ReplicationController

