---
aliases: 
tags:
  - corsi/informatica/machine_learning
paragrafo: Training Data
cssclasses:
  - 
---
> **Data Augmentation** è un insieme di tecniche che permette di *aumentare il numero di dati* di addestramento quando *non ne abbiamo a disposizione grandi quantità*.

La maggior parte di queste tecniche lavora partendo dal formato dei dati a disposizione. Queste sono:
- *Simple Label-Preserving Transformations*;
- *Perturbation*;
- *Data Synthesis*.

## Simple Label-Preserving Transformations
>Nella **Simple Label-Preserving Transformations** si *modifica* randomicamente un'immagine (o una frase) *preservando la sua etichetta*.

![[Pasted image 20231215213755.png]]

## Perturbation
>Nella **Perturbation** si *aggiungono dati rumorosi* randomici a quelli già disponibili, andando così a migliorare il decision boundary di un modello, facendo attenzione al suo *calo di prestazione*. 

Esistono anche attacchi verso i modelli che prevedono l'utlizzo di dati rumorosi per confonderli.

![[Pasted image 20231215214102.png|600]]


### Adversarial Augmentation
>L'**Adversarial Augmentation** è un tipo particolare di Perturbation e consiste nel *rilevare il minimo possibile noise injection necessario* per causare una *classificazione errata con alta confidenza*.

![[Pasted image 20231215214238.png|700]]

## Data Synthesis
>L'approccio del **Data Synthesis** sfrutta la *creazione di nuovi dati sintetici* a partire da un template. Questa tecnica può aumentare la capacità di generalizzazione del modello e la robustezza, riducendo problemi legati ad etichette corrotte.


___
[[000 Indice ML|↖ Ritorna all'indice ↖]]

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

<center><font color="rainbow"><i>My vision is augmented.</i></font></center>

Altri collegamenti: 