---
aliases: 
tags:
  - corsi/informatica/sicurezza
paragrafo: Cifrari a Blocchi
cssclasses:
  - 
---
>Un **Cifrario a Blocchi** è di tipo [[004 Cifrari Simmetrici|simmetrico]] e consiste nel dividere il testo in chiaro in un *certo numero di blocchi* che poi vengono *cifrati*.

![[Pasted image 20240611140904.png|700]]

Un cifrario a blocchi opera su un blocco di testo in chiaro da $n$ bit e produce un blocco di testo cifrato da $n$ bit, il che comporta:
- $2^n$ possibili input (testo in chiaro)
- $2^n$ possibili output (testo cifrato)
- Per rendere possibile la *decifrazione* del blocco cifrato (*reversibile*), ogni blocco in chiaro deve corrispondere ad un solo blocco cifrato, questo implica che bisogna avere una [[021 Applicazioni Biettive|funzione biettiva]]. Quindi ci sono $2^n!$ possibili trasformazioni.

> [!example]- <font color="orange">Esempio</font>
>Per $n=2$ si ha questo.
>
>![[Pasted image 20240611141753.png]]
>
>Nella tabella di destra, la funzione non è biettiva.


Il processo di un cifrario di sostituzione generale per blocchi (di 4 bit) si svolge in due fasi:
1. Un blocco di testo in chiaro di 4 bit viene utilizzato come input per il cifrario. Esistono 16 possibili blocchi di testo in chiaro (da 0000 a 1111).
2. Il cifrario mappa il blocco di testo in chiaro in uno dei 16 possibili stati di uscita. Ogni stato di uscita è rappresentato da un nuovo blocco di 4 bit di testo cifrato.

![[Pasted image 20240611143636.png|1000]]

Ma questo non è praticabile per blocchi con molti bit. Occorre quindi un'approssimazione, un metodo compatto per rappresentare la funzione di cifratura/decifratura. Uno di questi è il [[009 Cifrari di Feistel|Cifrario di Feistel]].

___
[[000 Indice Sicurezza|↖ Ritorna all'indice ↖]]

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