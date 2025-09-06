---
tags:
  - extra/docker
---
# %% START %%

> [!success] [Installa Docker Desktop](https://docs.docker.com/desktop/)
> Contiene anche una [[Interfaccia CLI|CLI]].


>**Docker** è un sistema che *permette di far girare un'applicazione su qualsiasi macchina host* (quindi su qualsiasi [[001 Introduzione a SO|sistema operativo]] o hardware), *senza dover installare programmi specifici* richiesti.

![[Pasted image 20250327112125.png|300]]


## I Componenti di Docker

È composto da tre componenti fondamentali:
- [[001 Docker Container|Container]]
- [[002 Docker Image|Image]]
- [[003 Dockerfile|Dockerfile]]


![[Pasted image 20250327115657.png|700]]

## Modo semplice per creare i file di Docker
Questo comando permette di avviare un processo di creazione guidato per i seguenti file con certi default in base al progetto:
- .dockerignore
- Dockerfile
- compose.yaml
- README.Docker.md

```sh
docker init
```


%% # END %%

---
###### Risorse
- https://gaweshprabhashwara.medium.com/introduction-to-docker-and-container-based-development-727c03bf466
- https://www.youtube.com/watch?v=gAkwW2tuIqE