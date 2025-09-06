---
aliases: 
tags:
  - corsi/informatica/sistemi_operativi
paragrafo: Processi
cssclasses:
  - 
---
Ogni qualvolta che la CPU passa nello stato di inattività, il SO sceglie per l'esecuzione uno dei processi in [[019 Stati di un Processo#^610966|stato ready]].

In molti algoritmi di schedulazione si ha la:
- Schedulazione **non-preemptive** (senza prelazione), quando un processo passa dallo stato di esecuzione allo stato di attesa, oppure quando termina:
	- In questo caso, quando si assegna la CPU a un processo, questo rimane in possesso della CPU *fino al momento del suo rilascio*.
- Schedulazione **preemptive** (con prelazione), quando un processo passa dallo stato di esecuzione o dallo stato di attesa allo stato pronto:
	- In questo caso, quando si assegna la CPU a un processo, questo *può essere rimosso* se si presenta un nuovo processo con una maggiore priorità.

#### CRITERI DI SCHEDULING
Diversi algoritmi di scheduling della CPU hanno proprietà differenti e possono favorire una particolare classe di processi. Si considerano quindi:
- L'*Utilizzo della CPU*: La CPU deve essere più attiva possible;
- Il *Throughput* (frequenza di completamento, produttività): numero di processi completati per ogni unità di tempo;
- Il **Tempo di Completamento** (*turnaround time*): intervallo che va dal momento dell'immissione del processo nel sistema fino al momento del completamento;
- **Tempo di Attesa** (*waiting time*): somma degli intervalli d'attesa di un singolo processo passati nella [[021 Scheduling di Processi e Code#CODE DI SCHEDULING PER PROCESSI|coda dei processi pronti]];
	- **Tempo di Attesa Medio**: media aritmetica del tempo di attesa, ovvero $\frac{wt_1\ +\ wt_2\ +\ ...\ +\ wt_n}{n}$.
