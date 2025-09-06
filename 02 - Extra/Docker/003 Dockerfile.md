---
tags:
  - extra/docker
---
> Un **Dockerfile** è un file di testo (con nome `Dockerfile`) che *descrive tutti i comandi necessari per assemblare un'[[002 Docker Image|image]]*.

Quindi è il punto di partenza per creare un [[001 Docker Container|container]].

![[Pasted image 20250327191957.png|800]]

## Sintassi Dockerfile
Un Dockerfile segue tipicamente questi step:
1. *Determina la [[002 Docker Image|base image]] e la working directory*
2. *Installa le dipendenze dell'applicazione*
3. *Copia del codice e altri programmi*
4. *Configura l'image finale*

> [!example]+ <font color="orange">Esempio per un progetto Python</font>
> ```dockerfile
> # Imposta la base image e la working directory
> FROM python:3.10
> WORKDIR /app
> 
> # Copia il file dei requisiti e installa le dipendenze
> COPY requirements.txt ./
> RUN pip install -r requirements.txt
> 
> # Copia il codice dell'app
> COPY . .
> 
> # Espone la porta su cui Flask verrà eseguito
> EXPOSE 5000
> # Comando per avviare l'app
> CMD ["python", "app.py"]
> ```

### 1) Base Image e Working Directory
- `FROM <image>` — specifica la *base image* che si vuole estendere, deve essere sempre la prima istruzione
- `WORKDIR <path>` — specifica la "working directory" o il *path* nell'image *dove verranno copiati i file e dove verranno eseguiti i comandi*
	- Si può considerare come il comando `mkdir <path> && cd <path>`
	- Tutte le istruzioni successive a questa verranno eseguite in questa directory


### 2-3) Installare le Dipendenze e Copiare il Codice nell'Image

> [!warning] Attenzione
> Questo step potrebbe variare in base al linguaggio di programmazione o al tipo di progetto.

- `RUN <command>` — dice *al builder* di eseguire un certo comando
- `COPY <host-path> <image-path>` — dice al builder di copiare *tutto* quello che si trova nel path dell'host e di incollarlo all'interno del path dell'image
	- Le directory e i file contenuti in `.dockerignore` verranno ignorati durante la copia

> [!info]- Differenze tra Python e Node
> Per Python:
> - Copia il file dei requisiti nella root della working directory
> - Installa con `pip` le dipendenze dal file dei requisiti
> 
> ```dockerfile
> COPY requirements.txt ./
> RUN pip install -r requirements.txt
> ```
> 
> Per Node:
> - Copia i due file di package (`package.json` e `package-lock.json`)
> - Installa le dipendenze di Node
> 
> ```dockerfile
> COPY package*.json ./
> RUN npm install
> ```


#### Dockerignore
Questo file (`.dockerignore`) funziona proprio come il `.gitignore`. 
*Ignorerà la copia di file e directory che si trovano al suo interno*.

> [!info]+ Ignorare cartelle delle dipendenze
> Questo lo si fa per evitare conflitti tra dipendenze installate localmente e sull'image.
> - Per Python ignorare le cartelle `venv` e `.venv`
> - Per Node ignorare la cartella `node_modules`

### 4) Configurare l'image
- `ENV <name> <value>` — imposta una *variabile d'ambiente* che verrà utilizzata dall'applicazione all'interno del container
- `EXPOSE <port-number>` — permette di indicare *una porta che l'immagine vorrebbe esporre* verso l'esterno
	- Se viene usato, bisogna inserire il parametro `-p` a `docker run [IMAGE]` ([[002 Docker Image#^8a08a6]])
- `CMD ["<command>", "<arg1>", "<argN>"]` — imposta *il comando di avvio dell'applicazione*, eseguito quando viene avviato il container e *dopo aver fatto il build dell'image*

## Layer Caching
Ogni istruzione che si scrive nel Dockerfile può essere considerata come un **layer**. Docker osserverà *individualmente* ognuno di questi layer per vedere se è cambiato oppure no:
- Se il layer "è cambiato", allora *si ricostruisce questo layer* (e tutti i successivi layer), per poi *salvare il risultato nella cache*
- Se il layer non è cambiato, allora *si prende il risultato dalla cache*

![[Pasted image 20250327200950.png]]

Questo è utile per **quando si cambia il codice ma non le dipendenze**, per *ridurre i tempi di build*.

## Comandi
https://docs.docker.com/
```sh
docker build . # Crea un'image a partire dal Dockerfile nella cartella corrente
```

Si possono usare questi parametri:
- `-t [NAME FOR THE NEW IMAGE]` — crea un'image con un nome
- `--no-cache` — esegue un rebuild totale dell'image senza utilizzare la cache


---
###### Risorse
- https://docs.docker.com/get-started/docker-concepts/building-images/writing-a-dockerfile/