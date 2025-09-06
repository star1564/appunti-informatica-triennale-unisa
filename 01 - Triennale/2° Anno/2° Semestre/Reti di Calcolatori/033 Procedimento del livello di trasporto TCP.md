---
aliases: 
tags:
  - corsi/informatica/reti_calcolatori
paragrafo: Livello Trasporto
cssclasses:
  - 
---

Ognuna di queste fasi svolge un ruolo fondamentale nella trasmissione di dati in modo affidabile attraverso la rete con [[030 TCP|TCP]].

![[Pasted image 20230908174939.png|650]]
## Segmentazione e Ricostruzione dei Dati
>Il livello di trasporto prende i dati dal [[035 Livello di Applicazione|livello superiore]] provenienti da un [[Processo (Job)|Processo]] e li *suddivide* in segmenti più piccoli, noti come **pacchetti**.
>
>Alla destinazione, il livello di trasporto *ricostruirà* questi segmenti per formare nuovamente i dati originali.

Questa suddivisione è importante per il trasporto efficiente dei dati attraverso la rete, soprattutto quando si tratta di dati di grandi dimensioni.

## Controllo del Flusso e degli Errori
>Il livello di trasporto implementa il **[[030 TCP#Controllo del flusso in TCP|controllo del flusso]]** per evitare che un mittente trasmetta dati più velocemente di quanto il destinatario sia in grado di elaborarli. 

Ciò evita il congestionamento della rete e garantisce che i dati vengano consegnati in modo affidabile.

>Include inoltre meccanismi per **rilevare e correggere** gli errori nei dati trasmessi, come un [[009 Rilevazione Errori nel Data Link#Checksum|Checksum]].

## Multiplexing e Demultiplexing
Ogni dispositivo finale nella rete può avere più di una comunicazione in corso contemporaneamente. Quindi, il livello di trasporto utilizza il concetto di [[032 Porte#Porta|Porte]] per distinguere tra diverse comunicazioni. 

Ogni pacchetto ha al suo interno una combinazione di *indirizzo IP e numero di porta* sia della macchina di origine che quella di destinazione, il che consente di instradare correttamente i dati all'applicazione appropriata sul dispositivo di destinazione.

![[Pasted image 20230908174656.png|650]]
<center>Nota che i sender e receiver sono dei socket</center>


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