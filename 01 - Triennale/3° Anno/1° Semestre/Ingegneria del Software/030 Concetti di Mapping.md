---
aliases:
  - Refactoring
tags:
  - corsi/informatica/ingegneria_software
paragrafo: Implementazione
cssclasses:
  - 
---
>Il **Mapping**, in programmazione, significa *tradurre (o associare) un concetto ad un altro concetto*.

Esistono quattro tipi di [[029 CVS - Implementazione#Attività per le Trasformazioni|Trasformazioni]] riguardo il mapping.

## Trasformazione del Modello

>Una **Trasformazione del Modello** opera solamente sul [[028 Object Design Document|Modello di Object Design]] e non sul codice, viene applicata a un modello di oggetti e *dà come risultato un altro modello di oggetti*. 
>Lo scopo della trasformazione del modello a oggetti è quello di *semplificare o ottimizzare il modello originale*, rendendolo più conforme a tutti i requisiti della specifica. 

Una trasformazione può aggiungere, rimuovere o rinominare classi, operazioni, associazioni o attributi. Una trasformazione può anche aggiungere informazioni al modello o rimuoverle.

![[Pasted image 20240125102900.png|500]]

## Refactoring
>Il **Refactoring** serve per *trasformare il codice sorgente per migliorarne la leggibilità* o modificabilità *senza cambiare il comportamento* del sistema. Si concentra sul migliorare il design di specifici attributi o metodi di una classe.

Per essere sicuri che il refactoring non cambi il comportamento del sistema, questa attività è eseguita in *piccoli step incrementali* intervallati da vari *test*.

> [!example]+ <font color="orange">Esempio</font>
>Esegui il refactoring per il campo email.
>1. Ispeziona `Player`, `LeagueOwner` e `Advertiser` per essere sicuri che il campo dell'email sia equivalente. Rinominare i campi equivalenti in `email` se necessario.
>2. Crea la classe pubblica `User`.
>3. Fai in modo che `Player`, `LeagueOwner` e `Advertiser` estendano la classe `User`.
>4. Aggiungi un campo [[Tipi, Signatures e Visibilità (IS)#^3e550d|protected]] chiamato `email` all'interno di `User`.
>5. Rimuovi i campi di email da `Player`, `LeagueOwner` e `Advertiser`.
>6. Compila e testa.
>
>![[Pasted image 20240125103853.png]]

Quello che è stato appena descritto è un processo manuale.
La maggior parte dei tool automatici di refactoring sugli IDE permette solamente di rinominare variabili o la cancellare files.

## Forward Engineering
>Il **Forward Engineering** produce un *template del codice sorgente* corrispondente al modello a oggetti. Come input possiede un insieme di elementi del modello a oggetti, come output ha un insieme di istruzioni di codice sorgente.

Gli obbiettivi sono:
- Mantenere una corrispondenza fra il modello di design ad oggetti ed il codice;
- Ridurre il numero di errori introdotti durante l'implementazione;
- Ridurre gli sforzi di implementazione.

![[Pasted image 20240125105859.png|650]]

In genere:
- Ogni classe del [[UML 5 - Diagramma di Classi|Class Diagram]] è mappata da una classe Java;
- La relazione di [[UML 5 - Diagramma di Classi#Ereditarietà o Generalizzazione|generalizzazione]] è mappata da una istruzione [[Keyword extends|Extends]];
- Ogni attributo del modello è mappato in un campo privato della classe Java e dai getter e setter.

## Reverse Engineering
>Il **Reverse Engineering** *produce diversi elementi di un modello* a partire dal codice sorgente.

È usato per ricreare il modello di sistemi già esistenti.



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