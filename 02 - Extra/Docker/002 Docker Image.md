---
aliases:
  - Base Docker Image
cssclasses:
  - 
tags:
  - extra/docker
---
>Un'**Image** in Docker è un pacchetto software eseguibile e *standalone* che include *tutto il necessario per far avviare un'applicazione*:
>- Tecnologie necessarie
>- Codice e Runtime
>- Tool e Librerie di sistema
>- Impostazioni e Config

È essenzialmente un *template per creare uno o più [[001 Docker Container|container]]*.

![[Pasted image 20250327181621.png|700]]

Le images possono anche essere *inserite/ottenute* in/da repository online come [Docker Hub](https://hub.docker.com/):
- **pull** — ottieni una image
- **push** — pubblica una image

## Base Images
>Una **Base Image** è essenzialmente un *punto di partenza per creare un'image*. Fornisce il livello di base su cui aggiungere il codice dell'applicazione, le dipendenze e le configurazioni. Infatti, la nuova image creata andrà ad [[015 Ereditarietà|estendere]] la base image.

Su [Docker Hub](https://hub.docker.com/) si trovano delle base image curate e sicure che servono ad avere dei buoni punti di partenza. Queste includono: 
- *sistemi operativi*: come Ubuntu e Alpine
- *runtime di linguaggi di programmazione*: come Python e Node
- *altri tool*: come MySQL

![[Pasted image 20250327183439.png|700]]


## Comandi
https://docs.docker.com/
```sh
docker run [PARAMETERS] [IMAGE UUID OR NAME] # Crea un nuovo container e lo avvia a partire da un'image
```

Si possono usare questi parametri:
- `-p <port-host>:<port-container>` — effettua il port forwarding ^8a08a6
- `-d` — esegue come daemon (in background)

---
###### Risorse
- https://www.docker.com/resources/what-container/