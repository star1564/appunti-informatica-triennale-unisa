---
aliases: 
tags:
  - corsi/informatica/programmazione_distribuita
paragrafo: Cloud Computing
cssclasses:
  - 
---
I servizi offerti dal cloud possono essere divisi in tre categorie principali:

- **Infrastructure as a Service** (*IaaS*): l'utente può noleggiare un calcolatore dove può personalizzare il *[[001 Introduzione a SO|sistema operativo]]*, sistemi di storage, applicazioni e i componenti della rete, ma *non controlla direttamente l'infrastruttura cloud*. Praticamente ha a disposizione un computer da configurare (come una macchina virtuale).

- **Platform as a Service** (*PaaS*): permette di distribuire e gestire *l'esecuzione di un'applicazione* utilizzando risorse fornite dal provider cloud tramite un ambiente adeguato (come un server per un gioco).

- **Software as a Service** (*SaaS*): permette di *utilizzare software tramite un interfaccia web* (come Google Docs o Word Online).

![[Pasted image 20231223115811.png]]

![[Pasted image 20231223120926.png]]

## Cloud Pubblici, Privati ed Ibridi
- *Cloud Pubblici*: costruito su internet, può essere utilizzato da chiunque pagando una certa quota mensile o giornaliera.
- *Cloud Privati*: costruito all'interno di un dominio o all'interno di un intranet in possesso di una singola organizzazione, viene utilizzato per la gestione interna e per il funzionamento della stessa.
- *Cloud ibridi*: utilizzato sia per scopi interno ad un azienda o a un organizzazione che per offrire servizi al pubblico tramite internet.

![[Pasted image 20231223121824.png]]

## Cloud Computing Economics 
Il cloud computing si rileva estremamente conveniente dal punto di vista **economico** in alcuni casi, per esempio quando la domanda per un servizio varia nel tempo.

Infatti, se gli host del servizio decidono di *gestire i picchi di carico*, si ritroveranno inevitabilmente ad acquistare delle macchine che per la maggior parte del tempo rimarranno *inutilizzate* (area verde).
Invece, se decidono di *ignorare i picchi*, con il passare del tempo, gli utenti, trovando un servizio lento e inaffidabile, *migreranno altrove con conseguente perdita di ricavi*. 

![[Pasted image 20231223122419.png]]

Altri vantaggi si hanno in situazioni in cui non è ben nota a priori la quantità di traffico che il servizio si ritroverà a gestire, problema che viene risolto dalla *rapida scalabilità* (allocando altre macchine), che il cloud possiede. 

Infine, un'altra situazione in cui il cloud è molto utile è quando ci troviamo ad avere bisogno di semplice e pura potenza di calcolo, che il cloud non ha difficoltà a offrire (evitando alti costi per l'acquisto di un gran numero di macchine performanti, l'utente paga quanto consuma).



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