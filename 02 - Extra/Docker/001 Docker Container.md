---
tags:
  - extra/docker
---
>Un **Container** in Docker è un *contenitore di [[Processo (Job)|processi]] isolato* che viene eseguito su una macchina host e che *ha un gruppo di risorse assegnato*.

Il processo eseguito all'interno di un container crede che è l'unico li e che si trovi in una distribuzione di Linux *minimale*.

![[Pasted image 20250327113430.png|500]]


Un container può avere due stati:
- Attivo (*started*): il processo del container è in esecuzione
- Fermo (*stopped*): il processo del container non è in esecuzione


Contiene l'applicazione da eseguire (sotto forma di [[002 Docker Image|docker image]]).

![[Pasted image 20250327175422.png]]


## Container e Macchine Virtuali
I container e le macchine virtuali sono *simili* in termini di isolamento e allocazione delle risorse, ma *funzionano in modo diverso*.

- I **container** sono un'[[Astrazione sui Dati|astrazione]] a *[[035 Livello di Applicazione|livello di applicazione]]* che *raggruppa codice e dipendenze in un singolo package*. 
	- Possono essere eseguiti più container sulla stessa macchina
	- Condividono il [[Kernel|kernel]] del sistema operativo con altri container, ciascuno dei quali viene eseguito come processo isolato nello spazio utente
	- Occupano meno spazio delle macchine virtuali (alcune centinaia di MB)
- Le **macchine virtuali** (VMs) sono un'astrazione *dell'hardware fisico* che *simula più computer all'interno di un singolo computer*. 
	- L'hypervisor consente di virtualizzare l'hardware e permette l'esecuzione di più macchine virtuali su una singola macchina
	- Ogni VM include una copia completa del sistema operativo, dell'applicazione, dei file binari e delle librerie necessarie, occupando decine di GB
	- Le macchine virtuali possono anche essere lente ad avviarsi

![[Pasted image 20250327114615.png|700]]

## Comandi
https://docs.docker.com/

```sh
docker ps # Lista tutti i container in esecuzione (usa -a per vedere anche quelli fermi)
docker container start [CONTAINER UUID OR NAME] # Start di un container
docker container stop [CONTAINER UUID OR NAME] # Stop di un container
docker container rm [CONTAINER UUID OR NAME] # Elimina un container NON IN ESECUZIONE
```


---
###### Risorse
- https://www.docker.com/resources/what-container/
- https://medium.com/dev-crumbs/scalability-and-microservices-a-road-to-docker-2d547f89755
- https://linuxiac.com/what-is-docker-container/