---
aliases: 
tags:
  - corsi/informatica/ingegneria_software
paragrafo: Testing
cssclasses:
  - 
---
Esistono diverse tecniche per aumentare l'[[035 CVS - Testing#^4fc61b|Affidabilità del Software]].

## Fault Avoidance
Le tecniche di **Fault Avoidance** cercano di rilevare gli errori in modo statico, cioè *senza fare affidamento sull'esecuzione* della componente del sistema. 

Tenta di impedire l'inserimento di [[035 CVS - Testing#^1cc62a|Fault]] nel sistema *prima che venga rilasciato*.

## Fault Tolerance
Le tecniche di **Fault Tolerance** assumono che un sistema possa essere *rilasciato con dei Fault* e che i [[035 CVS - Testing#^bb2926|Fallimenti]] possano essere risolti al runtime attraverso certi [[Terminologie Generali IS#^68fca8|Metodi]].

## Fault Detection
Le tecniche di **Fault Detection** sono svolte durante la [[003 Ciclo di Vita del Software#Ciclo di Sviluppo del Software|fase di sviluppo del software]] con lo scopo di *trovare gli [[035 CVS - Testing#^733a0c|Errori]]*. Non cerca di risolvere i difetti, ma *li identifica solamente*.

Ci sono diversi metodi.

### Review
La **Review** è un *controllo manuale* di alcuni o di tutti gli aspetti del sistema *senza mandarlo in esecuzione*.
Ci sono due tipi:
- **Walkthrough**: lo sviluppatore presenta il codice e la documentazione associata del componente *al team di revisione*. Il team di revisione formula commenti sulla mappatura dell'analisi e dell'[[023 Object Design|Object Design]] sul codice utilizzando il [[017 Requirement Analysis Document|RAD]].
- **Inspection**: è simile al walkthrough, ma la presentazione del codice *non viene fatta dagli sviluppatori*. Il team di revisione controlla le interfacce e il codice delle componenti rispetto ai requisiti. Il team è anche responsabile di controllare l'efficienza degli algoritmi rispetto ai requisiti non funzionali e di controllare se i commenti inseriti sono coerenti rispetto al comportamento del codice.

### Debugging
Il **Debugging** assume che le fault possano essere trovate *a cominciare da un fallimento non previsto*. 

Lo sviluppatore si muove in una successione di stati del sistema, fino ad *arrivare al punto in cui avviene l'errore*. Una volta che l'errore è stato individuato, bisogna identificare il fault e [[037 Concetti di Testing#Correzioni|correggerlo]].

### Testing
Il **[[037 Concetti di Testing|Testing]]** cerca di *creare un Fallimento o uno Stato Erroneo* in un modo pianificato. Questo permette allo sviluppatore di individuare punti in cui il sistema fallisce prima di essere rilasciato.

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