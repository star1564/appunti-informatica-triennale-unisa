---
aliases:
  - System Call
tags:
  - corsi/informatica/sistemi_operativi
paragrafo: Basi di un Sistema Operativo
cssclasses:
  - 
---
Le **Chiamate di Sistema** (system call) costituiscono un'interfaccia per i [[004 Servizi di un Sistema Operativo|servizi resi disponibili]] dal sistema operativo.
Tali chiamate sono tipicamente disponibili sotto forma di routine scritte in C o C++.

Per esempio, il comando `cp in.txt out.txt` copia il file in ingresso `in.txt` nel file in uscita `out.txt`.
Questo comando è composto da varie system call (come la scrittura di messaggi sul terminale, lettura e scrittura nei file, ecc.).

![[Pasted image 20220929112349.png|800]]
<center>Tutte le chiamate di sistema del comando <code>cp</code></center>

## CATEGORIE DI SYSTEM CALL
Le chiamate di sistema sono classificabili approssimativamente in sei categorie principali:
- **Controllo dei Processi**: composto da chiamate atte alla creazione ed alla terminazione di processi (sia in maniera normale che anomala);

- **Gestione dei File**: composto da chiamate atte alla creazione, eliminazione, apertura, chiusura, lettura e scrittura di file;
- **Gestione dei Dispositivi**: le diverse risorse controllate dal sistema operativo si possono concepire come dei dispositivi (fisici e virtuali), quindi questa sezione di categorie riguarda la loro gestione;
- **Gestione delle Informazioni**: composto da chiamate atte al trasferimento di informazioni generali tra il programma utente e il sistema operativo (come dati del sistema o l'orario);
- **Comunicazione**: principalmente composto da chiamate per la connessione ad altre macchine;
- **Protezione**: composto da chiamate atte ad offrire meccanismi di protezione (come per i permessi dei file).

___
[[000 Indice SO|↖ Ritorna all'indice ↖]]

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

