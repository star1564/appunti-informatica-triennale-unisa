---
aliases:
  - Assioma
  - Postulato
  - Teorema
  - Corollario
tags:
  - corsi/matematica/analisi
paragrafo: Nozioni di base
cssclasses:
  - 
---
## Assioma (Postulato)
Un **assioma** (postulato) è una *proposizione matematica che si considera vera senza essere dimostrata*. Gli assiomi sono il fondamento di qualsiasi teoria e devono essere tra loro indipendenti, non contraddittori e in numero finito.

## Teorema
Un **teorema** è un costrutto matematico che viene espresso da enunciati, dimostrazioni, ipotesi e tesi.

Un qualsiasi teorema è formato da due parti:
- L'*enunciato*: ossia la proposizione che esprime l'implicazione logica;
- La *dimostrazione*: ossia il processo deduttivo che permette di ricavare il valore di verità dell'enunciato.

A sua volta, l'enunciato di un teorema è costruito da ipotesi e tesi:

- Le *ipotesi* sono le condizioni e/o le proprietà su cui si fonda il ragionamento, e sono assunte come vere; ^f5398f
- La *tesi* è la condizione e/o proprietà che discende dalle ipotesi. ^0978e4

Indicando con $I$ le ipotesi e con $T$ la tesi, [[002 Dimostrare Teoremi|per dimostrare]] che un teorema si deve provare che vale l'[[006 Implicazione|implicazione]]:
$$I\Rightarrow T$$
Quando un teorema è stato dimostrato, si dice che $I$ è condizione sufficiente per il verificarsi di $T$ e che $T$ è condizione necessaria per il verificarsi di $I$.

## Corollario
Un **corollario** *è un teorema che è conseguenza immediata di un teorema fondamentale* e si ottiene come caso particolare dello stesso, ossia riducendo o particolarizzando le ipotesi del teorema da cui si discende.

A volte può succedere che da un teorema ne seguano altri, le cui dimostrazioni sono talmente immediate da essere omesse o solamente accennate: questi teoremi sono detti corollari. Ogni corollario è quindi associato a un proprio teorema di riferimento.

## Uguaglianza come definizione
Il simbolo $\color{#CC241D}:=$ in una formula indica che l'uguaglianza per definizione.

___
[[000 Indice Analisi|↖ Ritorna all'indice ↖]]

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
