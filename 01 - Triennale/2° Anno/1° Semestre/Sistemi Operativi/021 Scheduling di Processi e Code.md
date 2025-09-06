---
aliases: 
tags:
  - corsi/informatica/sistemi_operativi
paragrafo: Processi
cssclasses:
  - 
---
#### CODE DI SCHEDULING PER PROCESSI
Entrando nel sistema, ogni processo in [[019 Stati di un Processo#^610966|stato ready]] è inserito in una **Coda di Processi Pronti** (Ready Queue).

Questa coda viene memorizzata come una [[020 Liste nel C#RAPPRESENTAZIONE DI UNA LISTA|lista concatenata]], dove si conservano l'elemento head e tail.

Supponiamo che un processo effettui una richiesta di I/O su un disco. Poiché i dispositivi periferici sono molto più lenti dei processori, il processo dovrà attendere che l'I/O diventi disponibile.

I processi in attesa vengono collocati in una **Coda di Attesa**.

![[Scan2022-10-12_170910.jpg|500]]

## SCHEDULING DI PROCESSI
Nella gestione dei processi, gli **Schedulatori di processi** servono a selezionare i processi dalle varie code. Questa scelta avviene attraverso vari algoritmi.

![[Immagine 2022-10-12 173025 1.png|600]]

Gli schedulatori più importanti sono:
- **Schedulatore a lungo termine** *(job Scheduler)*: seleziona i processi dal disco che devono essere caricati in memoria centrale (ready queue). Questo avviene perché non tutti i processi possono stare contemporaneamente in memoria principale, altrimenti si consuma molto spazio.
- **Schedulatore a breve termine** *([[022 CPU Scheduler|CPU Scheduler]])*: seleziona il prossimo processo che la CPU dovrebbe eseguire.

Sistemi come Unix e Windows sono privi di schedulatore a lungo termine e si limitano a caricare in memoria tutti i nuovi processi e gestirli con lo scheduler a breve termine.


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