---
aliases:
  - Vandermonde
tags:
  - corsi/matematica/probabilità_statistica
inglese: Choose
paragrafo: Calcolo Combinatorio
cssclasses:
  - 
---
>Il **Coefficiente Binomiale** è un numero naturale definito a partire da una coppia di numeri naturali $n$ e $k$. Il coefficiente binomiale $n$ su $k$ rappresenta il numero di [[002 Sottoinsiemi|sottoinsiemi]] di $k$ elementi che si possono estrarre da un insieme di $n$ elementi. Utilizza il [[004 Fattoriale Discendente|Fattoriale Discendente]].
>$$\textcolor{#CC241D}{n\choose k}=\frac{(n)_k}{k!}=\color{#CC241D}\frac{n!}{k!(n-k)!}$$

## Proprietà

### In base al valore di $k$
- Se $k>n$ allora $\color{#61AFEF}{n\choose k}=0$
- Se $k=0$ allora $\color{#61AFEF}{n\choose 0}=1$
- Se $k=1$ allora $\color{#61AFEF}{n\choose 1}=n$
- Se $k=n$ allora $\color{#61AFEF}{n\choose n}=1$
- Se $k=n-1$ allora $\color{#61AFEF}{n\choose n-1}=n$ 
	- Stesso caso per quando si ha ${n+1\choose n}=n+1$ oppure ${n+2\choose n+1}=n+2$, ecc.

### Formula di ricorrenza dei coefficienti binomiali
$${n\choose k}={n-1\choose k}+{n-1\choose k-1}, \quad 1\le k\leq n$$

### Uguaglianza di Vandermonde
$${n+m\choose k}=\displaystyle\sum_{i=0}^{k} {n\choose i}{m\choose k-i}={n\choose 0}{m\choose k}+{n\choose 1}{m\choose k-1}+\dots + {n\choose k}{m\choose 0}$$

### Teorema del binomio
$$(x+y)^n=\sum_{k=0}^{n} {n\choose k}\cdot x^k\cdot y^{n-k},\quad\quad n\geq1$$

### Simmetria dei coefficienti binomiali

$${n\choose k}={n\choose n-k}$$

### Altre
$$n{n-1\choose k-1}=k{n\choose k}$$
$$\sum_{k=0}^{n} {n\choose k}=2^n$$


### Tavola di Tartaglia-Newton per calcolare i coefficienti binomiali
![[Pasted image 20230309154602.png]]




___
[[000 Indice PS|↖ Ritorna all'indice ↖]]

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
- [YouMath](https://www.youmath.it/lezioni/probabilita/calcolo-combinatorio/1515-coefficiente-binomiale.html)