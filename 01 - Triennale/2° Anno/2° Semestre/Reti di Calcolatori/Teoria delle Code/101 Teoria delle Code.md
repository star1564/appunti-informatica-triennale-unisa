---
tags:
  - corsi/informatica/reti_calcolatori
paragrafo: Teoria delle Code
cssclasses:
  - 
---
> La **Teoria delle Code** studia i vari *fenomeni di attesa* all'interno di [[022 Queue nel C#DEFINIZIONE|Code]] alla richiesta di un servizio. Lo scopo finale è quello di aumentare l'efficienza delle code. 

Questo formalismo lo si può applicare in molte applicazioni, tra cui le reti e lo [[021 Scheduling di Processi e Code|scheduling di processi nei sistemi operativi]].

---
### Sistema di Servizio
Un **Sistema di Servizio** rappresenta una struttura a cui arrivano casualmente dei client che richiedono lo svolgimento di una operazione o servizio da parte di uno o più servente, per poi uscire.

![[Pasted image 20230426144904.png|500]]

> [!summary] Timeline
> 1. I clienti che richiedono un servizio sono generati nel tempo dalla popolazione.
> 2. I clienti entrano nel sistema e raggiungono la coda.
> 3. Ad un certo momento, un cliente viene selezionato secondo la disciplina della coda;
> 4. Il servizio richiesto è effettuato da un servente;
> 5. Il cliente lascia il sistema.

Questo sistema è composto da:
- *Popolazione*: il numero totale dei distinti potenziali clienti che richiedono un servizio (caratterizzati singolarmente dalla loro dimensione);
 ^efe251
- *Numero di Serventi*: il numero totale di serventi che lavorano in serie o in parallelo. Ogni servente può svolgere l'operazione per un solo cliente per volta; ^289f68
	- $s$ rappresenta questo numero.

- *Schema di arrivo*: il modo ([[004 Automi Finiti Deterministici#Deterministici|deterministico]] o [[023 Variabili Aleatorie|aleatorio]]) secondo il quale i **clienti si presentano** a richiedere il servizio, e viene definito in termini di intervalli di tempo tra due arrivi successivi di clienti nel sistema (tempo di inter-arrivo); ^2bb0f3
	- Ovvero la distribuzione di probabilità degli intertempi di arrivo; 
	- $t_i^a$ rappresenta il tempo che intercorre tra l'arrivo del cliente $i$-esimo e il cliente ($i-1$)-esimo;
	- Può essere calcolato dividendo un tempo totale per il numero di clienti arrivati in quel lasso di tempo.

- *Schema di servizio*: il modo (deterministico o aleatorio) secondo il quale i **serventi svolgono un servizio** ai clienti; ^614518
	- Ovvero la distribuzione di probabilità degli intertempi di servizio; 
	- $t_i^s$ rappresenta il tempo che intercorre per svolgere un servizio da parte di un servente al cliente $i$-esimo.

- *Capacità del sistema*: numero massimo di utenti che possono essere contemporaneamente nel sistema, comprendendo sia gli utenti in attesa in coda, sia quelli che stanno fruendo del servizio;
 ^e5d338
- *Disciplina della coda*: l'ordine rispetto al quale gli utenti vengono serviti ([[022 Queue nel C#^a60071|FIFO]], [[021 Stack nel C#^c22c74|LIFO]], [[022 CPU Scheduler#A PRIORITÀ|Criteri di Priorità]], Service In Random Order);
 ^df9c48
- *Dimensione della coda*: numero di utenti che possono trovare posto nella coda. 
	- $t_i^q$ rappresenta il tempo passato in coda dall'$i$-esimo utente nella coda prima di iniziare ad usufruire del servizio richiesto. ^1ceae4

- *Tempo passato nella coda* del cliente $i$-esimo (assumendo che $t_i^a$ e $t_i^s$ siano indipendenti e identicamente distribuiti): $$t_i^w=t_i^q+t_i^s$$ ^8c74b4

___
[[000 Indice RC|↖ Ritorna all'indice ↖]] , [[102 Notazione di Kendall|Nota Successiva →]]

Altri collegamenti: 