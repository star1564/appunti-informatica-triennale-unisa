---
aliases: 
tags:
  - corsi/matematica/analisi
paragrafo: Funzioni
cssclasses:
  - 
---
## Funzione Pari
>Una [[011 Funzioni|Funzione]] si dice **Pari** se assume valori *simmetrici rispetto all'asse delle ordinate ($y$)*, ovvero
>$$\color{#CC241D}f(-x)=f(x)$$

> [!example]+ <font color="orange">Esempio</font>
>La [[F02 Funzione Potenza con Esponente Pari|Funzione Potenza con Esponente Pari]] è una Funzione Pari, perché dal grafico si vede che la parte a destra dell'asse delle $y$ è riflessa nella parte a sinistra e viceversa.
>![[Pasted image 20240927142527.png|500]]
>---
>La funzione $f(x) = x^4+3x^2$ è pari perché:
>
>$$f(-x)=(-x)^4+3(-x)^2=x^4+3x^2=f(x)$$
>
>Quindi è vero che $f(-x)=f(x)$.



## Funzione Dispari
>Una [[011 Funzioni|Funzione]] si dice **Dispari** se assume valori *simmetrici rispetto all'asse delle ascisse ($x$)*, ovvero
$$\color{#CC241D}f(-x)=-f(x)$$

> [!example]+ <font color="orange">Esempio</font>
>La [[F03 Funzione Potenza con Esponente Dispari|Funzione Potenza con Esponente Dispari]] è una Funzione Dispari, perché dal grafico si vede che la parte sopra l'asse delle $x$ è riflessa nella parte di sotto e viceversa.
>![[Pasted image 20240927143405.png|500]]
>---
>La funzione $f(x) = 2x^7+3x^5+4x$ è dispari perché:
>
>$$f(-x)=2(-x^7)+3(-x^5)+4(-x)=-2x^7-3x^5-4x= -(2x^7+3x^5+4x) = -f(x)$$
>
>Quindi è vero che $f(-x)=-f(x)$.



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
- [YouMath](https://www.youmath.it/lezioni/analisi-matematica/le-funzioni-da-r-a-r-in-generale/22-parita-e-disparita-di-una-funzione-da-r-a-r.html)