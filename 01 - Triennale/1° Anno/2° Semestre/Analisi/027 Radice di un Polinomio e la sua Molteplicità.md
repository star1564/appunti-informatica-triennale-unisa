---
aliases: 
tags:
  - corsi/matematica/analisi
paragrafo: Numeri Complessi
cssclasses:
  - 
---
## Radice Di Un Polinomio
>Dato un polinomio $p(x)$ a coefficienti [[003 Insieme dei numeri Reali|reali]] o [[022 Numeri Complessi|complessi]], si dice che $a\in\mathbb{C}$ è una sua **Radice** (o un suo zero) se la valutazione del polinomio in $x=a$ è *nulla*.
>$$a\text{ radice di } p(x)\iff p(a)=0$$

> [!example]+ <font color="orange">Esempio</font>
>$a=2$ è una radice del polinomio $p(x)=x^3-5x^2+11x-10$.
>
>Infatti: $p(2)=2^3-5\cdot(2^2)+11\cdot2-10=8-20+22-10=0$

## Molteplicità di una Radice
>Dato un polinomio $p(x)$ a coefficienti reali o complessi e $n\geq1$ un numero naturale, si dice che $a$ è una radice di $p(x)$ con **Molteplicità** $n$ se e solo se $p(x)$ è *divisibile per $(x-a)^{n}$ ma non è divisibile per $(x-a)^{n+1}$*.

> [!example]- <font color="orange">Esempio</font>
>$a=0$ è una radice con molteplicità 3 del polinomio $p(x)=x^5-x^3$.
>
>Per rendersene conto basta scrivere
>$p(x)=x^5-x^3=x^3(x^2-1)=x\cdot x\cdot x\cdot (x^2-1)$
>
>In questo modo risulta evidente che $a=0$ annulla 3 volte il polinomio $p(x)$, dunque è un suo zero con molteplicità tre.

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
- [YouMath](https://www.youmath.it/lezioni/algebra-elementare/polinomi/5218-zeri-di-un-polinomio.html)