---
aliases:
  - Software Pattern
  - Repository
  - MVC
  - Three-Tier
  - Pipe e Filter
tags:
  - corsi/informatica/ingegneria_software
paragrafo: Progettazione - System Design
cssclasses:
  - 
---
>L'**Architettura di un Software** include dei *pattern specifici* di costruzione di un sistema. Questo specifica:
>- Il tipo di decomposizione del sistema;
>- Il controllo del flusso globale;
>- Le condizioni di boundary;
>- I [[Protocollo|Protocolli]] di comunicazione tra i sottosistemi.

Esistono diversi tipi di architetture software (o *software pattern*).

## Repository
>Nel pattern architetturale a **Repository**, i sottosistemi *accedono e modificano un'unica struttura di dati* chiamata **repository centrale**. 
>I sottosistemi sono relativamente indipendenti e interagiscono solo attraverso la repository.

![[Pasted image 20240119103728.png|700]]

Il flusso di controllo viene gestito dalla repository dopo un *cambiamento dei dati* oppure dai sottosistemi tramite meccanismi di [[011 Metodi Sincronizzati|sincronizzazione]] o di [[012 Lock Intrinseci e Istruzioni Sincronizzate|lock]].

> [!success] Vantaggi
> Utile quando i dati cambiano frequentemente, in quanto si evitano incoerenze.

> [!failure] Svantaggi
> - La repository può diventare un collo di bottiglia in termini di prestazioni e di modificabilità.
> - Si ha un [[019 System Design#Coupling|Alto Coupling]] tra i sottosistemi, quindi è difficile cambiare la repository senza impattare tutti i sottosistemi.

## Model-View-Controller (MVC)
>Nel pattern architetturale **MVC**, i [[019 System Design#Sottosistema|sottosistemi]] sono di tre tipologie: Model, View e Controller.

Più precisamente:
- **Model**: rappresenta i *dati e la [[Logica di business]]* dell'applicazione.  ^c2d884
	- È responsabile di archiviare, recuperare e manipolare i dati.
	- In genere è implementato come una classe o un insieme di classi.
- **View**: permette di *mostrare i dati all'utente*. ^f67cb8
	- È responsabile dell'UI che l'utente visualizza.
	- In genere è implementato come una pagina [[006 Basi di HTML|HTML]] per un sito web oppure una [[026 Finestra Semplice|GUI]] in un programma.
- **Controller**: *gestisce la sequenza di interazioni* con l'utente e *aggiorna* il Model o la View. ^efc839
	- È responsabile di ricevere le richieste dall'utente, di elaborarle e di rispondere.
	- In genere è implementato come una classe o un insieme di classi.

![[Pasted image 20240119104445.png|500]]

> [!example]- <font color="orange">Esempio</font>
>Consideriamo il caso in cui l'utente invia una richiesta HTTP per visualizzare l'elenco di tutti i prodotti di un sito. 
>Il Controller riceve la richiesta e la elabora. Poi va a recuperare i dati dal Model e li passerebbe alla View. La View quindi visualizzerebbe i dati in una tabella.

L'MVC è un tipo particolare di [[#Repository|repository]], dove:
- Il Model implementa la struttura dati centrale;
- Il Controller gestisce il flusso di controllo.

## Client-Server
>Nel pattern architetturale **[[Modello Server-Client|Client-Server]]**, un sottosistema, il *[[Server]]*, fornisce dei servizi ad *istanze di sottosistemi*, chiamati *[[Client]]*.

![[Pasted image 20240119110727.png|800]]

## Peer-to-Peer
>Il pattern architetturale **[[Modello Peer-to-Peer|Peer-to-Peer]]** è una generalizzazione del pattern Client-Server, dove *i client possono essere server e viceversa*.

![[Pasted image 20240119110932.png|400]]

Non è molto efficace, dato che sono più difficili da progettare e possono introdurre situazioni di [[Deadlock]].

## Three-Tier
>Il pattern architetturale **Three-Tier** organizza i *sottosistemi in tre livelli*.

- *Livello di interfaccia*: include tutti gli oggetti che sono responsabili di *presentare* ed *ottenere* dati dall'utente ([[Logica di presentazione]]), come gli [[014 Modello ad Oggetti|oggetti Boundary]] ed [[014 Modello ad Oggetti|oggetti Control]]. Contiene molto spesso al suo interno la configurazione [[#Model-View-Controller (MVC)|MVC]]. ^37b01a
- *Livello di Logica dell'applicazione*: si occupa della [[Logica di business]]. ^0a5e48
- *Livello di Storage*: si occupa di archiviare, recuperare e manipolare i dati all'interno di un database.

![[Pasted image 20240209095725.png]]

## Pipe e Filter
>Il pattern architetturale **Pipe e Filter** è utile se si deve trasformare un insieme di dati presi di input in una serie di dati di output *attraverso una catena di sottosistemi*.

![[Pasted image 20240119113415.png|700]]

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