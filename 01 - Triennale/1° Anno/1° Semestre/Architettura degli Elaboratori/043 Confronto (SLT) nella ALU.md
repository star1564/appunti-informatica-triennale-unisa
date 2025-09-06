---
aliases: 
tags:
  - corsi/informatica/architettura_degli_elaboratori
paragrafo: ALU
cssclasses:
  - 
---
Per vedere se $a<b$ è vera si esegue la sottrazione $a-b$:
Se il risultato è **negativo**, allora *$a<b$ VERA*;
Altrimenti, *$a<b$ FALSA* ($a\geq b$).

Dato che usiamo il [[008 Complemento a 2|Complemento a 2]], possiamo notare se il numero è negativo osservando il [[Bit Più Significativo]] del risultato:
Se questo è **1**, allora è negativo;
Altrimenti è positivo.


Si espande il multiplexer dell'ALU ad 1 bit, aggiungendo la terza operazione "*less*":
Dalla ALU 1 alla ALU 31, less sarà sempre 0;
Nella ALU 0, less sarà il risultato della sottrazione nell'ALU 31 di $a_{31}-b_{31}$ che verrà chiamato *set*.

Se il bit di controllo *Operation* è 3, l'ALU darà come risultato:
*000...000*, se  $set=a_{31}-b_{31} = 0$;
*000...001*, se  $set=a_{31}-b_{31} = 1$;

| CarryIn | Binvert | Operation | **Istruzione** |
| ------- | ------- | --------- | -------------- |
| 0       | 0       | 00        | AND            |
| 0       | 0       | 01        | OR             |
| 0       | 0       | 10        | sum            |
| 1       | 1       | 10        | sub            |
| 1       | 1       | 11        | slt            | 

^ca46cd

**$$ALU_{1\sim30}$$**

![[Pasted image 20211104182434.png|500]]

**$$ALU_{31}$$**

![[Pasted image 20211104182845.png|500]]
<font color="orange">Operazioni disponibili</font>: AND, OR, NOR, sum, sub, slt.

![[Pasted image 20211104182050.png|500]]

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
- Appendice B, pagina 33