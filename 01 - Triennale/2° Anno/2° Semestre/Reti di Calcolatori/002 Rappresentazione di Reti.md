---
tags:
  - corsi/informatica/reti_calcolatori
paragrafo: Introduzione
cssclasses:
  - 
aliases: [broadcast]
---
Un modello comune per rappresentare una rete si basa sull'uso dei [[012 Grafi|Grafi]], in cui:

- **Nodi Host**: Questi sono dispositivi autonomi connessi alla rete e servono come *punti di origine e destinazione per le comunicazioni*. Esempi includono computer, telefoni, e altri dispositivi simili. Talvolta sono noti come Communication Endpoint, Network Edge o [[DTE|DTE]]. ^5b1741

- **Collegamenti** (Archi): Questi sono i canali trasmissivi che *collegano* due nodi tra loro. ^2ad4ab

- **Nodi di [[Commutazione di Dati]]**: Questi nodi hanno il compito di riconoscere le richieste per l'apertura di una connessione e di garantire che i dati relativi a tale connessione *raggiungano* la destinazione corretta. Inoltre, agiscono come punti di ridistribuzione dei dati. Questi nodi possono essere chiamati Network [[016 Bridge e Switch#Switch|Switch]], DSE (Data Switching Equipment) o Router. ^1593c0

- [[Protocollo|Protocolli]] (*Software*): Questi costituiscono il software che regola le comunicazioni all'interno della rete.



![[Immagine 2023-03-03 121902.png|650]]

## Modalità di Trasmissione
Ci sono due modalità di trasmissione.
### Punto a punto
>Un circuito fisso è detto **Punto a Punto** quando *collega due soli [[DTE]]*.

![[Pasted image 20230303124637.png]]

Il collegamento punto a punto è spesso utilizzato nella connessione tra due computer oppure in quella tra un computer ed un terminale.

> [!success] Vantaggi
> - *semplicità di gestione*: quello che viene trasmesso da un DTE è sempre diretto all'altro;
> - *tempi di attesa nulli*: il DTE che deve trasmettere trova sempre il circuito disponibile.

> [!failure] Svantaggi
> - il costo di linee molto lunghe su grandi distanze;
> - se bisogna collegare al proprio mainframe $n$ terminali, si dovrebbero installare $n$ linee di collegamento.

#### Multipunto
>Si inseriscono più di due DTE sulla stessa linea.

![[Pasted image 20230303134707.png]]

### Broadcast
>Una **Rete Broadcast** è dotata di *un unico canale di comunicazione* che è condiviso da *tutti* gli elaboratori.

I messaggi (chiamati **pacchetti**) inviati da un elaboratore contengono *indirizzi di destinazione* (che specificano i destinatari).

![[Pasted image 20230303130835.png|400]]

Quando un elaboratore riceve un pacchetto, esamina l'indirizzo di destinazione. Se questo coincide con il proprio indirizzo, il pacchetto viene elaborato, altrimenti viene ignorato.

Viene consentita anche la diffusione di un pacchetto a tutti gli elaboratori presenti sulla rete (*broadcasting*). 
I pacchetti inviati in questo modo non possono essere ignorati.

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




