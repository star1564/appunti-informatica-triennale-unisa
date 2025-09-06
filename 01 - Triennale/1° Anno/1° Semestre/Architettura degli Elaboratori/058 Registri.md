---
aliases: 
tags:
  - corsi/informatica/architettura_degli_elaboratori
paragrafo: MIPS - Processore
cssclasses:
  - 
---
I registri sono delle memorie interne al processore i cui dati, che provengono dalla memoria, sono usati dagli altri componenti (come la [[037 ALU|ALU]]).

Nel processore [[045 MIPS|MIPS]] ci sono *32 registri a 32 bit*. Ce ne sono pochi per velocizzare le operazioni.

## REGISTRO SINGOLO

I **registri** sono realizzati attraverso componenti che memorizzano *un solo bit*:
- Il [[059 Latch|Latch S-R]];
- Il [[060 Flip-Flop|Flip-Flop (con ciclo di clock)]].

Un registro corrisponde ad un *array di 32 flip-flop di tipo D* connessi tra di loro con lo stesso segnale di clock.

In un registro si può **leggere o scrivere** una stringa di n bit. 

Si aggiunge un bit, il *segnale di Write*, dove:
- Se *0*, si esegue la lettura (*READ*), che non altera lo stato del registro;
- Se *1*, si esegue la scrittura (*WRITE*), che lo altera.

![[Pasted image 20211126140651.png|250]]

## BANCO DEI REGISTRI (REGISTER FILE)

Il **banco dei registri** è composto da *32 registri* sui quali si può scrivere e si possono leggere dati.

![[Immagine 2021-12-01 164058.jpg|400]]

### LETTURA DATI

Per *leggere* un registro occorre specificare in input il numero del registro (Register number, di 5 bit), ottenendo in output il suo **contenuto** (Read data, di 32 bit).

#### REALIZZAZIONE

Nella realizzazione si usa un [[021 Multiplexer|Multiplexer 32:1]] in cui il register number è usato per ottenere i cinque segnali di selezione del MUX. L'Output del MUX è il contenuto del registro indicato.

![[Immagine 2021-12-01 164306.jpg|500]]

### SCRITTURA DATI

Per *scrivere* in un registro occorre specificare in input il numero del registro (Register number, di 5 bit), in cui scrivere ed il **dato da scrivere** (Write data, di 32 bit). Questo può accadere solamente se il *segnale di RegWrite* è 1.

#### REALIZZAZIONE

Nella realizzazione si usa un [[020 Decodificatore|Decodificatore]] con 5 input e 32 output per determinare il registro in cui scrivere.

L'output del decodificatore va in una porta [[026 AND (AdE)|AND]] insieme al segnale di Write ed *abilita alla scrittura* nel registro corrispondente.

I 32 bit del dato da scrivere diventano gli input dei 32 Flip-Flop di tipo D che formano il registro in cui scrivere, il cui stato viene modificato.

![[Immagine 2021-11-26 142657.jpg|500]]



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