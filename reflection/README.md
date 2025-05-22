# Kubernetes Nginx App Deployment

**Aufgabenbeschreibung:**

Diese Aufgabe beinhaltet das Erstellen und Deployen einer einfachen Nginx-Anwendung mithilfe von Kubernetes. Ziel ist es, die Grundlagen von Deployments, Services, Rolling Updates und Rollbacks in Kubernetes zu demonstrieren.

**Wie der Stack deployt wird:**

1.  **Voraussetzungen:** Ein funktionierendes Kubernetes-Cluster (z.B. Docker Desktop, Minikube). `kubectl` muss installiert und konfiguriert sein, um mit dem Cluster zu interagieren.
2.  **Deployment erstellen:** Die Anwendung wird über das `kubernetes/nginx-deployment.yaml` Deployment-Manifest bereitgestellt. Dieses definiert die Anzahl der Replikate und die verwendete Container-Image. Anwenden mit:
    ```bash
    kubectl apply -f kubernetes/nginx-deployment.yaml
    ```
3.  **Service erstellen:** Der Zugriff auf die Anwendung wird über den `kubernetes/nginx-service.yaml` Service vom Typ `NodePort` ermöglicht. Anwenden mit:
    ```bash
    kubectl apply -f kubernetes/nginx-service.yaml
    ```
    Der Service macht die Anwendung auf einem NodePort zugänglich, der über die Node-IP und den zugewiesenen Port im Browser erreichbar ist (z.B. `http://localhost:<NodePort>`).
4.  **Rolling Update:** Durch Ändern des Image im Deployment-Manifest und erneutes Anwenden (`kubectl apply -f kubernetes/nginx-deployment.yaml`) wird ein Rolling Update auf eine neue Version der Anwendung durchgeführt.
5.  **Rollback:** Ein Rollback zur vorherigen Version kann mit dem Befehl `kubectl rollout undo deployment/my-nginx` durchgeführt werden.

**Wo die Abgabe zu finden ist:**

Dieses Projekt-Repository enthält die folgenden relevanten Dateien und Ordner:

* `nginx-app-v1/`: Enthält das `Dockerfile` und die `index.html` für die Version 1 der Nginx-Anwendung.
* `nginx-app-v2/`: Enthält das `Dockerfile` und die `index.html` für die Version 2 der Nginx-Anwendung.
* `kubernetes/`: Enthält die Kubernetes Manifest-Dateien:
    * `nginx-deployment.yaml`: Definiert das Deployment der Nginx-Anwendung.
    * `nginx-service.yaml`: Definiert den Service, um auf die Nginx-Anwendung zuzugreifen.
* `reflection/`: Enthält die schriftlichen Reflexionsantworten in der Datei `reflection.md`.
* `README.md`: Diese Datei, die die Aufgabe, den Deployment-Prozess und den Speicherort der Abgabe beschreibt.

Das gesamte Projekt ist in diesem öffentlichen GitHub-Repository zu finden: https://github.com/yaceylan/kubernetes-nginx-app

## Screenshots

**Erstes Deployment (Version 1):**
![Nginx Version 1](reflection/screenshots/nginx v1.png)

**Nach dem Rolling Update (Version 2):**
![Nginx Version 2](reflection/screenshots/nginx v2.png)

**Nach dem Rollback (Version 1):**
![Rollback Version 1](reflection/screenshots/back v1.png)