---
aliases: 
tags:
  - corsi/informatica/programmazione_strutture_dati
paragrafo: Algoritmi di Ordinamento
cssclasses:
  - 
---
Se vogliamo ordinare un array secondo un parametro (esempio record degli studenti secondo la loro data di nascita) dobbiamo ordinarlo in base ad una chiave, che può essere un singolo campo o la combinazione di più campi.

### ALGORITMI
Tutti gli algoritmi si basano sui confronti.
- *Algoritmi semplici*. Numero di operazioni quadratico rispetto alla taglia dell'input: O(n<sup>2</sup>):
	- [[011 Selection Sort|Selection Sort]];
	- [[012 Insertion Sort|Insertion Sort]];
	- [[013 Bubble Sort|Bubble Sort]];
- *Algoritmi avanzati*. Sono più efficaci.
	- [[014 Merge Sort|Merge Sort]], numero di operazioni rispetto alla taglia dell'input: [[030 Complessità Computazionale|O(n log n)]];
	- [[015 Quick Sort|Quicksort]], dove si può avere O(n log n) nel caso medio, oppure quadratico nel caso peggiore.

![[ee5564f2-e015-4a79-962d-d3c24b6e6d73.jpg]]
<center>Complessità Computazionale degli algoritmi</center>

### PROPRIETÀ ALGORITMI DI ORDINAMENTO
Gli algoritmi possono avere le seguenti proprietà:

- **Stabile**: se due elementi hanno la stessa chiave, mantengono lo stesso ordine con cui si presentavano prima dell'utilizzo;
- **In Loco**: se richiede un numero fisso di variabili o altri array per l'ordinamento;
- **Adattivo**: se il numero di operazioni effettuate cambia in base all'input;
- **Interno o Esterno**
	- *Interno*: i dati sono contenti nella RAM;
	- *Esterno*: i dati sono contenuti su disco.

___
[[000 Indice PSD|↖ Ritorna all'indice ↖]]

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