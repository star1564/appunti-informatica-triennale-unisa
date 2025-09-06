---
aliases: 
tags:
  - corsi/informatica/basi_di_dati
paragrafo: Modello Relazionale
cssclasses:
  - 
---
L'insieme principali delle operazioni per il [[010 Modello Relazionale|modello relazionale]] è l'**Algebra Relazionale**. Le operazioni di quest'algebra sono alla base di linguaggi di programmazione come l'[[023 Definizioni SQL|SQL]] e consentono all'utente di specificare le [[Query|interrogazioni]] fondamentali in termini di *espressioni* e più in generale di *manipolare intere relazioni*.

È possibile suddividere le sue operazioni in due gruppi:
- **Operazioni sugli insiemi**: ogni [[010 Modello Relazionale#^tabelle|relazione (o tabella)]] è vista come un insieme, quindi si possono applicare le stesse operazioni matematiche:
	- [[017 Operazioni Insiemistiche per DB#UNIONE|UNION]], (corrisponde all'[[008 Unione tra Insiemi|unione di insiemi]] in matematica);
	- [[017 Operazioni Insiemistiche per DB#INTERSEZIONE|INTERSECTION]], (corrisponde all'[[009 Intersezione tra Insiemi|intersezione di insiemi]] in matematica);
	- [[017 Operazioni Insiemistiche per DB#DIFFERENZA|SET DIFFERENCE]], (corrisponde all'[[010 Differenza tra Insiemi|differenza di insiemi]] in matematica);
	- [[017 Operazioni Insiemistiche per DB#PRODOTTO CARTESIANO|CARTESIAN PRODUCT]], (corrisponde all'[[012 Prodotto Cartesiano tra Insiemi|prodotto cartesiano di insiemi]] in matematica).
- **Operazioni specificatamente disegnate per database relazionali**:
	- [[015 Operazioni per i DB Relazionali#SELECT|SELECT]];
	- [[015 Operazioni per i DB Relazionali#PROJECT|PROJECT]];
	- [[018 Operazioni di Join|JOIN]].
- [[016 Altri tipi di operazioni relazionali|Rename e Sequenze di Operazioni]].

___
[[000 Indice BD|↖ Ritorna all'indice ↖]]

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