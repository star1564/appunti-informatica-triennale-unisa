---
aliases: 
  - Piano Cartesiano
  - Ordinate
  - Ascisse
tags:
  - corsi/matematica/analisi
paragrafo: Funzioni
cssclasses:
  - 
---
>Una *[[011 Funzioni|funzione]] di variabile reale a valori reali* può essere rappresentata mediante un **Grafico nel Piano Cartesiano**, vale a dire un diagramma che permette di osservare e analizzare tutte le proprietà di una funzione.

Il grafico di $f$ è il luogo geometrico dei punti del piano per cui ad ogni *ascissa* $\color{#61AFEF}x$ appartenente al dominio della funzione si associa l'*ordinata* $\color{#61AFEF}y=f(x)$, ovvero il valore $y$ che la funzione $f$ associa alla $x$ considerata. 
$$\text{Grafico}(f)=\{(x,y)\in \mathbb{R}\times \mathbb{R}:\ y=f(x)\}$$
![[Pasted image 20240927124816.png|500]]^69bcb6




L'unione di tutti i punti di coordinata $(x,y)$ del piano costituisce il grafico della funzione $f$.

![[Pasted image 20240930101811.png|850]]



## Parti dei grafici di una funzione

- $\color{green}0^+$ è lo zero che si raggiunge dalla parte positiva dell'asse delle ordinate $y$
  
  ![[Pasted image 20220311114922.png|600]]

- $\color{#CC241D}0^-$ è lo zero che si raggiunge dalla parte negativa dell'asse delle ordinate $y$
  
  ![[Pasted image 20220311115049.png|600]]

- $\textcolor{green}{\mathbb{R}^+}\equiv Im(f(x))\in [0, +\infty)$ è la parte superiore dell'asse delle ascisse $x$
  
  ![[Pasted image 20220311115408.png|600]]

- $\textcolor{#CC241D}{\mathbb{R}^-}\equiv Im(f(x))\in (-\infty, 0]$ è la parte inferiore dell'asse delle ascisse $x$
  
  ![[Pasted image 20220311115524.png|600]]

## Funzioni Elementari
Ci sono diversi tipi di grafici e proprietà per ogni *funzione elementare*: 
- [[F01 Funzione Lineare e Costante|Funzioni Lineari e Costanti]];
- [[F02 Funzione Potenza con Esponente Pari|Funzione Potenza con Esponente Pari]];
- [[F03 Funzione Potenza con Esponente Dispari|Funzione Potenza con Esponente Dispari]];
- [[F04 Funzione Razionale-Frazione|Funzione Razionale]];
- [[F05 Funzione Radice con Indice Pari|Funzione Radice con Indice Pari]];
- [[F06 Funzione Radice con Indice Dispari|Funzione Radice con Indice Dispari]];
- [[F07 Funzione Esponenziale|Funzione Esponenziale]];
- [[F08 Funzione Potenza con Esponente Variabile|Funzione Potenza con Esponente Variabile]];
- [[F09 Funzione Logaritmica|Funzione Logaritmo]];
- [[F10 Funzione Valore Assoluto|Funzione Valore Assoluto]].


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
