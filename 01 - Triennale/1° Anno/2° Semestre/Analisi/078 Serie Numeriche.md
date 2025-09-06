---
aliases:
  - Successione di Somme Parziali
tags:
  - corsi/matematica/analisi
paragrafo: Serie Numeriche
cssclasses:
  - 
---
>Sia $\{a_n\}_{n\in\mathbb{N}}$ una [[029 Successioni|successione di numeri reali]], ad esempio $(a_0,a_1,a_2,...,a_n,...)$.
>
>Si dice **Serie Numerica** la *somma di tutti gli elementi di una successione* e si rappresenta con: $$\textcolor{#CC241D}{\displaystyle\sum_{n=1}^{+\infty} a_n}=a_1+a_2+a_3+\dots+a_n+\dots$$

## Successione di Somme Parziali
Dato che è *impossibile sommare infiniti numeri*, per studiare questa sommatoria è necessario riguardarla come una particolare successione, chiamata **successione delle somme parziali**:
$$
\begin{align*}
S_1 &= a_1 \\
S_2&= a_1+a_2 \\
S_3&= a_1+a_2+a_3 \\
&\dots \\
\color{#CC241D}S_n&= \color{#CC241D}a_1+a_2+a_3+\dots+a_n \\
&\dots \\
\end{align*}
$$

Ovvero la somma di tutti gli elementi della successione, dal primo all'$n$-esimo.

## Termini di una Serie
Una serie $\displaystyle\sum_{n=1}^{+\infty} a_n$ si dice a:
- **Termini Positivi**: se per ogni $n\in\mathbb{N}$ i termini della successione $\{a_n\}_n$ sono *tutti positivi*. ^c8052e
- **Termini Negativi**: se per ogni $n\in\mathbb{N}$ i termini della successione $\{a_n\}_n$ sono *tutti negativi*.


___
#
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
- [YouMath](https://www.youmath.it/lezioni/analisi-matematica/serie-numeriche/727-definizione-di-serie-numerica.html)
- [Elia Bombardelli](https://youtu.be/zrHz138eyv0?t=7)