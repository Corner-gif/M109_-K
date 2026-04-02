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
1. docker build -f <dockerfile> -t <cr-url z.b. ghcr.io/usrname/imgname:manifest> -->
2. docker push <tag> --> pushen (ggfs. docker login)
3. docker tag <old_tag><new_tag> --> tag ändern
4. docker run -d(detach) -p 8080:80 (port weiterleitung) --name <name> <tag> --> container starten
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

