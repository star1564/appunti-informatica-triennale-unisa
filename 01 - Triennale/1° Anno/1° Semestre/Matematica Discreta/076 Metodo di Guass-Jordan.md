---
aliases: 
tags:
  - corsi/matematica/matematica_discreta
paragrafo: Matrici - Sistemi
cssclasses:
  - 
---
>Con questo metodo si possono *risolvere* le matrici assegnati a dei [[075 Sistemi di Equazione in Matrice|sistemi di equazioni lineari]].
>1. Si [[073 Algoritmo di Gauss|riduce a scala la matrice]];
>2. Si [[063 Matrici a Scala Ridotta|riduce a scala ridotta la matrice]];
>3. La *colonna più a destra* rappresenta i risultati.


> [!info] [[075 Sistemi di Equazione in Matrice#CASI POSSIBILI DI SOLUZIONI|Casi possibili in un Sistema di Equazioni Lineari]]

#### CASI DI UNA SOLA SOLUZIONE
Si può notare che **esiste una ed una sola soluzione** *se abbiamo una matrice a scala ridotta* e *abbiamo la colonna più a destra piena* con le soluzioni delle incognite.

> [!example] <font color="orange">Esempio</font>
![[Pasted image 20211025112139.png]]

#### CASI DI SOLUZIONI IMPOSSIBILI
Si può notare che una soluzione è impossibile *se una o più righe della matrice a scala ridotta hanno 0 per tutti i posti rappresentanti le incognite* ed un risultato qualsiasi all'ultimo posto. 

Questo si nota anche in una matrice a scala non ancora ridotta, come in questo caso.

> [!example] <font color="orange">Esempio</font>
![[Pasted image 20211025112802.png]]

#### CASI DI SOLUZIONI INFINITE
Si può notare che una soluzione è infinita *se una o più righe della matrice a scala ridotta hanno 0 per tutti i posti*. 

Quando il numero di pivot nella matrice a scala ridotta finale è minore del numero di incognite del sistema, si introducono n parametri (n =  num. incognite - num. pivot). Per ogni scelta di numeri reali da affidare a questi parametri, si possono trovare infinite soluzioni del sistema, poiché i parametri possono essere scelti in infiniti modi.

Oppure:
$$sol=infinito^{num.incognite-rango\ matrice\ incompleta}$$

> [!example] <font color="orange">Esempio</font>
![[Pasted image 20211025113004.png]]

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
- Pagina Libro: 285