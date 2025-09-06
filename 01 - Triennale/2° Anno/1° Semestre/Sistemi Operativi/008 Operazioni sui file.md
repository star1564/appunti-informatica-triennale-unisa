---
aliases: 
tags:
  - corsi/informatica/sistemi_operativi
paragrafo: File System
cssclasses:
  - 
---
Un file è un **Tipo di dato astratto**, definito sono dalle operazioni che si possono compiere su di esso, rese disponibili dal sistema operativo.

Le operazioni disponibili sono:
- **Creazione** di un file: richiede al SO di:
	- trovare spazio nel file system;
	- allocare il suddetto spazio per il file;
	- creare un nuovo elemento nella directory.

- **Scrittura / Lettura** di un file: il SO mette a disposizione una opportuna [[005 Chiamate di Sistema|system call]] per: 
	- specificare il nome del file su cui operare (in caso di scrittura si specificano anche i dati da scrivere);
	- dato il nome del file, il sistema ricerca la directory per individuare la posizione del file;
	- il file system mantiene un *puntatore di scrittura / lettura* alla locazione nel file in cui deve avvenire l'operazione, questo si aggiorna ogniqualvolta si esegue l'operazione (funziona come un cursore in un file di testo);
	- si occupa anche di trovare spazio sufficiente per ospitare l'eventuale espansione del file (in caso di scrittura).
- **Riposizionamento in un file**: si assegna un nuovo valore al puntatore corrente del file;
- **Cancellazione** di un file: per cancellare un file si cerca l'elemento nella directory e si rilascia lo spazio allocato al file;
- **Troncamento** di un file: quando si vuole cancellare il contenuto di un file, ma mantenere i suoi attributi;
- Rinominare, copiare, spostare, ...
- [[009 Apertura di un file|Apertura di un file]];

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

