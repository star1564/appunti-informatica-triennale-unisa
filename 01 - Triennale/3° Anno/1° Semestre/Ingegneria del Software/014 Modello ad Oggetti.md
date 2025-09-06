---
aliases:
  - Analysis Object Models
  - oggetti Entity (IS)
  - oggetti Boundary (IS)
  - oggetti Control (IS)
tags:
  - corsi/informatica/ingegneria_software
paragrafo: Analisi dei Requisiti - Analisi
cssclasses:
  - 
---
>Il **Modello ad Oggetti** fa parte del [[013 Analisi dei Requisiti#Modello di Analisi|Modello di Analisi]] e si concentra sui concetti (con le loro proprietà e relazioni) che sono *manipolati dal sistema*.
>Questo modello viene rappresentato da [[UML 5 - Diagramma di Classi|diagrammi di classe]] e di [[UML 2 - Diagramma di Sequenza|sequenza]].

È costituito da [[Classi]], [[011 Variabile Esemplare|Attributi]], [[Metodi]] e Relazioni tra le Classi.
## Entity, Boundary e Control object
Il Modello ad Oggetti è costituito da diversi tipi di oggetti:
- Gli **Oggetti Entity** rappresentano *informazioni persistenti* tracciate dal sistema (come le [[034 Entità JPA|Entità in JPA]]).
 ^7cb8de
- Gli **Oggetti Boundary** rappresentano le *interazioni tra gli attori ed il sistema*, molto spesso è una parte dell'interfaccia utente (come un pulsante su un sito web).
 ^fc2909
- Gli **Oggetti Control** rappresentano il *controllo delle operazioni* eseguite dal sistema (come una classe che permette di effettuare una operazione), contengono la logica dell'operazione e *coordinano gli oggetti entity e boundary*. ^e6d6d5

Questo tipo di divisione, permette la creazione di oggetti più *piccoli e specializzati* e permettono al modello di essere *resistente ai cambiamenti* (infatti se si separa l'interfaccia da altre funzionalità, si dovrà solamente modificare una delle due).

![[Pasted image 20240118141725.png|600]]


Per distinguere il tipo di oggetto, si usano gli [[005 UML#Stereotipi|stereotipi di UML]] `<<entity>>`, `<<control>>`, `<<boundary>>`.

![[Pasted image 20240118123759.png|900]]


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