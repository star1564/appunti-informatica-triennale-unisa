---
tags:
  - corsi/informatica/reti_calcolatori
paragrafo: Livello Rete
cssclasses:
  - 
---
> Il **Livello di Rete** è il terzo strato del [[005 Modello ISO-OSI|Modello ISO-OSI]] e si occupa dell'*instradamento dei pacchetti* tra due dispositivi *separati da un certo numero di nodi intermedi* in una rete.

Il suo obiettivo principale è quello di fornire agli strati superiori un servizio per la consegna dei dati, in modo da far apparire la trasmissione come una trasmissione [[002 Rappresentazione di Reti#Punto a punto|punto a punto]], *mascherando l'infrasturttura della rete*, indipendentemente dalla topologia e dalla tecnologia di rete utilizzata.

![[Pasted image 20230526110127.png]]

## Funzioni fondamentali del livello di rete

### Instradamento (Routing)
L'**Instradamento** ([[026 Routing e Forwarding|routing]]), consiste nel *determinare il percorso ottimale* che i pacchetti di dati devono seguire per raggiungere la destinazione desiderata;
- Avviene attraverso degli algoritmi di instradamento;
- Deve anche gestire le *congestioni* (quando la quantità di traffico di rete supera la capacità disponibile dei collegamenti) scegliendo *percorsi alternativi*;
- In questa funzione, si crea e aggiorna una *tabella di routing* che associa alla destinazione la linea di uscita da utilizzare. ^17460a

### Inoltro (Forwarding)
L'**Inoltro** ([[026 Routing e Forwarding#Forwarding|forwarding]]), consiste nell'*inoltro dei pacchetti lungo il percorso* determinato dall'instradamento.
- Sceglie, in base all'indirizzo di destinazione o al circuito virtuale, la linea di uscita in funzione dei dati noti dalla tabella di routing.

## Datagrammi
Il livello di rete opera sui pacchetti di dati noti come **Datagrammi**. Questi datagrammi contengono informazioni sull'*indirizzo di origine e destinazione*, così come i *dati* stessi. Durante il processo di instradamento, i datagrammi vengono passati da un nodo di rete all'altro, utilizzando informazioni di instradamento contenute nella tabella di routing. ^9b21d6

Il livello di rete del modello ISO-OSI è implementato in protocolli come [[023 Internet Protocol (IPv4)|IP (Internet Protocol)]] che sono ampiamente utilizzati nella comunicazione su Internet. IP è responsabile dell'indirizzamento, dell'instradamento e dell'inoltro dei pacchetti di dati attraverso la rete.

___
[[000 Indice RC|↖ Ritorna all'indice ↖]]
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