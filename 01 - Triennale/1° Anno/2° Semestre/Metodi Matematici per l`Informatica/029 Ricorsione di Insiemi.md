---
aliases: 
tags:
  - corsi/matematica/metodi_matematici_informatica
paragrafo: Ricorsione
cssclasses:
  - 
---
La *definizione ricorsiva* di un insieme consiste di un passo base e di un passo ricorsivo. 

> [!quote] Definizione Ricorsiva di un Insieme
> - **Passo Base**: Definisce uno o più *oggetti elementari*.
> - **Passo Ricorsivo**: Definisce la regola che permette di *costruire oggetti più complessi* in termini di quelli già definiti dell'insieme.


> [!summary] Come rappresentare ricorsivamente un Insieme
> 1. Elencare gli elementi dell'insieme
> 2. Trovare l'elemento più piccolo dell'insieme
> 3. Capire come ottenere via via gli elementi


> [!example]- <font color="orange">Esempio</font>
> Sia $A$ l'insieme ricorsivamente definito come segue.
> - **Passo Base**: $1\in A$
> - **Passo Ricorsivo**: Se $x\in A$, allora $x+2\in A$
>
> Trovare e descrivere l'insieme.
>
> - $1\in A$ (passo base)
> - Applico il passo ricorsivo: $x=1\in A$, allora $1+2=3\in A$
>$[$Insieme attuale: $\{1,3\}$$]$
> - Applico il passo ricorsivo: $x=3\in A$, allora $3+2=5\in A$
>$[$Insieme attuale: $\{1,3, 5\}$$]$
> - $...$
> - $1, 3, 5, 7, 9, ... \in A$
 >
>Possiamo notare quindi che $A$ è l'insieme dei numeri dispari $\geq 0$
>Ovvero $\color{#61AFEF}A=\{n\in\mathbb{N}\ |\ \exists k\geq0: n=2k+1\}$



___
[[000 Indice MMI|↖ Ritorna all'indice ↖]]

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

