---
aliases:
  - Requisito
  - Requisiti Funzionali
  - Requisiti Non Funzionali
  - Vincoli per Requisiti
  - Ingegneria dei Requisiti
tags:
  - corsi/informatica/ingegneria_software
paragrafo: Analisi dei Requisiti - Estrazione
cssclasses:
  - 
---
>Un **Requisito** è una *caratteristica* che il sistema deve avere o un *vincolo* che deve soddisfare per essere accettato dal cliente. È scritto in un linguaggio naturale.

### Requisiti Funzionali
>I **Requisiti Funzionali** descrivono le *interazioni tra il sistema e il suo ambiente*, indipendentemente dalla sua implementazione. L'ambiente include l'*utente* e qualsiasi altro *sistema esterno* con cui il sistema interagisce. 

Quindi, i requisiti funzionali includono le *operazioni principali* che il sistema deve supportare.

> [!example]- <font color="orange">Esempio</font>
>- Il sistema di gestione della biblioteca deve consentire agli *utenti* di cercare, prenotare, prendere in prestito, restituire e aggiungere nuovi libri alla collezione della biblioteca. 
>- Gli utenti devono poter effettuare *ricerche* nella collezione di libri in base a titolo, autore o genere.
>- Deve anche *tenere traccia dei dati sugli utenti*, inclusi nomi, indirizzi, numero di telefono e storico di prestito. 
>- Il sistema deve inoltre consentire al *personale* della biblioteca di registrare nuovi arrivi di libri, gestire le scadenze dei prestiti e inviare avvisi agli utenti in caso di ritardo nella restituzione dei libri.

### Requisiti Non Funzionali
>I **Requisiti non Funzionali** descrivono aspetti del sistema che *non sono direttamente correlati al comportamento funzionale del sistema*. I requisiti non funzionali includono una vasta gamma di requisiti che si applicano a molti aspetti diversi del sistema, dalla *usabilità* alle *prestazioni*.

Quindi i requisiti funzionali includono *aspetti visibili (ma non influenzabili)* da parte degli utenti.

> [!example]- <font color="orange">Esempio</font>
>- L'applicazione web deve avere un *tempo di risposta* medio inferiore a 2 secondi per ogni richiesta utente.
>- Deve essere disponibile 24 ore su 24.

### Vincoli
>I **Vincoli** descrivono il *tipo di ambiente* in cui il sistema deve funzionare.

> [!example]- <font color="orange">Esempio</font>
> - La dimensione dell'intera applicazione deve essere *al di sotto di 10 GB*.

#
___
[[000 Indice IS|↖ Ritorna all'indice ↖]]

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