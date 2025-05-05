Tog mina egna anteckningar och lät ChatGPT organisera dem:

🛠️ Steg-för-steg
1️⃣ Skapa Kind-kluster
Jag startade med att skapa ett lokalt Kind-kluster (Kubernetes in Docker).

2️⃣ Installera Kyverno och skapa policy
Installerade Kyverno i Kind-klustret för att hantera policies.

Skapade en policy som:

➡️ Blockerar alla deployments som saknar labeln demo=true.

Testade policyn med en test-deployment och verifierade att den fungerade som tänkt.

3️⃣ Bygga Python Flask-app
Skapade en enkel Python-app med Flask som returnerar HTTP 200 OK.

Skrev en Dockerfile för att paketera appen:

dockerfile
Copy
Edit
FROM python:3.9
WORKDIR /app
COPY . /app
RUN pip install flask
CMD ["python", "app.py"]
Byggde Docker-imagen och testade att den fungerar som den ska.

4️⃣ Skapa Kubernetes Namespace
Skapade ett nytt namespace för appen:

bash
Copy
Edit
kubectl create namespace flask-app-namespace
5️⃣ Ladda Docker-image till Kind
Jag stötte på problem med att pusha Docker-imagen till Kind-klustret.

Lösning:
Jag använde kommandot nedan för att ladda upp imagen korrekt till Kind:

bash
Copy
Edit
kind load docker-image <image-name>
Efter detta fungerade allt och appen körde utan problem.

6️⃣ Skapa Kubernetes Service
Jag skapade ett Kubernetes Service-manifest för att exponera Flask-appen internt så att andra pods kunde nå den:

yaml
Copy
Edit
apiVersion: v1
kind: Service
metadata:
  name: flask-app-service
  namespace: flask-app-namespace
spec:
  selector:
    app: flask-app
  ports:
    - protocol: TCP
      port: 80
      targetPort: 5000
7️⃣ Installera Prometheus
Jag installerade Prometheus med Helm:

bash
Copy
Edit
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
helm install prometheus prometheus-community/prometheus
För att Prometheus skulle hitta min app behövde jag lägga till labeln:

yaml
Copy
Edit
labels:
  demo: "true"
Det var lite krångligt och jag behövde manuellt se till att allt hade rätt labels. Prometheus fungerade bra i början men slutade tyvärr fungera senare av okänd anledning.

💡 Lärdomar
demo=true-labeln är avgörande för att både Kyverno och Prometheus ska fungera korrekt.

Att ladda Docker-images till Kind kräver kommandot kind load docker-image. En vanlig docker push fungerar inte.

Helm gör det enkelt att installera Prometheus, men label selectors kan vara trixiga att felsöka.

