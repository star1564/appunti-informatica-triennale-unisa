---
aliases: 
tags:
  - corsi/informatica/architettura_degli_elaboratori
paragrafo: Rappresentazione Numeri - Interi
cssclasses:
  - 
---
> [!note] Interi

## OTTALE → BINARIO

Ad ogni cifra in ottale si fa corrispondere la **rappresentazione binaria su 3 bit**. In 3 bit perché ci troviamo in base otto, pertanto possiamo solo rappresentare il numero 8-1 (7) con un massimo di 3 bit.

> [!example]- <font color="orange">Esempio</font>
>$(213)_8$
>$2 = 010$
>$1 = 001$ 
>$3 = 011$
>$(213)_8=(010\>\>001\>\>011)_2$
>![[Immagine 2021-10-17 205258.jpg|150]]

## BINARIO → OTTALE

Si raggruppano i bit 3 a 3 **partendo da destra**, ed ad ogni gruppo si fa corrispondere la cifra **in ottale** (da 0 a 7). In 3 bit perché ci troviamo in base otto, pertanto possiamo solo rappresentare il numero 8-1 (7) con un massimo di 3 bit.

> [!example]- <font color="orange">Esempio</font>
>$(1\>\>010\>\>001\>\>011)_2$
>$011 = 3$
>$001 = 1$ 
>$010 = 2$
>$1 = 1$
>$(1\>\>010\>\>001\>\>011)_2 = (1213)_8$
>![[3d60d305-b7e2-49ba-9a5c-c32b02204165.png|150]]

___
[[000 Indice AdE|↖ Ritorna all'indice ↖]]

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