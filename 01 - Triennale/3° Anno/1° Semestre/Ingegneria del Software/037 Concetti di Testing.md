---
aliases: 
- Test Case
- Blackbox Testing
- Whitebox Testing
- Test Stub
- Test Driver
- Testing di Unità (Unit Testing)
- Testing Strutturale
- Testing Funzionale
- Testing di Performance
tags:
  - corsi/informatica/ingegneria_software
paragrafo: Testing
cssclasses:
  - 
---
## Test Case
>Un **Test Case** è un *insieme di dati di input e di risultati attesi* che esercitano una componente con lo scopo di causare [[035 CVS - Testing#^bb2926|Fallimenti]] e rilevare [[035 CVS - Testing#^1cc62a|Faults]]. 
>Diversi test case compongono un [[035 CVS - Testing#^f152ed|Test]].

Ha i seguenti attributi:
- **Nome**: determinare il nome a partire dal nome del [[035 CVS - Testing#^ef6c59|Componente di Testing]] o dal requisito che si sta testando;
- **Location**: descrive dove il test case può essere trovato (un path oppure un URL);
- **Input**: descrive l'insieme di dati in input o comandi che [[005 UML#Attori|Attore]] del test case deve inserire;
- **Oracolo**: conosce il comportamento atteso dal test case, ovvero la sequenza di dati in output che la corretta esecuzione del test dovrebbe produrre;
- **Log**: risultati di varie esecuzioni del test.

### Relazione dei Test Case
Una volta identificati e descritti i test case, si possono determinare le relazioni:
- **Aggregazione**: usata quando il test case può essere *scomposto in un insieme di sotto-test*.
- **Precedenza**: usata per indicare *l'ordine di testing*, dove un test può precedere un altro test.

![[Pasted image 20240125160342.png|500]]

## Blackbox e Whitebox Test
Sono due modi per testare.
![[Pasted image 20240125161658.png|500]]
### Blackbox
>Un **Blackbox Test** si concentra *solamente sull'input e sull'output del componente*.

I Blackbox Test non riguardano gli aspetti interni del componente, né il comportamento o la struttura dei componenti.

### Whitebox
>Un **Whitebox Test** si concentra *solamente sulla struttura interna del componente*.

Un Whitebox Test garantisce che, indipendentemente dal particolare comportamento di input/output, venga testato ogni stato nel [[016 Modello Dinamico|modello dinamico]] dell'oggetto e ogni interazione tra gli oggetti. 

Di conseguenza, i test Whitebox vanno oltre i test Blackbox. Infatti, la maggior parte dei test Whitebox richiedono dati di input che non potrebbero essere derivati dalla sola descrizione dei [[008 Requisiti#Requisiti Funzionali|Requisiti Funzionali]].

## Test Stub e Drivers
I Test driver e gli Stub vengono utilizzati per sostituire le *parti mancanti* del sistema.

> [!note] Cambiamento dell'interfaccia del componente
> Se l'interfaccia di un componente da testare cambia, devono cambiare anche i test driver e gli stub corrispondenti.

### Test Driver
>Un **Test Driver** simula la *parte del sistema che invoca il componente da testare*. 

Un test driver trasmette al componente gli input di test identificati nell'analisi del test case e visualizza i risultati.

### Test Stub
>Un **Test Stub** simula un *componente invocato dal componente da testare*. 

Lo stub di test deve fornire la stessa API del metodo del componente simulato e deve restituire un valore conforme al tipo di risultato restituito della firma del tipo del metodo. 

## Correzioni
Una volta che i test sono conclusi e sono state trovati dei [[035 CVS - Testing#^bb2926|Fallimenti]], gli sviluppatori effettuano una correzione del componente.

>La **Correzione** è una *modifica al componente testato* per eliminare un [[035 CVS - Testing#^1cc62a|Fault]]. Una correzione *può anche introdurre nuovi problemi*.

Per ridurre la probabilità di incontrare nuovi errori con una correzione, si può fare:
- **Problem Tracking**: serve per tenere traccia degli [[035 CVS - Testing#^733a0c|Errori]], Fallimenti e Fault individuati e *le relative soluzioni*.
- **Regression Testing**: vengono *rieseguiti i test precedenti* non appena viene corretta una componente.
- **Rationale Maintenance**: si specificano i *motivi di una correzione* o per lo sviluppo di una nuova componente.

## Attività di Testing
Ci sono delle specifiche attività nel testing:
- [[038 Component Inspection|Component Inspection]]
- [[039 Usability Testing|Usability Testing]]
- [[040 Unit Testing|Unit Testing]]
- [[041 Integration Testing|Integration Testing]]
- [[042 System Testing|System Testing]]



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