---
aliases: 
tags:
  - corsi/informatica/architettura_degli_elaboratori
paragrafo: Rappresentazione Numeri - Interi
cssclasses:
  - 
---
> [!note] Interi

## OTTALE → ESADECIMALE

Possiamo usare la *codifica binaria come rappresentazione intermedia*, [[004 Ottale e Binario#OTTALE → BINARIO|convertendo il numero da ottale a binario]], per poi [[005 Esadecimale e Binario#BINARIO → ESADECIMALE|convertirlo da binario ad esadecimale]].

> [!example]- <font color="orange">Esempio</font>
>$(1076)_{8}$
>$1 = 001$
>$0 = 000$ 
>$7 = 111$
>$6 = 110$
>$(1076)_{8} = (001\>\>000\>\>111\>\>110)_2$, quindi:
>
>$(0010\>\>0011\>\>1110)_2$
>$1110 = 14 = E$
>$0011 = 3$
>$0010 = 2$
>$(0010\>\>0011\>\>1110)_2 = (23E)_{16}$
>![[Immagine 2021-10-17 213253.jpg|200]]

## Esadecimale → Ottale

Possiamo usare la *codifica binaria come rappresentazione intermedia* con questo metodo:
Si [[005 Esadecimale e Binario#ESADECIMALE → BINARIO|converte il numero da esadecimale a binario]], per poi [[004 Ottale e Binario#BINARIO → OTTALE|convertire da binario ad ottale]].

> [!example]- <font color="orange">Esempio</font>
>$(1F0C)_{16}$
>$1 = 0001$
>$F = 1111$ 
>$0 = 0000$
>$C = 1100$
>$(1F0C)_{16} = (0001\>\>1111\>\>0000\>\>1100)_2$, quindi:
>
>$(0\>\>001\>\>111\>\>100\>\>001\>\>100)_2$
>$100 = 4$
>$001 = 1$
>$100 = 4$
>$111 = 7$
>$001 = 1$
>$0 = 0$
>$(0\>\>001\>\>111\>\>100\>\>001\>\>100)_2 = (17414)_{8}$
>![[Immagine 2021-10-17 214253.jpg|]]

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