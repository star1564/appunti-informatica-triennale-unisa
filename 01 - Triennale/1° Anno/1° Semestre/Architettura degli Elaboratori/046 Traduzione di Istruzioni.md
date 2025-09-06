---
aliases: 
tags:
  - corsi/informatica/architettura_degli_elaboratori
paragrafo: MIPS - Assembly
cssclasses:
  - 
---
>Per impartire istruzioni a un calcolatore è necessario parlare il suo linguaggio, composto da un **insieme d'istruzioni** (*Instruction Set*, *IS*) corrispondenti a *sequenze binarie*.

Queste istruzioni vengono date dal [[047 Assembly nel MIPS|linguaggio assembly]].
Questo è più leggibile del linguaggio macchina, perché usa simboli invece di sequenze binarie.

Ma, per un umano, è molto più semplice usare *linguaggi ad alto livello* per scrivere programmi (come il C).

## DA LINGUAGGIO AD ALTO LIVELLO ALL'ESECUTIBILE

Vedremo il processo per passare da un applicazione scritta in un linguaggio ad alto livello alla sua concreta esecuzione.
Avremo bisogno di diversi moduli.

### COMPILATORE

Il **compilatore** è il software che si occupa della *traduzione* di un programma da *linguaggio ad alto livello* a *linguaggio assembly*.

![[Immagine 2021-11-10 153036.jpg]]

### ASSEMBLATORE
L'**assemblatore** è il software che si occupa della traduzione di un programma da *linguaggio assembly* a *linguaggio macchina*.

![[Pasted image 20211110153652.png]]

Un assemblatore legge un singolo *file sorgente* in linguaggio assembly e produce un *file oggetto* contenente le istruzioni in linguaggio macchina.

### LINKER
Il **linker** ha il compito di *collegare diversi file* (come le librerie) in un solo file eseguibile.

![[Pasted image 20211110154023.png]]

### LOADER
Il **loader** ha il compito di individuare un'area libera nella memoria principale in cui caricare il programma per poi lanciarne l'esecuzione.

### PROCESSO COMPLETO
![[Immagine 2021-11-10 154543.jpg]]


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