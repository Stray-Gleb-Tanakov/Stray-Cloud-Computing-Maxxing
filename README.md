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