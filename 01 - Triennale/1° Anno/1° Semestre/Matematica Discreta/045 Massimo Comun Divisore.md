---
aliases: [MCD, Massimo Comun Divisore]
tags:
  - corsi/matematica/matematica_discreta
paragrafo: Aritmetica
cssclasses:
  - 
---
>Il **Massimo Comun Divisore** (MCD) rappresenta il *numero più grande che divide contemporaneamente due o più numeri interi*, lasciando un resto uguale a *zero*.
>$$MCD(a,b)=max\{d\in \mathbb{N}: d \text{ divide } a \text{ e } d \text{ divide } b\}$$

In altre parole, il MCD di due o più numeri interi è il numero più grande che può essere utilizzato per suddividere ciascuno di questi numeri in parti uguali senza lasciare un resto.

> [!example]- <font color="orange">Esempio</font>
>- I divisori di 12 sono 1, 2, 3, 4, 6, e 12.
>- I divisori di 18 sono 1, 2, 3, 6, 9, e 18.
>
>Il MCD di 12 e 18 è 6, perché 6 è il numero più grande che può essere utilizzato per suddividere sia 12 che 18 in parti uguali senza lasciare un resto. 
>
>Quindi:
>- 12 diviso per 6 è uguale a 2, senza resto.
>- 18 diviso per 6 è uguale a 3, senza resto.


> [!quote] Definizione Formale
>Siano $a, b\in \mathbb{Z}$. Un intero $d\in \mathbb{Z}$ è detto massimo comun divisore di $a$ e $b$, se risulta che:
>1. *$d$*$|a$, *$d$*$|b$;
>2. $t|a$, $t|b \Longrightarrow$ *$t|d$*.
>
>In particolare, se:
>- $a=b=0$, allora $MCD(a, b) = 0$.
>- $a|b$, 	allora $MCD(a, b) = a$.
>- $b=0$, 	allora $MCD(a, b) = a$.
>
>Con $a, b, d\in \mathbb{Z}$ si ha che:
> $$d=MCD(a,b) \iff D(d) = D(a) \cap D(b)$$
>
><font color="green">Dimostrazione (pL 179, 5.4.3)</font>.

### Calcolare il MCD
Per determinare il MCD si può usare l'[[044 Algoritmo delle Divisioni Successive|algoritmo delle divisioni successive]].

Quando si arriva ad un **resto pari a 0**, l'algoritmo si ferma ed il Massimo Comun Divisore è l'*ultimo resto diverso da zero* ottenuto.

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