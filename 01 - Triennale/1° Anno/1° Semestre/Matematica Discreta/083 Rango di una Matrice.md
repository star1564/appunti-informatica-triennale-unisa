---
aliases: 
tags:
  - corsi/matematica/matematica_discreta
paragrafo: Matrici - Terminologie
cssclasses:
  - 
---
Sia $A\in M_{m,n} (\mathbb{R})$, con $m$ righe ed $n$ colonne (dove può succedere che $m\neq n$).

Tra le [[066 Sottomatrici|Sottomatrici]] di $A$ ci sono delle sottomatrici quadrate e di queste è possibile calcolarne il [[079 Determinante di una Matrice Quadrata|Determinante]].

>Il determinante di una sottomatrice quadrata di $A$ di ordine $t$ si chiama **minore di ordine $t$** di $A$.

>Il **rango** di $A$, che si indica con **$\rho(A)$**, è un intero non negativo associato alla matrice $A$.

Questo è dato *dall'ordine massimo dei minori non nulli* che si possono estrarre da $A$, cioè l'ordine massimo delle sottomatrici quadrate che si possono estrarre da $A$ con determinante diverso da zero.

Si nota che in una matrice rettangolare $A$ con $m$ righe e $n$ colonne ha rango compreso tra 0 e il minimo tra il numero di righe e il numero di colonne della matrice. Ovvero:
> *$$0\le \rho(A)\le minimo(m,n)$$*

Il rango di una [[063 Matrici a Scala Ridotta|Matrice a Scala Ridotta]] è uguale al numero dei suoi pivot.

Il rango di due [[062 Matrici Equivalenti|matrici equivalenti]] sono uguali, ovvero $\rho(A)=\rho(B),\quad A\sim B$.

### TIPI DI RANGHI
#### RANGO 0
Diremo che $A$ ha rango **0** se A è la *matrice nulla*.

#### RANGO > 0
Una matrice ha il rango **$\rho(A)=k\in \mathbb{N}$** se:
1. $A$ possiede un minore di *ordine $k$ diverso da 0*;
2. *$A$ possiede una sottomatrice quadrata di ordine $k$* ($B$) con *$det\neq 0$* e tutte le sottomatrici quadrate di ordine $k+1$ che hanno $B$ come sottomatrice (orlati di B) hanno *$det = 0$*.

#### RANGO MASSIMO
Nel caso in cui *il rango coincida con il minimo* tra $m$ ed $n$, cioè:
$$\rho(A)=minimo(m,n)$$
si dice che la matrice ha **rango massimo**.

## CALCOLO DEL RANGO
Il rango di una matrice si può calcolare in diversi metodi:
- [[084 Calcolo Rango con Minore|Calcolo a partire dal rango massimo con i Minori]];
- [[085 Calcolo Rango con Orlati|Calcolo con gli Orlati]];
- [[061 Matrici a Scala|Trasformare una matrice in una matrice a scala]] e contarne i pivot.

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
- Pagina Libro: 283