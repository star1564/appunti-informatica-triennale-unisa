---
aliases:
  - Web Service
  - Service Oriented Architecture
  - Servizi Web
tags:
  - corsi/informatica/programmazione_distribuita
paragrafo: Web Services
cssclasses:
  - 
---
>Un **Web Service** è un sistema software in grado di *offrire un servizio* ad un consumer (anche *altri software*) che vi si collegano attraverso vari protocolli di comunicazione: 
>- *[[003 HTTP|HTTP]]* (principalmente questo);
>- [[037 SMTP|SMTP]];
>- [[049 Java Message Service|JMS]].

Un **Servizio** ha quattro proprietà:
1. Rappresenta logicamente *un'attività ripetibile con un risultato specifico*.
2. È *autonomo*.
3. È una "*scatola nera*" per i suoi consumers, il che significa che il consumer non deve essere a conoscenza del funzionamento interno del servizio (come il linguaggio in cui è scritto).
4. Può essere *composto da altri servizi*.

Un Web Service espone un'*interfaccia* ([[024 Dipendenze e Tipi di Iniezioni#Loosely Coupled|loose coupling]]) assieme alla *descrizione delle sue caratteristiche*, cioè è in grado di far sapere che funzioni mette a disposizione (senza bisogno di conoscerle a priori) e permette di capire come vanno utilizzate.

![[Pasted image 20231123180436.png]]

> [!info] I tre pilastri dei Web Service
> - **[[054 WSDL|WSDL]]**, l'interfaccia che espone un WS;
> - **[[055 SOAP|SOAP]]**, il protocollo per definire i messaggi dei WS;
> - **[[056 UDDI|UDDI]]**, un registro di servizi Web disponibili.

### Service Oriented Architecture
Il **Service Oriented Architecture** (SOA) è un'architettura software adatta a supportare l'uso di servizi web per garantire l'[[Interoperabilità]] tra diversi sistemi. Questa si concentra sulla creazione di **micro-servizi** invece di una applicazione *monolitica*:
- Con un'architettura basata su micro-servizi, un'applicazione è *realizzata da componenti indipendenti* che eseguono ciascun processo applicativo come un servizio.
- Nelle architetture monolitiche tutti i processi sono strettamente collegati tra loro e vengono eseguiti come un singolo servizio. Ciò significa che se un processo dell'applicazione sperimenta un picco nella richiesta, è necessario ridimensionare l'intera architettura.

![[Pasted image 20231123175625.png]]

# ‎ %%← Empty Char%%
___
[[000 Indice PD|↖ Ritorna all'indice ↖]]

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
- [HTML.it](https://www.html.it/pag/16448/cosa-sono-i-web-service/)
- [Micro-servizi AWS](https://aws.amazon.com/it/microservices/)