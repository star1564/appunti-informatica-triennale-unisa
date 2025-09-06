---
aliases: 
tags:
  - corsi/informatica/machine_learning
paragrafo: Data Engineering
cssclasses:
  - 
---
>La **trasmissione di dati tra processi** che *non condividono la memoria* può avvenire attraverso varie modalità di flusso dati. 

Esistono tre approcci principali.
### Passaggio di dati attraverso i database
Consiste nel *salvare i dati da un processo in un database*, permettendo al secondo processo di *recuperarli*.

Limitazioni:
- Richiede accesso condiviso al database da entrambi i processi.
- La lettura/scrittura potrebbe risultare troppo lenta in alcuni scenari.

### Passaggio di dati attraverso servizi
Sfrutta l'*invio diretto dei dati attraverso una rete* attraverso [[053 Web Service|Web Services]] (come *API REST*), [[006 Remote Procedure Calls|RPC]] e con richieste [[004 Richiesta e Risposta HTTP|POST/GET]].

Questo metodo è intrinsecamente legato all'architettura orientata ai servizi.

### Passaggio di dati attraverso un trasferimento in tempo reale
I dati trasmessi sono chiamati "*eventi*," caratterizzando un'architettura "event driven" o "[[049 Java Message Service|message driven]]".
- Due modalità comuni:
    - [[050 Tipi di destinazioni per Messaggi#Modello Publish-Subscribe|Publish-Subscribe]];
    - [[050 Tipi di destinazioni per Messaggi|Message Queue]].

## Dati Storici, Elaborazione Batch e Streaming
Una volta che i dati giungono nei motori di archiviazione dati, come database, data lake o data warehouse, assumono la natura di *dati storici*, in contrasto con i dati in streaming che continuano ad essere generati.

- **Dati Storici:**
	- Rappresentano *informazioni accumulate nel tempo*, solitamente conservate in strutture di archiviazione dati consolidate;
	- Spesso, l'elaborazione dei dati storici avviene attraverso job batch, eseguiti periodicamente.

- **Elaborazione Batch:**
	- Ideale per il trattamento efficiente di grandi volumi di dati in batch, avviato con cadenza regolare.

- **Elaborazione in Streaming:**
	- Se ben implementata, offre bassa latenza, consentendo l'elaborazione immediata dei dati appena generati, senza la necessità di archiviarli prima nei database.

Nell'ambito del Machine Learning si ritorna al problema dell'[[003 Apprendimento in modo Incrementale|apprendimento Offline-Online]], quindi l'**elaborazione in streaming (online)** è preferita per *calcolare caratteristiche che cambiano rapidamente*, mentre l'**elaborazione in batch (offline)** è più adatta per caratteristiche che cambiano con *frequenza inferiore*.

___
[[000 Indice ML|↖ Ritorna all'indice ↖]]

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