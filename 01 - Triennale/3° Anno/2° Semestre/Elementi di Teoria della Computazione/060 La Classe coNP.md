---
aliases: 
tags:
  - corsi/informatica/elementi_teoria_computazione
paragrafo: Verificabilità in Tempo Polinomiale
cssclasses:
  - 
---
>Un linguaggio $L$ fa parte della **Classe $coNP$** se e solo se il suo complemento $\overline{L}$ si trova nella [[058 La Classe NP|classe NP]]. Ovvero che 
>$$coNP=\{L\ |\ \overline{L}\in NP\}$$

Quindi è la classe dei problemi per i quali, invece di essere facile verificare la risposta, è *facile escludere quelle sbagliate* che potrebbero essere o meno le stesse di $NP$.

Esempi di linguaggi in $coNP$ sono:
- $\overline{HAMPATH}$, complemento di [[058 La Classe NP#HAMPATH|HAMPATH]]
- $\overline{CLIQUE}$, complemento di [[058 La Classe NP#CLIQUE|CLIQUE]]
- $\overline{SUBSET\text{-}SUM}$, complemento di [[058 La Classe NP#SUBSET-SUM|SUBSET-SUM]]

Per ognuno di questi linguaggi *non è noto* se tale linguaggio appartenga o meno a $NP$. Infatti, verificare che "qualcosa non c'è" sembra essere più difficile
che verificare che "c'è".

### NP e coNP
Un altro problema aperto è quello di capire se $$\color{#B35DB0}NP=coNP\ ?$$
Insieme alla domanda $\color{#B35DB0}P=NP\ ?$, abbiamo i seguenti quattro possibili scenari.

![[Pasted image 20240601173230.png]]

In ogni caso, si ha che $$P\subseteq coNP$$

> [!note]+ Nota
> Un linguaggio che non appartiene a NP e a coNP è un linguaggio [[042 Linguaggi Indecidibili|non decidibile]].

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