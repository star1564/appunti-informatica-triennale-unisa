---
aliases: 
tags:
  - corsi/informatica/sistemi_operativi
paragrafo: Sincronizzazione Processi
cssclasses:
  - 
---
Mentre un [[Processo (Job)|Processo]] si trova nella propria [[Sezione Critica]], qualsiasi altro processo che tenti di entrare nella sezione critica si trova nel ciclo del codice della entry section (ovvero `while (S <= 0);`) girando in attesa del semaforo (*Spinlock*). 
La condizione di attesa spreca cicli di CPU che altri processi potrebbero sfruttare produttivamente.

Questo fenomeno si chiama **Busy Waiting**.

### EVITARE IL BUSY WAITING
Per evitare di lasciare un processo in attesa nel ciclo `while` si può ricorrere ad una definizione alternativa di [[028 Semafori|semaforo]].
Si ha una [[018 Strutture|struttura]] contenente:
- Un valore intero (numero di processi in attesa);
- L'head di una lista (rappresentante una coda dei processi in attesa)

```C
typedef struct{
	int valore;
	struct processo *lista;
} Semaforo;
```

Si assume che siano disponibili due operazioni fornite dal SO come [[005 Chiamate di Sistema|system call]]:
- **`block()`**: posiziona il processo che richiede di essere bloccato nell'opportuna coda di attesa, ovvero sospende il processo che la invoca;
- **`wakeup()`**: rimuove un processo dalla coda di attesa e lo sposta nella ready queue.

```C
wait(Semaforo *s){
	s->valore--;
	if(s->valore < 0){
		//aggiungi il processo P a s->lista
		block(P);
	}
}

signal(Semaforo *s){
	s->valore++;
	if(s->valore <= 0){
		//rimuovi il processo P a s->lista
		wakeup(P);
	}
}
```

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