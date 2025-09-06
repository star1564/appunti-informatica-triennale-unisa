---
tags:
  - corsi/informatica/sistemi_operativi
---
Si consideri un sistema composto di $n$ [[Processo (Job)|Processi]] $$\{P_0, P_1, ..., P_{n-1}\}$$
ciascuno avente un segmento di codice in comune, chiamato **Sezione Critica**. 
All'interno di questa, ogni processo può modificare variabili comuni, aggiornare una tabella, scrivere in un file, ecc.

La cosa importante è che in questo sistema, quando un processo in esecuzione è entrato nella sua sezione critica, *non si consente ad alcun altro processo di essere in esecuzione*.

Per assicurarsi che questo accada, si usano le sezioni di entrata e di uscita.

#### SEZIONI D'INGRESSO E D'USCITA

```C
while(1){
	sezione d'ingresso
		/*sezione critica*/

	sezione d'uscita
		/*sezione NON critica*/
}
```

- Ogni processo deve chiedere il permesso per entrare nella propria sezione critica. La sezione di codice che realizza questa richiesta è la **Sezione d'ingresso**;
- La sezione critica può essere seguita da una **Sezione d'uscita**, che fa capire agli altri processi che c'è la possibilità di accedere alla propria sezione critica;
- La restante parte del codice viene detta *sezione non critica*.

Queste sezioni devono garantire:
- *Mutua Esclusione*: se il processo $P_1$ è in esecuzione nella sua sezione critica, nessun altro processo può essere in esecuzione nella propria sezione critica;

- *Progresso*: se nessun processo è in esecuzione nella sua sezione critica e qualche processo desidera accedervi, allora la sezione del processo che entrerà prossimamente nella propria sezione critica non può essere rimandata indefinitamente (evitare il *[[Deadlock]]*);

- *Attesa Limitata*:  se un processo ha effettuato la richiesta di ingresso nella sezione critica, esiste un limite al numero di volte che si consente ad altri processi di entrare nelle rispettive sezioni critiche, prima che si accordi la richiesta del primo processo (per evitare *[[Starvation]]*).


%%
[[000 Indice SO|↖ Ritorna all'indice ↖]]
%%
