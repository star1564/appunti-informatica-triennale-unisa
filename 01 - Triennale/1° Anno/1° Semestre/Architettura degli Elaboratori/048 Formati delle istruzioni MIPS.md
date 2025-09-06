---
aliases:
tags:
  - corsi/informatica/architettura_degli_elaboratori
paragrafo: MIPS - Assembly
cssclasses:
  -
---
>Una istruzione MIPS viene convertita in *una sequenza univoca di 32 bit*, in base al tipo di operazione da eseguire (**formato**), viene suddivisa in diversi campi.

Per effettuare operazioni tra più di due operandi bisogna utilizzare più istruzioni.

<font color="green">La scheda tecnica riassuntiva del MIPS si trova alla penultima e all'ultimissima pagina del libro OPPURE ALLA FINE DI QUESTA PAGINA</font>.

## FORMATO R

Il formato **R** (da Registro) viene usato per operazioni normali.
La sequenza è suddivisa in *6 campi*.

![[Pasted image 20211112110846.png|600]]
*OP*: Codice operativo dell'istruzione.
*rs*: Primo operando sorgente.
*rt*: Secondo operando sorgente.
*rd*: Operando destinazione.
*shamt*: Shift amount (per le operazioni di shift).
*funct*: Definisce la funzionalità specifica dell'istruzione (insieme ad OP).

## FORMATO I

Il formato **I** (da Immediato) viene usato per le operazioni immediate e per le operazioni di trasferimento.
La sequenza è suddivisa in *4 campi*.

![[Pasted image 20211112111954.png|600]]
*OP*: Codice operativo dell'istruzione.
*rs*: Primo operando sorgente.
*rt*: Operando destinazione.
*costante o indirizzo*: Riservato alla costante o all'indirizzo.

## FORMATO J

Il formato **J** (da Jump) viene usato per il salto incondizionato.
La sequenza è suddivisa in *2 campi*.

![[Pasted image 20211112121059.png|600]]
*OP*: Codice operativo dell'istruzione.
*etichetta o indirizzo di salto*: Riservato all'etichetta o all'indirizzo.

___

> [!summary]- PDF istruzioni MIPS
> ![[Scheda tecnica MIPS.pdf]]

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