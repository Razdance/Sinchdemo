1. Skapa Kind-kluster
   Starta ett lokalt Kubernetes-kluster med Kind för att köra miljön.

2. Installera Kyverno
   Installera Kyverno i klustret.

   Skapa en policy som blockerar alla deployments som saknar labeln demo=true.

   Testa policyn med en enkel deployment för att säkerställa att den fungerar.

3. Bygg Flask-app
   Skapa en enkel Python-app med Flask som returnerar HTTP 200 OK.

   Skriv en Dockerfile och bygg en Docker-image.

   Testa lokalt att imagen fungerar som den ska.

4. Skapa Namespace
   Skapa ett eget namespace för din app, exempelvis flask-app-namespace.

5. Ladda Docker-image till Kind
   Ladda upp Docker-imagen till Kind-klustret. Här kan det bli problem om imagen inte är korrekt inladdad – kontrollera noga att allt fungerar efteråt.

6. Skapa Service
   Skapa en Kubernetes Service för att exponera Flask-appen internt i klustret så andra pods kan nå den.

7. Installera Prometheus
   Installera Prometheus med Helm.

   Viktigt: Se till att rätt label (demo=true) finns på alla resurser som ska övervakas.
   Detta var lite krångligt och behövde justeras manuellt för att få allt att fungera.

   SAMMANFATTNING AV MINA ANTECKNINGAR AV CHATGPT. Lättare att visa än att lägga upp något här.
