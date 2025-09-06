---
aliases: 
tags:
  - corsi/informatica/programmazione_distribuita
paragrafo: Java RMI
cssclasses:
  - 
---
Il sistema di [[017 Java RMI|Java RMI]] è strutturato su tre livelli:
- *Stub/Skeleton Layer*, comprende gli stub lato client e gli skeleton lato server;
- *Remote Reference Layer*, specifica il comportamento della invocazione e la [[003 Fasi dello Sviluppo del Programma#SINTATTICA E SEMANTICA|semantica]] del riferimento ([[017 Java RMI#Unicast e Multicast|unicast, multicast]]);
- *Transport Layer*, si occupa della connessione e della sua gestione.

![[Pasted image 20231014110859.png|550]]

L'applicazione dell'utente si trova in cima a questi livelli ed interagisce (in maniera moderata) con il livello di stub/skeleton.

### Stub/Skeleton Layer
1. Lo *Stub* inizia la connessione con la macchina virtuale remota attraverso il Remote Reference Layer;
2. Effettua il Marshalling dei parametri del metodo verso il Remote Reference Layer;
3. Attende il risultato della invocazione;
4. Lo *Skeleton*, quando riceve una invocazione, effettua l'Unmarshalling dal Remote Reference Layer dei parametri;
5. Invoca il metodo dell'oggetto server passando i parametri;
6. Effettua il Marshalling del valore restituito dal metodo;
7. Lo *Stub* effettua l'unmarshalling del risultato;
8. Restituisce il risultato all'oggetto client.

### Remote Reference Layer
Questo layer si occupa di interfacciare il livello di trasporto con quello di stub/skeleton, occupandosi della parte di [[Protocollo]], che può essere [[017 Java RMI#Unicast e Multicast|unicast, multicast]] oppure oggetti attivabili.

### Transport Layer
Tutto il meccanismo di [[005 Modello ISO-OSI|trasporto attraverso la rete]].


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