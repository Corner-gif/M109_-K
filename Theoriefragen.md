# Theoriefragen
## Auftrag 1.1: NIST-Definition von Cloud Computing
* **Wie wird der Begriff Cloud definiert?**<br>
Cloud Computing ist ein Modell, das es ermöglicht, bequem, jederzeit und von überall über ein Netzwerk auf gemeinsam genutzte IT-Ressourcen zuzugreifen
* **Welches sind die 5 Merkmale einer Cloud?**<br>
1. on Demand self-service: man kann Resscourcen selber erstellen (vm)
2. Broad Network access: zugriff über das Internet von verschiedenen geräten möglich.
3. Rescource Pooling: Rescourcen werden für mehrere kunden gleichzeitig genutzt.
4. Rapid Elasticity: Schnell skalierbar
5. Measured Service: nutzung wird gemessen und bezahlt.
**wie heist der wirtschaftsbegriff den sich claud anbieter nutzen um geld zu schaffen?**
economies of scale
* **Welche Cloud Deployment Modelle kennen Sie?**<br>
1. Public
2. Private
3. Hybrid
4. Community cloud --> ähnlich wie privatecloud einfach mehrere kunden nur ausgewählte mitglieder.

* **Was sind Cloud Service Modelle?**<br>
Definieren wie und für wen eine Cloud bereitgestellt wird
IaaS --> Infrastructure as a service
PaaS --> Plattform as a service
SaaS --> Software as a Service
FaaS --> Function as a Service --> Teilmenge einer Applikation
## Auftrag 1.2: Grundlagen Cloud-Computing (Kopie)
* **Welche Cloud Dienstleistungen kennen Sie?**<br>
Onedrive, HI Cloud
* **Welche Cloud Anbieter kennen Sie?**<br>
AWS, Hürlimann Informatik AG
* **Was ist der wesentliche Unterschiede zwischen Monitoring und Logging in der Cloud?**<br>
Monitoring Zeigt was passiert logging zeichnet auf was passiert ist.
* **Weshalb soll ich Dienste aus der Cloud beziehen? Was sind die Vorteile?**<br>
Kosten, von überall erreichbar, Skalierbarkeit
* **Was sind die Nachteile?**
Abhängichkeit, weniger kontrolle, Datenschutz (daten liegen nicht bei dir)
* **Was beschreibt das Konzept der "Shared Responsibility" (geteilte Verantwortung) im Kontext der Nutzung von Public Cloud Diensten?**<br>
Anbieter schürzt die Cloud, Nutzer schützt den Inhalt.


## Auftrag 2.1: Dienstleistungen von Cloud Anbietern vergleichen<br>
| Kriterium              | AWS (Amazon Web Services)                          | Microsoft Azure                                | Google Cloud                                  |
|------------------------|----------------------------------------------------|------------------------------------------------|-----------------------------------------------|
| Dienstleistungen       | EC2, S3, RDS, DynamoDB, SageMaker, VPC             | VMs, Blob Storage, SQL DB, Cosmos DB, Azure AI | Compute Engine, Cloud Storage, Cloud SQL,     |
| Kostenstruktur         | Hoch, komplexe Preisstruktur                       | Mittel, gut für Microsoft-Kunden               | Oft günstiger, transparente Preise            |
| Skalierbarkeit         | Sehr hoch (Marktführer)                            | Sehr gut (Enterprise)                          | Sehr gut (stark bei Big Data / AI)            |
| Sicherheit             | Sehr ausgereift                                    | Sehr stark im Unternehmensbereich              | Sehr sichere Infrastruktur                    |
| Verfügbarkeit          | Sehr hoch (globale Infrastruktur)                  | Sehr hoch                                      | Sehr hoch                                     |
| Zuverlässigkeit        | Sehr hoch                                          | Hoch                                           | Hoch                                          |
| Kundensupport          | Gut, aber kostenpflichtig                          | Sehr gut für Firmen                            | Gut, aber weniger verbreitet                  |
| Vorteile               | Grösstes Angebot, flexibel, Marktführer            | Gute Integration mit Microsoft                 | Stark bei AI/ML, einfache Preise              |
| Nachteile              | Komplex, teuer                                     | Weniger übersichtlich                          | Kleineres Ökosystem                           |

