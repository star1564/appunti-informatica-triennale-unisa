---
aliases: 
tags:
  - corsi/informatica/ingegneria_software
paragrafo: Testing
cssclasses:
  - 
---
Una volta [[041 Integration Testing|integrati]] i [[035 CVS - Testing#^ef6c59|componenti]], il **System Testing** assicura che il sistema completato *sia conforme ai [[008 Requisiti#Requisiti Funzionali|Requisiti Funzionali]] e [[008 Requisiti#Requisiti Non Funzionali|Non Funzionali]]*.

Durante il System Testing vengono eseguite diverse attività.

![[Immagine 2024-01-25 185834.png|900]]

## Functional Testing
>Il **Functional Testing** (*Testing dei Requisiti*) trova le differenze del comportamento tra il [[012 Specifica dei Requisiti#Modello Funzionale (Modello dei Casi d'Uso)|Modello Funzionale (dei casi d'uso)]] e quello osservato nel sistema. È di tipo [[037 Concetti di Testing#Blackbox|Blackbox Testing]].

Dato che non è sempre possibile testare tutti i casi d'uso, questo tester *seleziona quelli che sono pertinenti all'utente* e che hanno *un'alta probabilità di trovare una [[035 CVS - Testing#^bb2926|Failure]]*.

La differenza con l'[[039 Usability Testing|Usability Testing]] è che qui vengono trovate le differenze tra il [[012 Specifica dei Requisiti#Modello Funzionale (Modello dei Casi d'Uso)|Modello dei Casi d'Uso]] ed il *comportamento osservato* (invece del modello dei casi d'uso e le aspettative dell'utente).

## Performance Testing
>Il **Performance Testing** trova differenze tra gli [[021 Attività del System Design#1 - Identificare gli Obbiettivi di Progettazione|Obbiettivi di Progettazione]] selezionati durante il [[019 System Design|System Design]] e il sistema. Dato che questi obbiettivi sono ottenuti dai [[008 Requisiti#Requisiti Non Funzionali|Requisiti Non Funzionali]], i [[037 Concetti di Testing#Test Case|Test Case]] possono essere ottenuti dall'[[022 System Design Document|SDD]] o dal [[017 Requirement Analysis Document|RAD]].

Si eseguono varie attività in questo testing:
- **Stress Testing**: controlla se il sistema riesce a *gestire molte richieste simultanee*.
- **Volume Testing**: cerca delle [[035 CVS - Testing#^1cc62a|Fault]] quando il sistema deve *elaborare una grande quantità di dati*.
- **Security Testing**: cerca delle *falle di sicurezza* nel sistema.
- **Timing Testing**: valuta il *tempo di risposta* del sistema.
- **Recovery Test**: valuta l'abilità del sistema di *riprendersi da uno [[035 CVS - Testing#^733a0c|Stato Erroneo]]*.

Dopo che sono stati eseguiti tutti i test funzionali e prestazionali e che non sono stati rilevati guasti durante questi test, il sistema si dice *convalidato*.

## Pilot Testing
>Il **Pilot Testing** avviene quando il sistema è *utilizzato da un insieme selezionato di utenti*. Questi utenti testano il sistema *senza* l'utilizzo di scenari o linee guida.

- Un **Alpha Test** è un test effettuato in *ambiente di sviluppo*.
- Un **Beta Test** è un test effettuato nell'*ambiente di utilizzo*.

## Acceptance Testing
>L'**Acceptance Testing** confronta i [[010 Problem Statement#^9fd154|criteri di accettazione all'interno del problem statement]] con i risultati ottenuti dal sistema completato.

Ci sono tre modi in cui il cliente valuta il sistema durante l'acceptance testing:
- **Benchmark Test**: il cliente prepara una serie di *[[037 Concetti di Testing#Test Case|Test Case]]* su cui il sistema deve operare;
- **Competitor Test**: il sistema viene confrontato e testato *rispetto ad un sistema concorrente*;
- **Shadow Test**: viene testato *il vecchio sistema con quello attuale*, confrontando gli output.

## Installation Testing
Dopo che il sistema viene accettato, viene installato sull'ambiente finale.

>In molti casi l'**Installation Testing** ripete i test case eseguiti durante il [[#Functional Testing|Functional Testing]] ed il [[#Performance Testing|Performance Testing]]. Quando il cliente è soddisfatto, il sistema viene formalmente rilasciato, ed è pronto per l'uso.

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