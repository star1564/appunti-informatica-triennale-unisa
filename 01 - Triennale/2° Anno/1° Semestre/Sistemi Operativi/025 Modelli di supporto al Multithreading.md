---
aliases: 
tags:
  - corsi/informatica/sistemi_operativi
paragrafo: Multithreading
cssclasses:
  - 
---
### TIPI DI SUPPORTO DEL SO AI THREAD
I [[023 Thread|Thread]] possono essere distinti in:
- **Thread a Livello Utente**: sono gestiti al di sopra del livello del [[Kernel]] e senza il suo supporto e conoscenza. Vengono usate delle librerie di funzioni ([[006 API a Chiamate di Sistema#^356da0|API]]) per gestire $n$ thread in esecuzione.
	- Vantaggio: può essere implementato un pacchetto di thread anche su SO che non supportano i thread.
	- Svantaggio: una chiamata di sistema bloccante da parte di un thread bloccherebbe tutti gli altri thread (il kernel blocca il processo).
- **Thread a Livello Kernel**: sono gestiti direttamente dal sistema operativo, occupandosi della creazione, scheduling, [[026 Sincronizzazione Processi|sincronizzazione]] ed eliminazione dei thread.
	- Vantaggio: quando un thread si blocca, il kernel può eseguire altri thread (anche dello stesso processo).

## MODELLI DI SUPPORTO
In ogni caso, deve esistere una relazione tra i thread a livello utente e i thread a livello kernel.

### MODELLO MOLTI A UNO (M : 1)
In questo tipo, ci sono **molti thread a livello utente** che però corrispondono ad un **unico thread a livello kernel**.

![[Pasted image 20221110111703.png|400]]

I thread sono implementati a livello di applicazione, il loro *scheduler non fa parte del SO*, che continua a vedere il tutto come un unico processo.

- *Vantaggi*: 
	- la gestione dei thread è efficiente nello spazio utente (scheduling poco oneroso);
	- non richiede un kernel [[024 Multithreading|Multithread]] per poter essere implementato.
- *Svantaggi*:
	- l'intero processo rimane bloccato se un thread invoca una chiamata di sistema di tipo bloccante;
	- i thread sono legati allo stesso processo a livello kernel;
	- i thread non possono essere eseguiti su processori fisici distinti.

---
### MODELLO UNO A UNO (1 : 1)
Ciascun thread a livello utente **corrisponde ad un thread a livello kernel**.

![[Pasted image 20221110112111.png|500]]

Il kernel vede una traccia di esecuzione distinta per ogni thread e vengono gestiti dal suo scheduler (come se fossero processi).

- *Vantaggi*: 
	- lo scheduling è molto efficiente;
	- se un thread effettua una chiamata bloccante gli altri thread possono proseguire nella loro esecuzione;
	- i thread possono essere eseguiti su processori fisici distinti.
- *Svantaggi*:
	- può esserci inefficienza per il carico di lavoro dovuto alla creazione di molti thread a livello kernel;
	- richiede un kernel multithread per poter essere implementato.

---
### MODELLO MOLTI A MOLTI (M : M)
Si mettono in corrispondenza **più thread a livello utente** con un **numero minore o uguale di thread a livello kernel**.

![[Pasted image 20221110112616.png|500]]

Il sistema dispone di un insieme ristretto di thread, ognuno dei quali viene assegnato di volta in volta ad un thread utente.

- *Vantaggi*: 
	- c'è la possibilità di creare tanti thread a livello utente quanti sono necessari per la particolare applicazione;
	- i corrispondenti thread a livello kernel possono essere eseguiti in parallelo sulle architetture [[002 Organizzazione di un Elaboratore#CPU MULTI-CORE|multi-core]];
	- se un thread invoca una chiamata di sistema bloccante, il kernel può far eseguire un altro thread.
- *Svantaggi*:
	- c'è difficoltà nel definire il numero di thread e le modalità di cooperazione tra i due scheduler.

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