---
aliases:
  - Middleware
tags:
  - corsi/informatica/programmazione_distribuita
paragrafo: Middleware
cssclasses:
  - 
---
I [[004 Sistemi a Oggetti Distribuiti|sistemi distribuiti basati su Oggetti Distribuiti]] si trovano all'incrocio tra due aree della tecnologia del software: i *sistemi distribuiti*, che cercano di unire risorse da nodi diversi in una rete, e lo *sviluppo orientato agli oggetti*, che mira a semplificare i sistemi software e a creare parti di software riutilizzabili.

Gli oggetti distribuiti mirano a creare servizi distribuiti riutilizzabili che siano efficienti, flessibili, sicuri e affidabili. Fanno ciò utilizzando nodi diversi in una rete, che possono variare sia per hardware che per software. 

>Il **Middleware** ad oggetti distribuiti è un elemento chiave che *si trova tra le applicazioni utente e l'infrastruttura di rete e hardware*. Questo middleware aiuta le componenti del sistema a comunicare e cooperare tra loro.

![[Pasted image 20230929163843.png|600]]

La comunicazione tra i nodi potrebbe avvenire attraverso [[005 Chiamate di Sistema|System Call]] di basso livello offerti dal sistema operativo di ciascun nodo. Tuttavia, questo sarebbe *troppo complicato e dispendioso* in termini di tempo e risorse. I programmatori dovrebbero in questo caso occuparsi di dettagli tecnici complessi, come la conversione dei dati in stream di byte per la trasmissione su una rete, gestendo allo stesso tempo le *differenze tra le architetture hardware*.

Il middleware ha lo scopo di semplificare queste attività e fornisce agli sviluppatori strumenti più accessibili che si integrano bene con le loro abilità tradizionali per creare applicazioni.

## Middleware Esplicito ed Implicito
Il middleware può essere suddiviso in due categorie principali.
### Middleware Implicito 
Questo tipo di middleware opera in modo trasparente e *automatico*, senza richiedere alcuna azione specifica da parte degli sviluppatori o degli utenti del sistema distribuito. Si occupa di aspetti come la gestione della comunicazione, la sicurezza e la gestione dei dati *senza richiedere intervento diretto*. Un esempio è il middleware per la gestione della concorrenza in sistemi distribuiti.

![[Pasted image 20230929180344.png]]
### Middleware Esplicito 
Il middleware esplicito *richiede un intervento diretto da parte degli sviluppatori o degli utenti* per specificare come deve avvenire la comunicazione o altre operazioni di sistema. Gli sviluppatori devono utilizzare API o strumenti specifici forniti dal middleware per gestire le operazioni desiderate. ^5c5856

![[Pasted image 20230929180101.png|500]]

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