---
aliases:
  - Insieme delle Parti
  - Insieme Potenza
tags:
  - corsi/matematica/matematica_discreta
paragrafo: Teoria degli Insiemi
cssclasses:
  - 
---
>Un **Insieme delle Parti** (*insieme potenza*) di un [[001 Insieme|Insieme]] dato è un nuovo insieme che *contiene tutti i possibili [[002 Sottoinsiemi|sottoinsiemi]] dell'insieme originale*, inclusi l'[[001 Insieme#^2a175b|insieme vuoto]] e l'insieme stesso.

$$\color{#CC241D}\mathcal{P}(S)=\{X: X\subseteq S\}$$
Questi elementi non si devono ripetere.

Se $S$ è un insieme finito, allora:
$$\color{#61AFEF}|\mathcal{P}(S)|=2^{|S|}$$
> [!quote] Dimostrazione (pagina libro 7, 1.1.9)

> [!example]- <font color="orange">Esempio</font>
>- $\emptyset, |\emptyset| = 0, |\mathcal{P}(\emptyset)| = 2^0 = 1, P(\emptyset) = \{\emptyset \}$
>- $A = \{a\}, |A| = 1, |\mathcal{P}(A)| = 2^1 = 2, \mathcal{P}(A) = \{\emptyset, A\}$
>- $B = \{a, b\}, |B| = 2, |\mathcal{P}(B)| = 2^2 = 4, \mathcal{P}(B) = \{\emptyset, B, \{a\}, \{b\}\}$
>- $C = \{a, b, c\}, |C| = 3, |\mathcal{P}(B)| = 2^3 = 8, \mathcal{P}(C) = \{\emptyset, C, \{a\}, \{b\}, \{c\}, \{a, b\}, \{a, c\}, \{b, c\}\}$
>- $S \subseteq T \iff P(S) \subseteq P(T)$ 

___
[[000 Indice MD|↖ Ritorna all'indice ↖]]

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