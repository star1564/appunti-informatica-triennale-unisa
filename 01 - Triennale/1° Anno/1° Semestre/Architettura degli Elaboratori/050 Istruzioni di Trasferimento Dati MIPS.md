---
aliases: 
tags:
  - corsi/informatica/architettura_degli_elaboratori
paragrafo: MIPS - Assembly
cssclasses:
  - 
---
Le operazioni aritmetiche si effettuano solo tra registri. Dato che questi sono pochi abbiamo bisogno di istruzioni che *trasferiscono dati da e verso la memoria*.

La **memoria** è un grande array con *$2^{32}$* celle. Ciascuna cella può contenere 8 bit (1 byte).

A ciascun byte è assegnato un **indirizzo** *lineare e progressivo*, tramite cui può essere prelevato.

Ciascuna **parola** (word) di 32 bit (4 byte) necessita di *4 locazioni successive* nella memoria. Quindi per passare da una word alla successiva bisogna incrementare l'indirizzo della cella di 4. ^61b5d3

## LOAD

[[048 Formati delle istruzioni MIPS#FORMATO I|Formato I]].
La `lw` *preleva* il contenuto di una locazione di memoria e lo trasferisce in un registro.
Bisogna specificare l'indirizzo della cella in cui la word è memorizzata.
Abbiamo un registro di *base* ed un *offset*.

**`lw $t0, 4($s3)`**


In questo caso si carica nel registro `$t0` la word che si trova in memoria all'indirizzo dato dal contenuto del registro `$s3` a cui va sommato 4.


## STORE

[[048 Formati delle istruzioni MIPS#FORMATO I|Formato I]].
La `sw` *salva* il contenuto di un registro in una locazione di memoria.
Bisogna specificare l'indirizzo della cella in cui la word è memorizzata.
Abbiamo un registro di *base* ed un *offset*.

**`sw $t0, 32($s3)`**

In questo caso si porta in memoria la word contenuta nel registro $t0 nella locazione di memoria il cui indirizzo è dato dal contenuto del registro $s3 a cui va sommato 32.

___
[[000 Indice AdE|↖ Ritorna all'indice ↖]]

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