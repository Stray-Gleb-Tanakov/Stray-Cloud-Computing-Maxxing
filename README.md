This is a message for Ian!
** 1. 
`hello-world` runt één commando (printen) en exit direct - container stopt.  
`nginx` start een server die blijft luisteren -- hoofdproces loopt door → container blijft “Running”.

**2. 
Geen download meer: image zat al lokaal in cache. Dus sneller, geen “Pulling from library/hello-world”.

**3. Waarom maar een paar processen bij `ps aux` in de container?**  
Container draait alleen wat nodig is voor die app: geen systemd, cron, etc. Alleen nginx master/worker (+ je shell als je erin zit). ps doesnt work tho

**4. `/etc/os-release` anders dan op je laptop, maar draait wel op jouw machine. Hoe?**  
Containers delen de kernel van de host, maar hebben hun eigen userspace/filesystem. Dus andere distro in de container,zelfde kernel. basically DID, second personality one brain

**5. Twee nginx-containers op verschillende poorten: commando + waarom niet dezelfde poort?**  
```bash
docker run -d -p 8080:80 --name nginx1 nginx
docker run -d -p 8081:80 --name nginx2 nginx
```
Host-poorten zijn uniek: maar één proces kan op `0.0.0.0:8080` luisteren. Tweede container geeft “address already in use”. learnt this at cybersec

**6. Verschil `docker stop` vs `docker kill`**  
- `stop`: stuurt SIGTERM → wacht (std 10s) → dan SIGKILL. Graceful shutdown.  
- `kill`: stuurt direct SIGKILL. Harde, onmiddellijke moord.

**7. Hoeveel plaats images samen? Commando?**  
```bash
docker images
```
Kijk naar kolom SIZE. Optellen voor totaal.


**1. `traefik/whoami` starten + wat toont hij?**

```bash
docker run -d -p 8080:80 --name whoami traefik/whoami
```

Browser: `http://localhost:8080`  
Toont: simpele pagina met hostname, IP, en HTTP-headers (wie je request was). Handig om te zien dat je container bereikbaar is. [hub.docker](https://hub.docker.com/r/traefik/whoami)

***

**2. Waarom kan je `postgres` niet zomaar starten zoals nginx?**

Omdat de postgres image **verplichte environment variables** nodig heeft, vooral:

- `POSTGRES_PASSWORD` (required) – superuser password, mag niet leeg zijn. [docs.docker](https://docs.docker.com/guides/postgresql/)

Zonder die vars start de container niet correct of is hij onbruikbaar (geen login mogelijk). Nginx werkt “out of the box” zonder extra config. [docs.docker](https://docs.docker.com/guides/postgresql/)

***

**3. Container zelf een naam geven: welke optie?**

Optie: `--name` [docs.docker](https://docs.docker.com/engine/containers/run.md)

Voorbeeld:

```bash
docker run -d -e POSTGRES_PASSWORD=geheim --name mijn-postgres postgres
```

Docker verzint anders random namen als `boring_kare`, `happy_euler`, etc. Met `--name` kies je zelf. [docs.docker](https://docs.docker.com/engine/containers/run.md)