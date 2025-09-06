---
aliases: 
tags:
  - corsi/informatica/architettura_degli_elaboratori
paragrafo: Rappresentazione Numeri - Con Virgola
cssclasses:
  - 
---
> [!note] Con virgola

La rappresentazione in virgola fissa usa un numero (*n*,**s**), dove *n* è la parte intera, ed **s** è la parte frazionaria.

Fornisce una rappresentazione approssimata del numero dato, limitando la rappresentazione di numeri molto grandi o molto piccoli.

In base al sistema posizionale pesato, un numero con la virgola si rappresenta in questo modo:
$$N=(\textcolor{#61AFEF}{a_{n-1}a_{n-2}...a_1a_0}, \textcolor{red}{a_{-1}a{-2}...a{-s}})$$

## CONVERSIONE

### BINARIO → DECIMALE
In generale:
$$N= \textcolor{#61AFEF} {a_{n-1}\times 2^{n-1}+...+a_0\times 2^0 } + \textcolor{red} {a_{-1}\times 2^{-1}+...+a_{-s}\times 2^{-s}}$$ $$=$$ $$\sum^{n-1}_{i=-s}a_i\times 2^i$$

Dove:
$$b^{-n} = \frac{1}{b^n}$$

> [!example]- <font color="orange">Esempio</font>
$(11011,100001)_2 =$ *$2^4+2^3+2^1+2^0$* $+$ **$2^{-1}+2^{-6}$** $= 16+8+2+1+0.5+0.015625 = 27.515625$

### DECIMALE → BINARIO
Si moltiplica la **parte frazionaria** per la base:
$$N=F_0$$
$$2\times F_0 = \textcolor{red}{a_{-1}}+F_{-1}$$
$$2\times F_1 = \textcolor{red}{a_{-2}}+F_{-2}$$
$$...$$
$$2\times F_{-i} = \textcolor{red}{a_{-(i+1)}}+F_{-(i+1)}$$

Per trovare il numero binario si prendono gli *0* e gli *1* a **partendo da sopra ad arrivare alla fine**.

La conversione finisce quando troviamo un valore di F-1 uguale a zero, oppure quando abbiamo finito il numero di bit a disposizione per la rappresentazione binaria.

> [!example]- <font color="orange">Esempio</font>
(0,234)<sub>10</sub>
$2\times 0.234 = \textcolor{red}{0} + 0.468$
$2\times 0.468 = \textcolor{red}{0} + 0.936$
$2\times 0.936 = \textcolor{red}{1} + 0.872$
$2\times 0.872 = \textcolor{red}{1} + 0.744$
$2\times 0.744 = \textcolor{red}{1} + 0.488$
$2\times 0.488 = \textcolor{red}{0} + 0.976$
$2\times 0.976 = \textcolor{red}{1} + 0.952$
$...$
(0,234)<sub>10</sub> = (0,0011101...)<sub>2</sub>


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