## Auftrag 3.1: Grundlagen Container-Technologie

### Was ist Container-Technologie?
Container sind eine Form der Virtualisierung, bei der Anwendungen mit allen Abhängigkeiten in isolierten Einheiten (Containern) laufen.  
Sie teilen sich den Kernel des Host-Betriebssystems.

---

### Vorteile von Containern gegenüber VM
+ Sehr schnell startbereit
+ Weniger Ressourcenverbrauch
+ Kleinere Images
+ Einfach portierbar (läuft überall gleich)

### Nachteile
- Weniger Isolation als VM
- Abhängig vom Host-Kernel
- Sicherheit kann schwieriger sein

---

### Produkte (VM & Container)
**VM:**
- VMware
- VirtualBox
- Hyper-V

**Container:**
- Docker
- Kubernetes
- Podman

---

### Unterschiede: VM vs Container

| Kriterium        | VM                          | Container                     |
|------------------|-----------------------------|-------------------------------|
| Bereitstellung   | langsam                     | sehr schnell                  |
| Speicherplatz    | gross (GB)                  | klein (MB)                    |
| Portabilität     | mittel                      | sehr hoch                     |
| Effizienz        | geringer                    | sehr hoch                     |
| Betriebssystem   | eigenes OS pro VM           | teilen Host-Kernel            |

---

### Können VMs ersetzt werden?
Nein.  
Container ersetzen VMs **nicht vollständig**, da:
- VMs mehr Isolation bieten
- verschiedene Betriebssysteme möglich sind

---

### Begriffe: Container, Image, Registry
- **Container:** laufende Instanz eines Images  
- **Image:** Vorlage (inkl. App + Abhängigkeiten)  
- **Registry:** Speicherort für Images (z. B. Docker Hub)

---

### Warum sind Container immutable?
Container werden nicht verändert, sondern neu erstellt.  
→ Änderungen = neues Image  

Vorteile:
- konsistent
- reproduzierbar
- weniger Fehler

---

### Self-Managed vs Fully Managed

**Self-Managed:**
- Du verwaltest alles selbst
- mehr Kontrolle
- mehr Aufwand

**Fully Managed:**
- Anbieter übernimmt Betrieb
- weniger Aufwand
- weniger Kontrolle

---

### Kurzfazit
Container = leicht, schnell, portabel  
VM = stabil, isoliert, flexibel beim OS


## Auftrag 4.1: Container-Orchestrierung
* **Warum braucht man Container-Orchestrierung?**
Load Balancing
Horizontale Skalierbarkeit
überwachen verfügbarkeit usw.
* **Wie funktioniert Container-Orchestrierung?**
   * Container-Orchestrierung automatisiert das Verwalten von Containern.
* **Welche Container-Orchestrierung Technologien kennen Sie?**
   * Kubernetes
   * Docker Swarm
* **Was versteht man unter horizontaler Skalierung im Kontext von Cloud-Anwendungen?**
   * Mehr Instanzen statt stärkere Hardware
* **Was gibt es für Deployment Strategien?**
    * Recreate: alte Version stoppen → neue starten (Downtime)
    * Rolling Update: schrittweise ersetzen (keine Downtime)
    * Blue-Green: zwei Umgebungen, Umschalten
    * Canary: neue Version nur für wenige User testen
container:
Kleinerer rescourcenverbrauch
getrennte binaries

CLI --> Command Line Interface
Web UI
API -->

## Auftrag 4.2 Container Orchestration mit Docker Compose

* **Beantworten Sie folgende Fragen:**
* **Was ist Redis?**

* **Welche Ports werden genutzt?**
* **Was ist die Bedeutung von ENV im Dockerfile?**