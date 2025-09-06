---
tags:
  - corsi/informatica/reti_calcolatori
paragrafo: Livello Data Link
cssclasses:
  - 
---
Il livello [[008 Livello Data Link|Data Link]] gestisce due tipi di collegamenti:
- [[002 Rappresentazione di Reti#Punto a punto|Punto a punto]];
- [[002 Rappresentazione di Reti#Broadcast|Broadcast]].

Nel tipo di broadcast, quando una [[006 Livello Fisico#^bdfb6b|trama]] viene inviata sul mezzo condiviso, *tutti i dispositivi collegati* a quella [[LAN]] la possono ricevere. 
Pertanto ogni scheda di rete (NIC), confronta il suo [[MAC (Indirizzo e Sottolivello)#Indirizzo MAC|indirizzo MAC]] con quello di destinazione del pacchetto. Nel caso i *MAC address coincidano*, il NIC corrispondente accetterà il pacchetto anziché ignorarlo.

Possono però accadere [[Interferenza e Collisione|Interferenze e Collisioni]].

## Protocolli ad accesso multiplo
> I [[Protocollo|Protocolli]] che regolano la trasmissione in modo da non far presentare collisioni sono detti **Protocolli ad accesso multiplo**. Tali protocolli regolano anche la velocità con cui i dispositivi possono comunicare: se un solo nodo deve comunicare, può farlo con una velocità pari a $K$ b/s; se $N$ nodi devono comunicare, possono farlo con una velocità pari a $\frac{K}{N}$ b/s ciascuno. 

Tali protocolli sono suddivisi in tre categorie:
- **[[012 Protocolli a suddivisione del canale|Protocolli a suddivisione del canale]]**: il canale viene suddiviso in *parti più piccole*;
- **[[013 Protocolli ad accesso casuale|Protocolli ad accesso casuale]]**: ogni dispositivo ha il permesso di accedere al canale *in qualsiasi momento* (*se è libero*) e trasmettere dati. Rischio di interferenze e collisioni;
- **[[014 Protocolli a rotazione|Protocolli a rotazione (collision free)]]**: ciascun nodo ha il suo *turno per trasmettere*, il quale viene regolato in base alla quantità di dati da trasmettere.

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