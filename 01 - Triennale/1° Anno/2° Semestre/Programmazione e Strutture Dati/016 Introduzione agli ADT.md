---
aliases: 
tags:
  - corsi/informatica/programmazione_strutture_dati
paragrafo: Abstract Data Type
cssclasses:
  - 
---
Gli **Abstract Data Type** (ADT) sono dei tipi (o classi) per oggetti il cui comportamento è definito da un numero finito di valori ed operazioni.

La definizione di ADT menziona solamente come devono funzionare certe operazioni ma non come sono implementate. Infatti non specifica come i dati saranno organizzati nella memoria e quali algoritmi verranno utilizzati per implementare le operazioni. Si usa l'[[001 Introduzione a OOP#^1dfc00|astrazione]] per poter creare gli ADT.

Per esempio, il cliente di un tipo di dati *primitivo* (int, float, char, …) o *aggregato* (array, puntatori, ...) non deve sapere come quel tipo di dati viene implementato, ma deve sapere solo quali tipi di operazioni si possono eseguire con essi.

Per avere una lista di tutte le funzioni necessarie di un ADT utilizzeremo una tabella che si occupa della *Specifica* e della *Implementazione*.

![[19f094ac-d4c7-431a-86cb-c24426a6b5c3.jpg]]

La Specifica si occupa della definizione del tipo di dati, della definizione della *Sintattica* (regole di nomi e tipi) e *Semantica* (significati di valori e vincoli).
L'Implementazione è la codifica di quello che è stato definito nella specifica.

Gli ADT si implementano grazie all'utilizzo delle [[018 Strutture|Strutture]], che contengono tutti i dati necessari in un blocco di memoria.

## STRUTTURE DATI CON GLI ADT
Attraverso gli ADT si possono implementare le **Strutture Dati**. Le strutture dati disponibili sono: ^bb3682
- [[020 Liste nel C|Liste]];
- [[c314fe4c-8b0a-4c7f-919d-2ba5be100323.jpg|Stack (Pile)]];
- [[022 Queue nel C|Queue (Code)]];
- [[023 Alberi Binari|Alberi Binari]];
- [[024 Alberi Binari di Ricerca|Alberi Binari di Ricerca]];
- [[026 Tabelle Hash|Tabelle Hash]].

___
[[000 Indice PSD|↖ Ritorna all'indice ↖]]

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
- [In parte da GeeksForGeeks](https://www.geeksforgeeks.org/abstract-data-types/)