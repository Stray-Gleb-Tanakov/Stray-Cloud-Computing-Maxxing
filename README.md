KUBERNETES IS BELOW. KUBERNETES IS A PSYOP. KUBERNETES ISN'T REAL. IT'S CREEPING CLOSER. ENTROPY IS NEAR.

# Storingsmelder

Storingsmelder draait met Docker Compose als twee services:
- `web`: de Flask-applicatie, gebouwd uit de Dockerfile.
- `db`: PostgreSQL 18.

De applicatie bereikt PostgreSQL via de servicenaam `db`.
Een healthcheck zorgt ervoor dat de applicatie pas start wanneer
PostgreSQL verbindingen aanneemt.

## Vereisten

- Docker Desktop met Docker Compose
- Git

## Starten vanaf nul

Clone deze repository en open PowerShell in de repositorymap.
git clone https://github.com/Stray-Gleb-Tanakov/Stray-Cloud-Computing-Maxxing.git
cd Stray-Cloud-Computing-Maxxing
Copy-Item .env.example .env


Maak het lokale configuratiebestand:

```powershell
Copy-Item .env.example .env
```

Open `.env` en vul een eigen wachtwoord in bij `POSTGRES_PASSWORD`.
Gebruik voor deze configuratie een lang, willekeurig wachtwoord
met letters en cijfers.

Bouw en start de applicatie en database:

```powershell
docker compose up -d --build
```

Open http://localhost:8000.

Controleer de services en de databaseverbinding:

```powershell
docker compose ps
curl.exe http://localhost:8000/health
```

De health-endpoint hoort `"status": "ok"` en `"opslag": "PostgreSQL"`
terug te geven.

## Stoppen en opnieuw starten

```powershell
docker compose down
docker compose up -d
```

De databasegegevens blijven behouden in het named volume `postgres-data`.

## Alle gegevens verwijderen

Let op: dit verwijdert ook de opgeslagen meldingen.

```powershell
docker compose down -v
```

## Logs bekijken

```powershell
docker compose logs web
docker compose logs db
```

## Publiek image

Het applicatie-image is beschikbaar als:

`ghcr.io/stray-gleb-tanakov/storingsmelder:1.0`

## Configuratie en geheimen

Het lokale `.env`-bestand bevat het databasewachtwoord en wordt niet
gecommit. `.env.example` bevat alleen de variabelenaam met een lege waarde.





KUBERNETES ISN'T REAL, IT'S A PSYOP MADE BY THE GOVERNMENT TO MAKE YOU THINK SERVICES ARE RUNNING

# Storingsmelder

Storingsmelder draait op Kubernetes met een PostgreSQL-database.

## Vereisten
- Docker Desktop met een kind-cluster (`kind create cluster`)
- kubectl
- Het image staat publiek op ghcr.io/stray-gleb-tanakov/storingsmelder:1.1

## Opzetten
kubectl create namespace proef
kubectl apply -f k8s/ -n proef
kubectl get pods -n proef

Wacht tot de pods van de applicatie en de database Running zijn.
De applicatie herstart soms een of twee keer terwijl de database opstart.

## Bekijken
kubectl port-forward service/storingsmelder 8000:80 -n proef
Open daarna http://localhost:8000
Controle: http://localhost:8000/health toont PostgreSQL.

## Opruimen
kubectl delete namespace proef

## Opmerking
Het databasewachtwoord staat nog in de manifests. Dit wordt opgelost in opdracht 3.