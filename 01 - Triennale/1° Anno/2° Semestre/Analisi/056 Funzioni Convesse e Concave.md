---
aliases: 
  - Funzione Convessa
  - Funzione Concava
tags:
  - corsi/matematica/analisi
paragrafo: Derivate
cssclasses:
  - 
---
Considerare una [[011 Funzioni|funzione]] reale a valori reali $f:\mathbb{R}\to \mathbb{R}$.

Qui sono presentate delle definizioni semplici.
Se $f$ è una funzione [[046 Derivata di una Funzione#Funzione Derivabile|Funzione Derivabile]] in un intervallo $(a,b)$ è possibile usare una definizione alternativa.

### Funzione Convessa
>Si dice che $f(x)$ è **Convessa** se e solo se, prendendo due punti qualsiasi del suo grafico, il segmento che li congiunge *si trova al di sopra* del grafico stesso.

![[Pasted image 20220505104359.png|500]]


> [!example]+ <font color="orange">Esempio</font>
>La [[F10 Funzione Valore Assoluto|funzione del valore assoluto]] è una funzione convessa.


> [!quote]+ Definizione di Convessità con una Funzione Derivabile 
>Si dice che in $x_0$ la funzione è convessa se esiste un [[009 Intorno di un Punto|intorno]] $I_{x_0}$ di $x_0$ tale che $$f(x)> t(x),\quad \forall x\in I_{x_0}-\{x_0\}$$
>
>Dove $t$ è la funzione della retta tangente alla funzione $f$.
>
>![[Immagine 2024-10-04 174305_1.png|350]]

### Funzione Concava
>Si dice che $f(x)$ è **Concava** se e solo se, prendendo due punti qualsiasi del suo grafico, il segmento che li congiunge *si trova al di sotto* del grafico stesso.

![[Pasted image 20220505104441.png|500]]

> [!example]+ <font color="orange">Esempio</font>
> - La [[F09 Funzione Logaritmica|funzione logaritmica]] è una funzione concava
> - La [[F01 Funzione Lineare e Costante|funzione lineare o costante]] è una funzione sia concava che convessa

> [!quote]+ Definizione di Concavità con una Funzione Derivabile 
>Si dice che in $x_0$ la funzione è concava se esiste un [[009 Intorno di un Punto|intorno]] $I_{x_0}$ di $x_0$ tale che $$f(x)< t(x),\quad \forall x\in I_{x_0}-\{x_0\}$$
>
>Dove $t$ è la funzione della retta tangente alla funzione $f$.
>
>![[Immagine 2024-10-04 174305.png|350]]








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
- [YouMath](https://www.youmath.it/lezioni/analisi-matematica/derivate/2297-teoremi-derivata-seconda.html)