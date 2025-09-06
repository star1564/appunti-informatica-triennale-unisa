---
aliases: 
tags:
  - corsi/informatica/programmazione_distribuita
paragrafo: Introduzione
cssclasses:
  - 
---
>Il modello di sistemi distribuiti **Basato su Oggetti** (*Distributed Object Computing*) si sviluppa come semplificazione del [[001 Introduzione a OOP|modello orientato agli oggetti]], in modo da consentire una *rappresentazione a oggetti di applicazioni complesse* che siano facilmente realizzabili e distribuibili su reti di grandi dimensioni. 

Il modello mette a disposizione i seguenti costrutti di rappresentazione principali:
- l'**oggetto**, insieme integrato di *dati* (che costituiscono il suo *stato*) e di *funzioni* (che costituiscono il suo *comportamento*) su di essi operanti. Ogni oggetto è dotato di una propria identità che lo differenzia dagli altri a prescindere dallo stato in cui si trova ed è creato come *istanziazione di una classe*;
- l'**interfaccia**, che *raggruppa i servizi messi a disposizione da un oggetto* e invocabili da altri oggetti. Ogni oggetto può possedere una o più interfacce;
- la **connessione** tra due oggetti, che costituisce il meccanismo con il quale i servizi forniti dalle interfacce dell'uno (servente) *sono usati* dall'altro (cliente). Una connessione può essere *sincrona* (bloccante per il cliente) o *asincrona* (non bloccante), di *tipo pull* (il cliente chiede l'esecuzione del servizio) o di tipo *push* (il servente propone attivamente il servizio).

![[Pasted image 20230929162641.png]]

Il modello basato su oggetti è più povero del modello orientato agli oggetti almeno per i seguenti due motivi:
- Non è presente il concetto di [[015 Ereditarietà|Ereditarietà]] fra classi;
- Non è affermata esplicitamente la corrispondenza, tipica della modellazione concettuale dei dati, fra oggetti del modello e oggetti del mondo reale da rappresentare.

> [!example]+ <font color="orange">Esempio</font>
>![[Pasted image 20230929163318.png|Schema a oggetti di un sistema di commercio elettronico]]


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