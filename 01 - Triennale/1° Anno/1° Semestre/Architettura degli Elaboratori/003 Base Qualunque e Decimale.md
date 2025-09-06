---
aliases:
tags:
  - corsi/informatica/architettura_degli_elaboratori
paragrafo: Rappresentazione Numeri - Interi
cssclasses:
  -
---

> [!note] Interi

## BASE QUALUNQUE → DECIMALE

Si moltiplica ogni cifra binaria per l'opportuna potenza di 2 e si esegue la somma.

Ci sono due metodi, funzionanti per qualsiasi base:
- **[[Sommatoria]]**:
**$$N=a_{n-1}\times b^{n-1}+ a_{n-2}\times b^{n-2} +...+ a_{1}\times b^{1}+ a_0\times b^0$$**  $$=$$ $$\sum_{i=0}^{n-1} a_i \times b^i$$
Dove: *a* è il contenuto della cifra; *n* è il numero delle cifre; *b* è la base qualunque del numero.

> [!example]- <font color="orange">Esempio</font>
>Con $n = 5, b = 2$
>$(11010)_2 = 1\times2^4 + 1\times2^3 + 0\times2^2 + 1\times2^1 + 0\times2^0 =$ **$2^4+2^3+2^1$** $=16+8+2=26$ 

- **Partendo dal [[Bit Più Significativo]] fino ad arrivare a quello [[Bit Meno Significativo]]**:
$$S_{n-1}=a_{n-1}$$
$$S_{n-2}=a_{n-2} + b \times S_{n-1}$$
$$...$$
$$S_{2}=a_{2} + b \times S_{3}$$
$$S_{1}=a_{1} + b \times S_{2}$$
$$N = S_{0}=a_{0} + b \times S_{1}$$
Dove: *a* è il contenuto della cifra; *n* è il numero delle cifre; *b* è la base qualunque del numero.

## DECIMALE → BASE QUALUNQUE

Dato un N decimale, per ottenere un numero in un altra base, si esegue un [[044 Algoritmo delle Divisioni Successive|algoritmo delle divisioni successive]], dove il quoziente sarà il numero decimale $\frac{N}{b}$, $b$ la base in cui si deve convertire $N$, $r$ il resto della divisione di $N/b$.

Quando si arriva ad un **quoziente pari a 0**, l'algoritmo si ferma.

Il numero in Base Qualunque convertito si otterrà prendendo le cifre dei **resti** a partire dal basso andando verso l'alto. Secondo l'esempio riportato, $(512)_{10}=(100000000)_2$.  

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