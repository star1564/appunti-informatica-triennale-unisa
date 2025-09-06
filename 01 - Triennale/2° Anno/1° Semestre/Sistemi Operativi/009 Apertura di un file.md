---
aliases:
  - File Descriptor
tags:
  - corsi/informatica/sistemi_operativi
paragrafo: File System
cssclasses:
  - 
---
La maggior parte delle [[008 Operazioni sui file|operazioni sui file]] richiede una ricerca nella directory dell'elemento associato al file specificato.

Per evitare questa continua ricerca, molti sistemi richiedono l'impiego di due [[005 Chiamate di Sistema|chiamate di sistema]] `open()` e `close()`.

Prima che un file venga utilizzato si usa la `open()` per inserirlo in una **Tabella dei File Aperti** mantenuta dal sistema operativo nella memoria principale.

Quando si richiede un'operazione su un file, questo viene individuato tramite un *indice* in tale tabella, evitando qualsiasi ricerca.

Quando il file non viene più attivamente usato, viene chiuso con la `close()` e quindi viene rimosso dalla tabella.

![[Pasted image 20221008165238.png]]

Ci sono due tipi di tabelle:
- La tabella **di tutti i file aperti** nel sistema, *indipendente* da tutti i processi, contenente le *informazioni relative ai singoli file*;

- La tabella dei file **aperti da un processo**, contenente: ^d52b90
	- i *file descriptor*: tag numerico (intero non negativo) che identifica univocamente un file aperto da un processo; ^8ad3ec
	- i *puntatori* alle tabelle dei singoli file, contenenti:
		1. flag di stato, aiuta a capire certe cose sul file aperto (come sola lettura, scrittura, ecc.);
		2. current file offset, posizione corrente all'interno del file; ^5aa314
		3. v-node ptr, puntatore alla tabella dei file aperti nel sistema con gli [[007 File#ATTRIBUTI DI UN FILE|attributi del file]] ed il [[File Control Block|FCB (i-node)]].

Quando un processo inizia, nella tabella dei file del processo vengono aperti tre file: [[Standard Input-Output|stdin (fd 0), stdout (fd 1), stderr (fd 2)]].

> [!question]- Cosa succede quando più di un processo deve accedere ad un file?
> Il puntatore alla tabella dei file aperti punterà allo stesso file.
> 
> ![[Pasted image 20221006125341.png]]

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
