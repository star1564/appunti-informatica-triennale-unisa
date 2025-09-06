---
aliases: 
tags:
  - corsi/informatica/architettura_degli_elaboratori
paragrafo: MIPS - Processore
cssclasses:
  - 
---
Questo significa *propagare i dati in avanti*, non appena sono disponibili, *verso le unità che li richiedono*.

> [!example]+ <font color="orange">Esempio</font>
>```
>add $2, $1, $3
>sub $12, $2, $5
>```
>Non appena la ALU calcola la somma tra $1 e $3, *il risultato viene subito messo a disposizione* della sub.
>
>Ciò può essere fatto mediante l'aggiunta di un circuito di propagazione che fornisce, in un punto dell'esecuzione il dato mancante.

Abbiamo bisogno di un modo per rilevare gli Hazard.
Le condizioni che generano [[068 Hazard sulla Pipeline#HAZARD SUI DATI|hazard sui dati]] sono di 4 tipi diversi:
1. *EX/MEM.RegistroRd = ID/EX.RegistroRs*;
2. *EX/MEM.RegistroRd = ID/EX.RegistroRt*;
3. *MEM/WB.RegistroRd = ID/EX.RegistroRs*;
4. *MEM/WB.RegistroRd = ID/EX.RegistroRt*.

I primi due hazard si verificano quando uno dei due operandi (rs, rt) di una istruzione che is trova nello **stadio ID** e che deve entrare dello **stadio EX** (ed è quindi presente nel registro ID/EX) dipende dal risultato di una istruzione precedente che viene prodotto nello **stadio EX** (ed è quindi presente nel registro EX/MEM).
![[Pasted image 20211220092353.png|400]]

Dato che non tutte le istruzioni scrivono nel banco dei registri, possiamo evitare di propagare controllando se *RegWrite = 0*.

## UNITÀ DI PROPAGAZIONE

Per consentire agli input dell'ALU di provenire da uno qualsiasi dei registri di pipeline ID/EX, EX/MEM, MEM/WB, vengono aggiunti dei multiplexer. I segnali di controllo di tali multiplexer (*PropagaA* e *PropagaB*) sono generati da una **Unità di Propagazione**.

![[Pasted image 20211220092914.png]]

![[Pasted image 20211220093022.png]]


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