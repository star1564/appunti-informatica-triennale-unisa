---
tags:
  - corsi/informatica/reti_calcolatori
paragrafo: Introduzione
cssclasses:
  - 
---
Una **Tipologia di Rete** è una configurazione geometrica dei [[002 Rappresentazione di Reti|collegamenti]] tra i vari componenti della rete.
Le varie tipologie sono legate al conseguimento dei loro obbiettivi fondamentali.

### Rete Gerarchica o ad Albero
Quella ad [[013 Alberi|albero]] è tipo di configurazione più comune.

![[Pasted image 20230303131623.png]]

>Il traffico di dati ([[Flusso Trasmissivo|flusso o trasmissione]]) va dai nodi dei livelli più bassi, verso i nodi intermedi o più alti.

I nodi più in alto (o radice) sono quelli *più potenti* dell'intera struttura e sono spesso responsabili della gestione della rete.

> [!failure] Svantaggi
> - Il nodo principale, se è sovraccarico di lavoro, può diventare un collo di bottiglia per l'intera rete, il che comporta un rallentamento dei servizi per tutti gli utenti.
> - La caduta del nodo principale rende inutilizzabile l'intera rete.
> 	- Questo può essere risolto con la *politica di back-up*, dove ci sono altri nodi della rete che svolgono le stesse funzioni del nodo principale in caso questo venisse a mancare.

---
### Rete a Stella
Questa configurazione è simile a quella ad albero, ma non c'è alcuna distribuzione funzionale (ovvero non ci sono livelli diversi).
>Tutte le funzioni riguardanti gli utenti periferici sono realizzate nel nodo centrale.

![[Pasted image 20230303132243.png]]
> [!success] Vantaggi
> Rete di accesso semplice.

> [!failure] Svantaggi
> Presenta gli stessi svantaggi (e risoluzioni) della rete ad albero.

---
### Rete a Dorsale o Bus Condiviso
In questa configurazione (diventata popolare con le [[LAN]] di tipo Ethernet) si ha un unico cavo ([[Bus]]) che collega tutte le stazioni.

![[Pasted image 20230303132634.png]]

>La trasmissione di una stazione viene ricevuta da tutte le altre.
>In ogni istante solo un elaboratore può trasmettere, mentre gli altri devono astenersi.

È necessario un meccanismo di *arbitraggio* per risolvere i conflitti quando due o più elaboratori vogliono trasmettere contemporaneamente.

> [!success] Vantaggi
> Rete di accesso molto semplice.

> [!failure] Svantaggi
> - Un unico portante trasmissivo serve tutte le stazioni, pertanto una sua eventuale interruzione mette fuori uso tutta la rete.
> - La mancanza di punti di concentrazione rende difficoltosa l'individuazione di eventuali punti di malfunzionamento.

---
### Rete ad Anello
Questa configurazione è stata resa popolare dalle prime [[LAN]] di tipo Token-Ring o FDDI.

![[Pasted image 20230303133424.png]]

>La trasmissione è unidirezionale, ovvero i dati viaggiano in un *solo senso*.

Anche qui c'è bisogno di un meccanismo di *arbitraggio*.

> [!success] Vantaggi
> Apre ottime prospettive per l'utilizzo della [[007 Mezzi Trasmittivi del Livello Fisico#Fibra Ottica|fibra ottica]].

> [!failure] Svantaggi
> Dato che la trasmissione è unidirezionale, se bisogna mandare dei dati ad una postazione vicina ma dal verso opposto, bisogna far circumnavigare i dati nell'anello.

---
### Rete a Maglia o Mesh
>Questa configurazione consiste nel collegare tra di loro le varie stazioni *con diversi circuiti*.

![[Pasted image 20230303133945.png]]

> [!success] Vantaggi
> - Questa tipologia assicura buone prestazioni, in quanto il traffico viene ripartito sui vari percorsi.
> - Essa conferisce una elevata affidabilità all'intera struttura attraverso la presenza di *percorsi multipli*.

> [!failure] Svantaggi
> - I costi dei collegamenti possono essere elevati.
> - Gestione della struttura molto complessa.


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