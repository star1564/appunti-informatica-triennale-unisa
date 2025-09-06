---
tags:
  - extra/docker
---
>**Docker Compose** è uno strumento per poter definire e gestire *applicazioni multi-container*, ovvero applicazioni composte da più servizi "containerizzati". Permette di far funzionare tutti i container *insieme* per creare l'applicazione completa. 
>
>Viene tutto configurato all'interno di un singolo file YAML chiamato *`compose.yaml`*.

Con questo è possibile avviare e fermare l'intera applicazione in un singolo comando.

![[Pasted image 20250328100745.png|800]]

Infatti, risolve i problemi che si possono avere quando si cerca di far: 
- avviare tutti i container allo stesso tempo
- comunicare i container tra di loro

## Modello applicativo del Compose

### Service
I componenti di elaborazione di un'applicazione sono definiti **service**, un concetto astratto che *eseguono la stessa image del container e la stessa configurazione una o più volte*.

### Network
I service comunicano tra loro attraverso i **network**, un'astrazione della capacità di stabilire *[[026 Routing e Forwarding|routing]] IP tra contenitori* all'interno di servizi collegati tra loro.

### Volume
I service *condividono e conservano dati persistenti* all'interno dei **volume**. Essenzialmente, un volume è una cartella nel filesystem a cui Docker può accedere per: 
- salvare qualsiasi tipo di dato dai container
- condividere i dati con gli altri container

### Configs
Alcuni service richiedono dati di configurazione che dipendono dal runtime o dalla piattaforma. Per questo, si definisce un concetto di **config** dedicato. Dal punto di vista del contenitore di service, i config sono paragonabili ai volumi, in quanto *sono file montati nel container*. 

### Segreti
Un **secret** è un tipo specifico di dati di configurazione per *dati sensibili* che non dovrebbero essere esposti. I segreti sono resi disponibili ai service come *file montati nei loro container*.


## Sintassi Docker Compose

> [!example]+ <font color="orange">Esempio</font>
> ![[Pasted image 20250328105212.png]]
> 
> ```yaml
> services:
>   frontend:
>     image: example/webapp
>     ports:
>       - "443:8043"
>     networks:
>       - front-tier
>       - back-tier
>     configs:
>       - httpd-config
>     secrets:
>       - server-certificate
> 
>   backend:
>     image: example/database
>     volumes:
>       - db-data:/etc/data
>     networks:
>       - back-tier
> 
> volumes:
>   db-data:
>     driver: flocker
>     driver_opts:
>       size: "10GiB"
> 
> configs:
>   httpd-config:
>     external: true
> 
> secrets:
>   server-certificate:
>     external: true
> 
> networks:
>   # The presence of these objects is sufficient to define them
>   front-tier: {}
>   back-tier: {}
> ```

I servizi si possono dichiarare utilizzando un Dockerfile già presente oppure da zero.

```yaml
services:
  backend:
	# Servizio con un Dockerfile già configurato nella cartella corrente
    build: .
    ports:
      - "9000:9000" # Se ha una EXPOSE
  db:
    # Servizio senza Dockerfile
    image: postgres:latest
    environment:
	  POSTGRES_USER: user
	  POSTGRES_PASSWORD: pass
	  POSTGRES_DB: mydb
	volumes:
	  - postgres_data:/var/lib/postgresql/data

volumes:
  postgres_data:
```



## Comandi
https://docs.docker.com
```sh
docker compose up # Avvia tutti i servizi definiti nel file YAML
docker compose down # Ferma e rimuovi i servizi attivi
docker compose ps # Lista tutti i servizi e i loro stati
```

---
###### Risorse
- https://docs.docker.com/compose/
- https://docs.docker.com/compose/intro/compose-application-model/