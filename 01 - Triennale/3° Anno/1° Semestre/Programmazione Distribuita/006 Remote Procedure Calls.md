---
aliases: RPC
tags:
  - corsi/informatica/programmazione_distribuita
paragrafo: Middleware
cssclasses:
  - 
---
>La computazione [[004 Sistemi a Oggetti Distribuiti|distribuita basata su oggetti]] si fonda sulla astrazione del **Remote Procedure Call** (*RPC*), un modello considerato come predecessore del [[005 Middleware|Middleware]] che permette a un programma di *eseguire una procedura (funzione) su un calcolatore diverso* da quello ospitante il programma chiamante, proprio come se fossero in locale.

L'obiettivo del RPC era semplificare il lavoro del progettista o dello sviluppatore in modo che non dovessero preoccuparsi se un nodo fosse locale o remoto. Ciò ha permesso:
- di trasmettere dati usando flussi di byte attraverso [[032 Porte#Socket|Socket]], in modo codificato e standardizzato in modo che dall'altra parte della comunicazione i dati fossero ricostruiti come erano stati trasmessi (chiamato *marshalling*).
- di superare le differenze nella rappresentazione dei dati da trasmettere o nella rappresentazione di stringhe di caratteri (noto come *data representation*).

![[Pasted image 20230929171731.png|600]]

Quando un programma principale chiama una procedura remota tramite RPC, il processo è simile a chiamare una procedura locale nel linguaggio di programmazione utilizzato. La procedura remota ha un nome, una lista di parametri in ingresso e una lista di parametri in uscita. 

1. Quando viene chiamata tramite RPC, viene effettivamente invocato un modulo locale chiamato *client stub* con lo *stesso nome e gli stessi parametri*. Il client stub rappresenta localmente la procedura remota, nascondendo la complessità dell'invocazione fisica al programma chiamante. Il client stub prende i valori dei parametri in ingresso dal programma chiamante, *li impacchetta in un messaggio* attraverso il **marshalling** e aggiunge il nome della procedura. Questo messaggio viene quindi trasmesso al sistema che ospita il programma chiamato tramite il [[030 TCP|protocollo di comunicazione]]. 
‎ %%← Empty Char%%
2. Nel sistema ospitante, il messaggio viene interpretato per individuare la procedura da eseguire, grazie a un modulo chiamato *dispatcher*. Il dispatcher invoca quindi il *server stub* della procedura richiesta, passando il resto del messaggio ricevuto. Il server stub *estrae i dati dal messaggio* attraverso l'**unmarshalling** e chiama la procedura richiesta con i valori dei parametri in ingresso. L'esecuzione della procedura restituisce al server stub i valori dei parametri in uscita previsti.
‎ %%← Empty Char%%
3. Tali valori sono impacchettati e ritrasmessi al sistema chiamante, che li ricostruisce e li restituisce al programma chiamante.

RPC imponeva che fosse rispettata la *sincronia della invocazione*, bloccando il client fino a quando il server non avesse risposto alla invocazione remota di procedura

> [!failure] Svantaggi
> - RPC è un paradigma procedurale e non a oggetti;
> - I tipi di dati sono solamente elementari;
> - Mancanza della gestione delle eccezioni di malfunzionamento sul canale che non blocca la computazione;
> - Invocazione concorrente di più programmi.

A scapito dei suoi svantaggi e del fatto che è superato dal middleware, RPC rimane ancora utilizzato per sistemi critici del funzionamento di internet (come il [[036 DNS|DNS]]).

## Passaggio da RPC al Middleware
Il modello RPC è stato esteso per permettere l'*invocazione di metodi di oggetti remoti*. Questa estensione va oltre una semplice chiamata a procedure sugli oggetti, infatti integra le caratteristiche della programmazione orientata agli oggetti come il [[016 Polimorfismo|Polimorfismo]], l'[[015 Ereditarietà|Ereditarietà]] e la [[023 Gestione delle Eccezioni|gestione delle eccezioni]].

Il passaggio dai Remote Procedure Call agli Oggetti Distribuiti *è stato guidato dalla complessità crescente dei sistemi distribuiti*, che diventavano sempre più ampi, complessi, eterogenei e difficili da gestire.

Il modello a oggetti distribuiti offre componenti e ambienti riutilizzabili che si integrano nelle architetture software e nei design pattern. Di conseguenza, fornisce un modo efficiente per effettuare chiamate di metodi su oggetti attraverso la rete, utilizzando nodi diversi con diverse caratteristiche hardware e software.

Alcuni esempi di soluzioni basate su oggetti distribuiti sono:
- **CORBA**, *[[005 Middleware#Middleware Esplicito|middleware esplicito]]*: 
	- Era una tecnologia di middleware che permetteva agli oggetti distribuiti di comunicare in un sistema distribuito attraverso interfacce IDL e un Object Request Broker (ORB), semplificando l'invocazione di metodi tra oggetti distribuiti come se fossero locali.
- **Java Remote Method Invocation** ([[017 Java RMI|Java RMI]]), *middleware esplicito*: 
	- La soluzione della Sun Corporation utilizzata in Java.
- **Microsoft .NET**, [[005 Middleware#Middleware Esplicito ed Implicito|middleware sia esplicito che implicito]] in base a come lo si usa: 
	- La soluzione della Microsoft, utilizzata per i suoi linguaggi di programmazione come C# e Visual Basic.




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