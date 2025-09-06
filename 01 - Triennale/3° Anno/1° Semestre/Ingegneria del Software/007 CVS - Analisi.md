---
aliases: 
tags:
  - corsi/informatica/ingegneria_software
paragrafo: Analisi dei Requisiti
cssclasses:
  - 
---
>La seconda fase del [[003 Ciclo di Vita del Software|CVS]], l'**Analisi**, cerca di *identificare e registrare i requisiti* precisi degli utenti finali. 

![[Pasted image 20240121105529.png|900]]

In questa fase, il team cerca di rispondere alla domanda:

> [!quote] ‎ %%← Empty Char%%*Quali sono le aspettative dei nostri utenti dal nostro software?*

## Ingegneria dei Requisiti
>L'attività dell'**Ingegneria dei Requisiti** ha lo scopo di *definire i [[008 Requisiti|Requisiti]]* del sistema in costruzione.

Si divide in:
- **[[009 Estrazione dei Requisiti|Estrazione dei requisiti]]**: una specifica del sistema *comprensibile al cliente* scritta in *linguaggio naturale*.
	- Parte dal [[010 Problem Statement|Problem Statement]];
	- Produce il [[012 Specifica dei Requisiti#Modello Funzionale (Modello dei Casi d'Uso)|Modello Funzionale]];
	- Produce la ***[[012 Specifica dei Requisiti|Specifica dei Requisiti]]***.
- **[[013 Analisi dei Requisiti|Analisi dei Requisiti]]**: un modello di analisi che gli *sviluppatori possono interpretare* senza ambiguità espresso in una *notazione formale o semi-formale*.
	- Parte dalla <i>Specifica dei Requisiti</i>;
	- Produce il [[016 Modello Dinamico|Modello Dinamico]] ed il [[014 Modello ad Oggetti|Modello ad Oggetti]];
	- Produce il ***[[017 Requirement Analysis Document|Requirement Analysis Document (RAD)]]***.

![[Pasted image 20240121111309.png|800]]

Entrambe le attività si focalizzano sul punto di vista dell'utente e *definiscono il confine del sistema* da sviluppare.

> [!example] <font color="orange">Per visualizzare un esempio dell'intero processo di analisi, consiglio sul [[000 Indice IS|libro]] di leggere "4.6 ARENA Case Study" (pag. 153)</font>

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