---
aliases:
tags:
  - corsi/matematica/probabilità_statistica
paragrafo: Eventi
cssclasses:
  - 
---
>Le **operazioni tra eventi** sono le stesse che vengono utilizzate in [[001 Insieme|Insiemistica]]. Permettono di individuare [[011 Evento|eventi]] di uno [[010 Spazio Campionario|spazio campionario]] a partire da altri suoi eventi. Queste operazioni sono: 
>- *[[008 Unione tra Insiemi|unione]]*
>- *[[009 Intersezione tra Insiemi|intersezione]]*
>- *[[010 Differenza tra Insiemi|differenza]]*
>- *[[013 Complemento di un Insieme|complemento]]*

> [!success]+ KEYWORD
> - "e" -> [[Congiunzione - AND]] -> $A\cap B$
> - "o", "oppure" -> [[Disgiunzione - OR]] -> $A\cup B$

> [!example]- <font color="orange">Esempio</font>
>Nell'esperimento del lancio di 2 monete, con Spazio Campionario $S=\{cc,ct,tc,tt\}$, se
>- $A=\{cc,ct\}=\{\text{croce al primo lancio}\}$;
>- $B=\{cc,tt\}=\{\text{nei due lanci si ha lo stesso risultato}\}$.
>
>Abbiamo che: 
>- $A\cup B=\{cc,ct,tt\}$
>- $A\cap B=\{cc\}$ 
>- $\overline{A}=\{tc,tt\}, \overline{B}=\{ct,tc\}$

L'Unione e l'Intersezione totale tra più eventi è definita come: $$\bigcup^\infty_{n=1}A_n=A_1\cup A_2\cup\dots A_{n},\quad\quad\quad\bigcap^\infty_{n=1}A_n=A_1\cap A_2\cap\dots A_{n}$$

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
- [YouMath](https://www.youmath.it/lezioni/probabilita/probabilita-discreta/5171-evento-unione-evento-intersezione-evento-differenza-evento-complementare.html)