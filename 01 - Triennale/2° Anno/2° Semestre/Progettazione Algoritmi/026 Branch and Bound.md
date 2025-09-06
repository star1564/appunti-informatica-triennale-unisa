---
aliases: 
tags:
  - corsi/informatica/progettazione_algoritmi
paragrafo: Alberi e Grafi
cssclasses:
  - 
---
>La tecnica **Branch and Bound** è un'algoritmo utilizzato per risolvere problemi di ottimizzazione, in cui *si cerca di trovare la migliore soluzione tra un insieme di soluzioni possibili*. L'obiettivo della tecnica Branch and Bound è quello di ridurre efficacemente lo spazio di ricerca, *evitando l'esplorazione* di soluzioni che sono sicuramente peggiori della migliore soluzione già trovata.

L'algoritmo Branch and Bound utilizza una strategia di ricerca ad [[013 Alberi|albero]]. Inizialmente, viene creata una radice che rappresenta il problema originale. Successivamente, l'algoritmo *genera iterativamente le soluzioni parziali*, creando nuovi rami dell'albero per esplorare le possibili scelte. In ogni nodo dell'albero, viene calcolato un **limite superiore (bound)** per valutare se un ramo può portare a una soluzione migliore della soluzione ottimale finora trovata.

L'idea principale dell'algoritmo Branch and Bound è quella di *escludere un ramo dall'esplorazione se il suo limite superiore/inferiore è peggiore del valore della soluzione ottimale corrente*. In questo modo, si riduce lo spazio di ricerca, evitando di esplorare percorsi che non possono portare a soluzioni migliori.

Durante la ricerca, l'algoritmo mantiene il valore della soluzione ottimale corrente e aggiorna tale valore quando viene trovata una soluzione migliore. Inoltre, tiene traccia del limite superiore corrente e utilizza questa informazione per decidere quali rami esplorare o escludere.

Viene usato solo per problemi di *ottimizzazione e minimizzazione*.

> [!summary] Algoritmo
>```C
>BranchBound(i){ // Per problemi di minimizzazione si usa il LowerBound
>	if(i < n){ // i è l'indice del nodo corrente
>		determina i nodi figli di i;
>		for(ogni nodo figlio j){
>			s[i+1] = j; // la soluzione parziale nota è s[1, ..., i]
>			if(LowerBound(s/*[1, ..., i]*/) < MINCOST) {
>				BranchBound(i+1);
>			}
>		}
>	} else {
>		if(cost(s/*[1, ..., n]*/) < MINCOST){
>			MINSOL = s/*[1, ..., n]*/ // MINSOL è la soluzione ottima
>			MINCOST = cost(s/*[1, ..., n]*/) // MINCOST è il valore ottimo
>		}
>	}
>}
>```


___
[[000 Indice PA|↖ Ritorna all'indice ↖]]

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
- [Abdul Bari](https://www.youtube.com/watch?v=3RBNPc0_Q6g)