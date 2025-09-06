---
tags:
  - corsi/informatica/reti_calcolatori
paragrafo: Livello Data Link
cssclasses:
  - 
---
### ADSL
L'**ADSL** (*Asymmetric Digital Subscriber Line*) è lo standard per fornire all'abbonato un *accesso digitale* a banda più elevata di quello che poteva supportare il proprio modem. La trasmissione avveniva tramite un modem collegato al [[007 Mezzi Trasmittivi del Livello Fisico#Doppino|doppino telefonico]].

È diviso in:
- *Linea Telefonica*;
- Canali per Traffico Dati in uscita (*upstream*) ed in entrata (*downstream*).

Il modem ADSL riceve i dati da trasmettere e li splitta in *flussi paralleli* da trasmettere sui diversi canali.

![[Pasted image 20230526095307.png]]

---
### ATM
L'**ATM** (*Asynchronous Transfer Mode*) è un'architettura che si basa sulla *commutazione dei pacchetti* e sulla *multiplazione asincrona*. 
Invece di inviare dati in pacchetti variabili, ATM suddivide i dati in unità fisiche chiamate **Celle** di dimensioni costanti (53 byte).

![[Pasted image 20230526102548.png|500]]

Le celle ATM vengono quindi inviate attraverso la rete in *modo indipendente* e *trasparente* (senza controllo di errori). Questo approccio consente una commutazione più rapida ed efficiente dei dati rispetto ai protocolli di commutazione dei pacchetti tradizionali. 

Il principio di commutazione adotta un **instradamento connection-oriented** ed avviene in questo modo:
- Per iniziare il flusso, si invia un messaggio di inizio connessione;
- Stabilita la connessione, tutte le celle *seguono lo stesso cammino*, utilizzando percorsi e canali virtuali;
- La *consegna delle celle non è garantita*, ma lo è *l'ordine di arrivo*.

---
### SONET
**SONET** (*Synchronous Optical Network*) è una tecnologia di rete utilizzata per la trasmissione a lunga distanza di dati su [[007 Mezzi Trasmittivi del Livello Fisico#Fibra Ottica|fibre ottiche]] dagli operatori telefonici nelle loro reti di trasporto.

SONET è basato sulla *sincronizzazione precisa del tempo* tra i nodi di rete. Utilizza una struttura di trame sincronizzate, chiamate **Optical Carrier** (OC), per trasmettere i dati. 

Una delle caratteristiche principali di SONET è la sua *capacità di rilevare e ripristinare automaticamente le interruzioni nella rete*. 
Se si verifica un guasto in una fibra ottica o in un nodo di rete, SONET è in grado di commutare rapidamente il traffico su un *percorso alternativo* senza interrompere la comunicazione. 

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