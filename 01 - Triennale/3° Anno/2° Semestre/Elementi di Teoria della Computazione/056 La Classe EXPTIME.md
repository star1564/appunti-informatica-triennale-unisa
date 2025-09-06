---
aliases: 
tags:
  - corsi/informatica/elementi_teoria_computazione
paragrafo: Complessità di Tempo Polinomiale
cssclasses:
  - 
---
>La **Classe $EXPTIME$** (o $EXP$) è l'insieme dei linguaggi $L$ per i quali esiste una Macchina di Turing deterministica $M$ con un solo nastro che [[024 Macchine Decisionali|decide]] $L$ in *[[054 Tempo Polinomiale ed Esponenziale|tempo esponenziale]]* $O({2}^{n^k})$ per qualche $k\geq 1$ con $k\in\mathbb{N}$, ovvero $$EXPTIME=\bigcup_{k\geq1}TIME({2}^{n^k})$$

Alla classe $EXPTIME$ corrisponde la classificazione dei problemi di decisione algoritmicamente risolvibili in *problemi intrattabili*, ovvero problemi per i quali esistono algoritmi esponenziali per decidere i linguaggi associati.

> [!note] $P\subsetneqq EXPTIME$
> Si ha che $P\subseteq EXPTIME$. Questo perché ogni problema che può essere risolto in tempo polinomiale **può anche essere risolto in tempo esponenziale** tramite un altro algoritmo. Infatti, un algoritmo di tempo polinomiale è un tipo particolare di algoritmo di tempo esponenziale, dove l'esponente è $\leq 1$.
> 
> ![[Pasted image 20240601153527.png|350]]
> 
> Inoltre esistono linguaggi in $\color{#CC241D}EXPTIME-P$ a cui corrispondono problemi che richiedono, per essere risolti, tempo *sicuramente esponenziale* nella dimensione dei loro dati, con esponente $>1$. Quindi [^1]   $$\color{#CC241D}P\subsetneqq EXPTIME$$





[^1]: [Subset of above not equal to](https://math.stackexchange.com/questions/1038233/subset-of-above-not-equal-to-subsetneqq-symbol)




___
[[000 Indice ETC|↖ Ritorna all'indice ↖]]

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