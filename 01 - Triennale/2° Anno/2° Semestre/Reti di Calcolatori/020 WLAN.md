---
tags:
  - corsi/informatica/reti_calcolatori
paragrafo: Livello Data Link
cssclasses:
  - 
---
> Una **WLAN** (*Wireless [[LAN|Local Area Network]]*) è una rete locale in cui i nodi comunicano tra di loro attraverso il [[007 Mezzi Trasmittivi del Livello Fisico#Radiodiffuzione|canale radio]] ed è utilizzata dove risulta difficoltoso l'uso di cavi.

![[Pasted image 20230526091911.png]]

Questo sistema di comunicazione dati è molto flessibile e può essere usato come estensione o alternativa alle normali LAN su cavo.

> [!success] Vantaggi
> *Mobilità*: Gli utenti possono accedere alle risorse di rete da qualsiasi posizione senza doversi collegare a una presa;
> *Velocità e semplicità di installazione*: Non c'è bisogno di installare cavi;
> *Costi*: Le spese per installare una WLAN sono molto inferiori alla LAN cablata;
> *Scalabilità*: La configurazione può essere modificata facilmente per soddisfare esigenze diverse;

Le bande utilizzate nelle trasmissioni wireless sono a *2.4 GHz* ed a *5 GHz*.

## Architettura delle WLAN IEEE 802.11
L'architettura IEEE 802.11 è costituita da diversi componenti e servizi che interagiscono tra di loro.

Il *componente base* è la **Stazione**: è una qualsiasi unità che contiene le funzionalità del protocollo 802.11.
- Queste stazioni possono essere *mobili* (telefoni), *portatili* (PC o laptop) o *stazionarie* (access point);
- Un insieme di stazioni costituisce un **Basic Service Set** (BSS).

Esistono due modalità di funzionamento:
- Independent Basic Service Set (IBSS) o *Ad Hoc Network*;
- Infrastructure Basic Service Set o *Infrastructure Mode*.

![[Pasted image 20230526093618.png|600]]

### Ad Hoc Network
È la tipologia più semplice dove un insieme di stazioni si sono identificate tra di loro e sono interconnesse in modalità [[Modello Peer-to-Peer|Peer-to-Peer]].
Una stazione è raggiungibile solo se situata entro un *raggio di copertura*.

Quindi le stazioni *comunicano direttamente tra loro* e non ci sono funzioni di relay.

### Infrastructure Mode
È una BSS con un componente chiamato **Access Point** che fornisce la funzione di *relay*. La comunicazione tra le stazioni avviene *solo attraverso l'Access Point*.

Il sistema è diviso in celle costituite dalle BSS, dove ciascuna cella è controllata dall'Access Point.

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