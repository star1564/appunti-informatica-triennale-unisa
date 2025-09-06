---
tags:
  - corsi/informatica/reti_calcolatori
paragrafo: Introduzione
cssclasses:
  - 
---
Oggi il *software di rete* è strutturato in molti modi diversi.

Per diminuire la complessità, la maggior parte delle reti è organizzata come una pila ([[021 Stack nel C#DEFINIZIONE|Stack]]) di **Livelli**, costruiti uno sull'altro.
Lo scopo di ogni livello è quello di *offrire determinati servizi ai livelli superiori*, senza specificare i dettagli implementativi.

Quando il livello $n$ all'interno di un computer è in comunicazione con il livello $n$ di un altro computer, le regole e le convenzioni usate in questa comunicazione sono globalmente note come **[[Protocollo|Protocolli]] di livello $n$**.

![[Pasted image 20230310150420.png]]
<center>Generalmente ci sono 5 protocolli</center>

In realtà i dati non sono trasferiti direttamente dal livello $n$ di un computer al livello $n$ di un altro.

>Infatti, ogni livello *passa dati e informazioni di controllo a quello immediatamente sottostante*, fino a raggiungere il più basso.

Sotto al livello 1 si trova il **Supporto Fisico** attraverso cui è possibile la comunicazione vera e propria.

Tra ciascuna coppia di livelli contigui si trova un'*interfaccia* che definisce le operazioni elementari e i servizi che il livello inferiore rende disponibili a quello soprastante.

L'elenco dei protocolli usati da uno specifico sistema è chiamato **Pila di Protocolli**.

> [!example]- <font color="orange">Esempio</font>
> Un filosofo (che parla solo inglese) vuole mandare un messaggio ad un altro filosofo (che parla solo francese). Quindi, i filosofi assumono due interpreti che decidono come lingua neutra nota a entrambi l'olandese. Il primo interprete riceve e traduce il messaggio, per poi mandarlo ad una segretaria, che invia un fax ad un'altra segretaria. La seconda segretaria da il messaggio al secondo interprete che traduce il messaggio in francese.
> ![[Pasted image 20230310152408.png]]



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