- *Tempo di Risposta*: tempo che intercorre dalla formulazione della prima richiesta fino alla produzione della prima risposta (non è l'output).

## ALGORITMI DI SCHEDULING

### FCFS (FIRST COME FIRST SERVED)
Con questo algoritmo, alla CPU si assegnano i processi **nell'ordine in cui questi sono arrivati**.  
Questo algoritmo ha *tempi medi di attesa lunghi*.

Per esempio: 

| Processo | CPU Burst |
| -------- | --------- |
| $P_1$    | 24        |
| $P_2$    | 3         |
| $P_3$    | 3         |

Abbiamo il seguente diagramma di Gantt:
![[Pasted image 20221020103938.png|500]]

Tempo di attesa per: 
- $P_1$ = 0; 
- $P_2$ = 24; 
- $P_3$ = 27.

Tempo medio di attesa: $\frac{0+24+27}{3}=17$.

---
### SJF (SHORTEST JOB FIRST)
Con questo algoritmo, alla CPU si assegnano i processi **con il prossimo CPU burst più breve**.  
Si ha una versione *preemptive e non-preemptive*.
Per esempio: 

| Processo | Tempo di Arrivo | CPU Burst |
| -------- | --------------- | --------- |
| $P_1$    | 0.0             | 7         |
| $P_2$    | 2.0             | 4         |
| $P_3$    | 4.0             | 1         |
| $P_4$    | 5.0             | 4         |

##### Versione non-preemptive
Abbiamo il seguente diagramma di Gantt:
![[Pasted image 20221020104711.png|500]]

Il tempo di attesa si calcola così: $\color{#61AFEF}tempo\ partenza\ processo - tempo\ di\ arrivo$.
Tempo di attesa per: 
- $P_1$ = (0-0) = 0; 
- $P_2$ = (8-2) = 6; 
- $P_3$ = (7-4) = 3; 
- $P_4$ = (12-5) = 7.

Tempo medio di attesa: $\frac{0+6+3+7}{4}=4$.

##### Versione preemptive
Abbiamo il seguente diagramma di Gantt:
![[Pasted image 20221020105300.png|500]]

Il tempo di attesa si calcola così: $\color{#61AFEF}tempo\ partenza\ ultima\ parte\ del\ processo - tempo\ finale\ prima\ parte\ del\ processo + diff.\ arrivo$.
Tempo di attesa per: 
- $P_1$ = (11-2) = 9; 
- $P_2$ = (5-4) = 1; 
- $P_3$ = (5-5) = 0; 
- $P_4$ = (11-11 + (7-5)) = 2.

Tempo medio di attesa: $\frac{9+1+0+2}{4}=3$.

---
### A PRIORITÀ
Con questo algoritmo, si associa una priorità numerica a ciascun processo. Alla CPU si assegnano i processi **con priorità più alta**.  

Un problema che si potrebbe avere è che i processi a bassa priorità potrebbero non essere mai eseguiti (*[[Starvation]]*). La soluzione sarebbe l'invecchiamento (*aging*) dei processi, ovvero l'ascensione graduale della priorità con il passare del tempo.

In generale, più la CPU burst è piccola, più la priorità cresce.

Si ha una versione *preemptive e non-preemptive*.

La versione preemptive è usata in molti sistemi operativi (Windows XP, Linux, Solaris, ...), ognuno ne ha la sua versione personalizzata.

Per esempio: 

| Processo | Tempo di Arrivo | CPU Burst | Priorità |
| -------- | --------------- | --------- | -------- |
| $P_1$    | 0.0             | 10        | 3        |
| $P_2$    | 5.0             | 1         | 1        |
| $P_3$    | 3.0             | 2         | 3        |
| $P_4$    | 10.0            | 1         | 2        |
| $P_5$    | 11.0            | 5         | 4        |

##### Versione non-preemptive
Abbiamo il seguente diagramma di Gantt:
![[Pasted image 20221020110505.png|500]]

Tempo di attesa per: 
- $P_1$ = (0-0) = 0; 
- $P_2$ = (10-5) = 5; 
- $P_3$ = (12-3) = 9; 
- $P_4$ = (11-10) = 1;
- $P_5$ = (14-11) = 3.

Tempo medio di attesa: $\frac{0+5+9+1+3}{5}=3,6$.

##### Versione preemptive
Abbiamo il seguente diagramma di Gantt:
![[Pasted image 20221020110727.png|500]]

Tempo di attesa per: 
- $P_1$ = ((8-5) + (11-10)) = 4; 
- $P_2$ = (6-6) = 0; 
- $P_3$ = ((8-8) + (6-3)) = 3; 
- $P_4$ = (11-11) = 0;
- $P_5$ = ((19-19) + (14-11)) = 3.

Tempo medio di attesa: $\frac{4+0+3+0+3}{5}=2$.

---
### ROUND ROBIN (CIRCOLARE)
Ogni processo riceve la CPU per una piccola unità di tempo (*time quantum*). Se entro questo arco di tempo il processo non lascia la CPU, **viene rimesso nella coda** dei processi pronti.
![[Pasted image 20221020111438.png|570]]

Per esempio, con *tempo pari a 20 millisecondi*: 

| Processo | CPU Burst |
| -------- | --------- |
| $P_1$    | 53        |
| $P_2$    | 17        |
| $P_3$    | 68        |
| $P_4$    | 24        |

Abbiamo il seguente diagramma di Gantt:

![[Pasted image 20221020111605.png|530]]

Tempo di attesa per: 
- $P_1$ = 81; 
- $P_2$ = 0; 
- $P_3$ = 57; 
- $P_4$ = 40.

Tempo medio di attesa: $\frac{81+0+57+40}{4}=44,5$.



___
[[000 Indice SO|↖ Ritorna all'indice ↖]]

```dataviewjs
/*
function extractUpperCaseLetters(inputString) {
	// Use a regular expression to match uppercase letters (A-Z)
	const uppercaseLetters = inputString.match(/[A-Z]/g);
	
	// Check if uppercaseLetters is not null, and join the matched letters into a string
	if (uppercaseLetters !== null) {
		return uppercaseLetters.join('');
	} else {
	    // If no uppercase letters were found, return an empty string
	    return '';
	}
}
*/

function extractNumberFromString(inputString) {
	const match = inputString.match(/^\d{3}/);
	
	if (match) {
		return match[0];
	} else {
		return null; // Return null if no match is found
	}
}

function startsWithNumber(inputString, targetNumber) {
  // Use a regular expression to check if the string starts with the target number
  const pattern = new RegExp(`^${targetNumber}`);
  return pattern.test(inputString);
}

function sort2Array(array){
	let res = []
	
	if (array[0] == undefined)
		res.push(array[1], array[1])
	else if (array[1] == undefined)
		res.push(array[0], array[0])
	else if (parseInt(extractNumberFromString(array[0])) > parseInt(extractNumberFromString(array[1])))
		res.push(array[1], array[0])
	else
		res.push(array[0], array[1])
	
	return res
}

let toDisplay = []
function searchPrevAndNext(tag, currentNumber) {
	const prevNumber = (parseInt(currentNumber) - 1).toString().padStart(3, "0");
	const nextNumber = (parseInt(currentNumber) + 1).toString().padStart(3, "0");
	
	let prevAndNext = [(dv.pages(tag).where(p => startsWithNumber(p.file.name, prevNumber) || startsWithNumber(p.file.name, nextNumber)).file.name)]
	
	// Prev = [0]; Next = [1]
	let sortedPrevAndNext = sort2Array(prevAndNext[0])
	
	if (currentNumber == "001"){ 
		// Se è la prima pagina aggiungi solo il prossimo
		let nextLink = "[[" + sortedPrevAndNext[1] + "|" + "Nota Successiva →" + "]]"
		toDisplay.push(nextLink)
	} else if (prevAndNext[0][1] == undefined){
		// Se è l'ultima pagina aggiungi solo il precedente
		let prevLink = "[[" + sortedPrevAndNext[0] + "|" + "← Nota Precedente" + "]]"
		toDisplay.push(prevLink)
	} else {
		let nextLink = "[[" + sortedPrevAndNext[1] + "|" + "Nota Successiva →" + "]]"
		toDisplay.push(nextLink)
		
		let prevLink = "[[" + sortedPrevAndNext[0] + "|" + "← Nota Precedente" + "]]"
		toDisplay.push(prevLink)
	}
	
	
}

if (dv.current().tags[0] == null || dv.current().tags[0] == undefined){
	dv.header(1, "Errore - Inserire il tag nelle proprietà del file")
} else {
	let tag = "#" + dv.current().tags[0]

	// Purtroppo obsidian non riesce a leggere i link e a creare connessioni nel grafo,
	// quindi ho disattivato il link all'indice automatico e la funzione extractUpperCaseLetters
	//let indexName = "000 Indice " + extractUpperCaseLetters(tag)
	//let index = dv.page(indexName).file
	//let indexLink = "[[" + index.name + "|" + "↖ Ritorna all'indice ↖" + "]]"
	//toDisplay.push(indexLink)
	
	let currentPage = dv.current().file
	let currentPageNumber = extractNumberFromString(currentPage.name)
	
	searchPrevAndNext(tag, currentPageNumber)
	
	dv.list(toDisplay)
}
```

Altri collegamenti: 