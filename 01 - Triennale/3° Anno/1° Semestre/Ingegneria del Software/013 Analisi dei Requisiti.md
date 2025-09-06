---
aliases:
  - Requirements Analysis
  - Modello di Analisi
tags:
  - corsi/informatica/ingegneria_software
paragrafo: Analisi dei Requisiti - Analisi
cssclasses:
  - 
---
>L'**Analisi dei Requisiti** (*Requirements Analysis*) ha come ha come obbiettivo principale la costruzione di un modello per il sistema, chiamato **Modello di Analisi**, che sia corretto, completo, consistente, privo di ambiguità e che rappresenti il [[002 Sistemi, Modelli e Viste#^ae7aa9|Dominio di Applicazione]].

> [!info] Input ed output della fase
> Questa attività comincia da una [[012 Specifica dei Requisiti|Specifica dei Requisiti]] e produce un [[017 Requirement Analysis Document|Requirement Analysis Document]].

In questa attività, gli sviluppatori traducono la specifica dei requisiti, prodotta durante l'[[009 Estrazione dei Requisiti|estrazione dei requisiti]], in un [[002 Sistemi, Modelli e Viste#Modello|Modello]] con un *linguaggio formale o semi-formale*.

![[Pasted image 20240121111420.png|800]]

### Modello di Analisi
Il Modello di Analisi descrive le [[014 Modello ad Oggetti#Entity, Boundary e Control object|entità, boundary e oggetti di controllo]] visibili agli utenti.

È composto da tre modelli:
- **[[012 Specifica dei Requisiti#Modello Funzionale (Modello dei Casi d'Uso)|Modello Funzionale]]**, rappresentato da [[011 Scenari e Casi d_Uso|casi d'uso e scenari]];
- **[[014 Modello ad Oggetti|Modello ad Oggetti]]**, rappresentato da [[UML 5 - Diagramma di Classi|diagrammi di classe]] e di [[UML 2 - Diagramma di Sequenza|sequenza]];
- **[[016 Modello Dinamico|Modello Dinamico]]**, rappresentato da [[UML 3 - Diagramma di Stato|diagrammi di stato]].

![[Pasted image 20240118130529.png|700]]


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