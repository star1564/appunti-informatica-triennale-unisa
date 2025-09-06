---
aliases:
  - Trasformazione
tags:
  - corsi/informatica/ingegneria_software
paragrafo: 
cssclasses:
  - 
---
>La quarta fase del [[003 Ciclo di Vita del Software|CVS]], l'**Implementazione**, ottimizza e mappa gli oggetti dell'[[023 Object Design|Object Design]] in codice.

![[Pasted image 20240121105706.png|900]]

Durante l'implementazione, gli sviluppatori possono incontrare diversi *problemi di integrazione* (come parametri non documentati). Quindi, il codice che si va ad implementare potrebbe essere lontano dal progetto originario e difficile da comprendere. Per questo, si utilizza un **approccio disciplinato** per evitare la degradazione del sistema.

## Attività per le Trasformazioni
>Una **Trasformazione** mira a *migliorare un aspetto del modello* (ad esempio, la sua modularità) *preservando tutte le altre proprietà* (ad esempio, la sua funzionalità).  Queste trasformazioni avvengono durante numerose attività di progettazione e implementazione degli oggetti.

Ci sono principalmente le seguenti attività:
- ***[[031 Ottimizzare l_Object Design Model|Ottimizzare l'Object Design Model]]***: si *migliora la prestazione* del [[028 Object Design Document|Modello di Object Design]] (come la riduzione delle moltiplicazioni delle associazioni per velocizzare le query);
- ***[[032 Mappare Associazioni a Collezioni|Mappare le Associazioni a delle Collezioni]]***: si esegue il *mapping delle associazioni in costrutti di codice*, in quanto i linguaggi di programmazione non permettono la rappresentazione di associazioni;
- ***[[033 Mappare Contratti in Eccezioni|Mappare i Contratti in Eccezioni]]***: si descrivono i *comportamenti* delle operazioni quando i contratti sono violati (gestione delle eccezioni);
- ***[[034 Mappare il Modello in uno Storage Schema|Mappare il Modello delle Classi in uno Storage Schema]]***: durante il system design, si è scelta un modo per rendere persistenti i dati (come un [[DBMS]] o files), ora bisogna mappare il modello delle classi in uno schema che rappresenta il database.

Queste aiutano con la scrittura del codice e con la creazione di un database.

> [!info] Principi di Trasformazione
>Per evitare che le trasformazioni creino errori, si devono seguire questi principi:
>- Ogni trasformazione deve riguardare un singolo obbiettivo di progettazione;
>- Ogni trasformazione deve essere locale e deve cambiare solo pochi metodi e poche classi;
>- Ogni trasformazione deve essere applicata singolarmente e non devono essere effettuate trasformazioni simultanee.
>- Ogni trasformazione deve essere seguita da una fase di testing.

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