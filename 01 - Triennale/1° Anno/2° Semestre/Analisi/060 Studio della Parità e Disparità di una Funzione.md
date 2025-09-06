---
aliases: 
tags:
  - corsi/matematica/analisi
paragrafo: Studio di Funzione
cssclasses:
  - 
---

> [!info] Nota (2/7) sullo [[058 Studio di una Funzione|Studio di una Funzione]]
> [[059 Studio del Dominio di una Funzione|<- Passo precedente]] | [[061 Studio dell_Intersezione degli assi di una Funzione|Prossimo passo ->]]

Capire se la funzione $y=f(x)$ è [[013 Funzione Pari e Dispari|pari, dispari o nessuna delle due]] è molto utile.

Si vede prima di tutto se il [[059 Studio del Dominio di una Funzione|dominio]] della funzione è **simmetrico** rispetto al valore $x=0$.
Senza simmetria nel dominio non può infatti esservi alcuna simmetria per il grafico della funzione.

> [!example]+ <font color="orange">Esempio</font>
>$[-2, 2], (-\infty, -1]\ \cup\ [1,+\infty)$ sono simmetrici.
>$[-5, 4], (-\infty, -1)\ \cup\ [3,10)$ NON sono simmetrici.

1. Se il dominio *non è simmetrico*, allora la funzione **non è certamente pari né dispari**, si prosegue quindi nello studio della funzione.

2. Se invece il dominio *è simmetrico*, bisogna considerare l'espressione analitica $\color{#61AFEF}y=f(x)$ ed effettuare le seguenti valutazioni:
	- Se $f(-x)=f(x)$ allora la funzione è [[013 Funzione Pari e Dispari#Funzione Pari|pari]]
	- Se $f(-x)=-f(x)$ allora la funzione è [[013 Funzione Pari e Dispari#Funzione Dispari|dispari]]
	- In qualsiasi altro caso la funzione non è pari né dispari

Se una funzione è pari o dispari, si ci può *limitare a studiare la funzione sulla parte del dominio* contenuta in $[0, +\infty)$, il che semplifica molto le cose.


> [!example]+ <font color="orange">Esempio</font>
> $$y=\frac{ln(x+1)}{-2-ln(x+1)}$$
> Osserviamo il dominio della funzione è $$Dom(f)=(-1,\ e^{-2}-1)\ \cup\ (e^{-2}-1,\ +\infty)$$
> Questo non è simmetrico, quindi <font color="orange">non è pari né dispari</font>.



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
- [YouMath](https://www.youmath.it/lezioni/analisi-matematica/studio-di-funzioni-grafico/178-studio-di-paritadisparita.html)