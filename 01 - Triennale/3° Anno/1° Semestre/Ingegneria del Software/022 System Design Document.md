---
aliases:
  - SDD
tags:
  - corsi/informatica/ingegneria_software
paragrafo: Progettazione - System Design
cssclasses:
  - 
---
>Il **System Design Document (SDD)** contiene il risultato ottenuto dalle [[021 Attività del System Design|Attività del System Design]]. È una interfaccia tra gli sviluppatori e coloro che hanno deciso l'architettura del sistema.

È quindi una forma di comunicazione tra gli sviluppatori ed i progettisti ad alto livello.

1. La prima sezione è una *introduzione*, che serve per provvedere una breve panoramica dell'architettura del software e gli obbiettivi della progettazione. Fornisce anche un riferimento ad altri documenti (come il [[017 Requirement Analysis Document|RAD]] o documentazione di sistemi esistenti).

2. La seconda sezione riguarda *l'architettura software corrente*, che descrive l'architettura del sistema da sostituire. Se non esiste, questa sezione può essere rimpiazzata da una ricerca per un architettura per sistemi simili.

3. La terza sezione riguarda *l'architettura proposta per il sistema*, che descrive le sezioni del [[021 Attività del System Design|Modello di System Design]].

> [!tip] <font color="orange">Template</font>
>Pagina 284-285 del libro per spiegazioni complete.
>```
>1. Introduzione
>	1.1 Finalità del sistema
>	1.2 Obbiettivi della Progettazione
>	1.3 Definizioni, acronimi ed abbreviazioni
>	1.4 Riferimenti
>2. Architettura corrente del software
>3. Architettura proposta per il software
>	3.1 Panoramica
>	3.2 Decomposizione in sottosistemi
>	3.3 Mapping Hardware/software
>	3.4 Gestione dei Dati Persistenti
>	3.5 Controllo degli Accessi e Sicurezza
>	3.6 Controllo del Flusso del sistema
>	3.7 Condizioni al Boundary
>4. Subsystem services
>5. Glossario
>```

> [!warning] Non devono essere presenti le tecnologie utilizzate (tranne il [[DBMS]] nel [[UML 8 - Diagramma di Deployment|Deployment Diagram]])



___
[[000 Indice IS|↖ Ritorna all'indice ↖]]

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