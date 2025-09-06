---
aliases:
tags:
  - corsi/informatica/architettura_degli_elaboratori
paragrafo: Rappresentazione Numeri - Con Segno
cssclasses:
  -
---
> [!note] Con segno

Nel Complemento a 2 il **peso** del [[Bit Più Significativo]] è *negativo*.

L'**intervallo di rappresentazione** è:
**$$[-2^{n-1}, +2^{n-1}-1]$$**

^5f9c5a

Il valore di $a_{n-1}a_{n-2}...a_{1}a_{0}$ è dato dalla seguente relazione:
$$N=-2^{n-1}a_{n-1}+\sum^{n-2}_{i = 0}2^i\times a_i$$

> [!example]- <font color="orange">Esempio</font>
><font color="green">0</font>0010010<sub>C2</sub> = +2<sup>4</sup> + 2<sup>1</sup> = <font color="green">+18</font><sub>10</sub>
><font color="red">1</font>0010010<sub>C2</sub> = -2<sup>7</sup> + 2<sup>4</sup> + 2<sup>1</sup> = -128 + 18 = <font color="red">-110</font><sub>10</sub>

Quindi il bit più significativo non indica solo il segno, ma **rappresenta se il numero è positivo o negativo**, questo perché:
**$$\sum^{n-1}_{i = 0}2^i=2^n-1$$**

> [!example]- <font color="orange">Esempio</font>
>**1**11111$_{C2}=-2^5+\textcolor{#61AFEF}{2^4+2^3+2^2+2^1+2^0}=-2^5+\textcolor{#61AFEF}{2^5-1} = -1$

> [!success] Vantaggi del Complemento a Due
> - Una sola rappresentazione dello zero;
> - Facile da ottenere l'opposto di un numero;
> - L'aritmetica è semplice;
> - Non si sprecano bit.

## CALCOLO DELL'OPPOSTO DI BINARI NEL C2

Dato un numero in binario puro positivo  $N_{10}$ ed i suoi bit $b$, esistono tre modi per poter rappresentare il loro opposto (cambiare il loro segno):
- Fare il **[[Complemento Bit a Bit]]**, per poi *sommare 1*.![[Pasted image 20211019231953.png|500]]  ^b531f2

- Usare la formula **$b_{n-2}...b_0=2^{n-1}-|N|$** (SOLO PER I POSITIVI);
	- Rappresentare il numero in [[003 Base Qualunque e Decimale#DECIMALE → BASE QUALUNQUE|binario puro]];
	- Aggiungere un nuovo [[Bit Più Significativo]] e renderlo 1.

- Partire da destra, andando verso sinistra, dopo aver trovato il primo 1, fare il [[Complemento Bit a Bit]] del resto. Il bit più significativo deve essere appropriato.

## AUMENTARE IL NUMERO DEI BIT
Per i numeri positivi si aggiunge 0 nella parte più significativa, 1 per i numeri negativi.

## DECIMALE → C2
- Si calcola il binario puro del decimale;
- Si aggiungono gli 0 per raggiungere il numero di bit richiesto;
- Se il numero da convertire è positivo: FINE;
- Altrimenti si calcola il suo opposto.


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