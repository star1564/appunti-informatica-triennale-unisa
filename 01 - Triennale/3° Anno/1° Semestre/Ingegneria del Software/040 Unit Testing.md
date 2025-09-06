---
aliases: 
  - Unit Test
  - Equivalence Testing
tags:
  - corsi/informatica/ingegneria_software
paragrafo: Testing
cssclasses:
  - 
---
>Lo **Unit Testing** si concentra sul *test di singoli componenti* come oggetti e sottosistemi in un sistema software.

Trova differenze del comportamento tra il [[028 Object Design Document|Modello di Object Design]] e quello osservato nell'implementazione.

Questo approccio presenta tre vantaggi principali:
- *Riduce la complessità di testing* per tutte le altre attività;
- Semplifica l'individuazione e la risoluzione dei problemi, poiché in ciascun test *sono coinvolti solo pochi componenti*;
- Permette il test dei componenti in modo indipendente, consentendo *attività di test parallele*.

Gli oggetti da testare vengono generalmente scelti dal [[014 Modello ad Oggetti|Modello ad Oggetti]] e dalla [[022 System Design Document|decomposizione in sottosistemi]]. L'insieme minimo per i test dovrebbe includere oggetti che partecipano ai casi d'uso.

I sottosistemi vengono testati come componenti solo *dopo aver testato individualmente ciascuna classe* all'interno del sottosistema. I sottosistemi esistenti con struttura interna sconosciuta, come quelli disponibili in commercio, dovrebbero essere trattati come componenti durante i test.

I diversi tipi di Unit Testing possono essere divisi in:
- *[[037 Concetti di Testing#Blackbox|Blackbox]] Unit Testing* (**funzionale**): i casi di test sono determinati in base a ciò che il componente deve fare.
- *[[037 Concetti di Testing#Whitebox|Whitebox]] Unit Testing* (**strutturale**): i casi di test sono determinati in base a come il componente è implementato.

## Blackbox Unit Testing
### Equivalence Testing
L'obbiettivo è quello di ridurre il numero di test case attraverso la divisione degli input in [[027 Classi di Equivalenza|Classi di Equivalenza]]. 

> [!example] <font color="orange">Esempio</font>
>Ad esempio, tutti gli input di numeri negativi e tutti gli input di numeri positivi sono due classi di equivalenza distinti.

In genere le classi di equivalenze vengono create in questo modo:
- Se l'input valido **è un range**: si creano tre classi di equivalenza corrispondenti ai valori *sotto*, *dentro* e *sopra* il range di valori.
- Se l'input valido **è un insieme discreto**: si creano due classi di equivalenza corrispondenti ai valori *dentro* e *fuori* dall'insieme.

Qui si presuppone che i sistemi *si dovrebbero comportare in modo simile per tutti i membri di una classe*. Per testare il comportamento associato ad una classe di equivalenza, bisogna testare solo un membro della classe.

### Boundary Testing
È un tipo particolare di Equivalence Testing che si concentra *sulle condizioni al boundary (limite) della classe di equivalenza*.
Invece di prendere gli elementi a caso da una classe, si vanno a prendere solamente gli elementi ai limiti.

## Whitebox Unit Testing
Ci sono quattro tipi:
- **Statement Testing**: vengono testati *i singoli statement* del codice;
- **Loop Testing**: vengono dati vari input in modo da *saltare un ciclo*, entrare in un ciclo *una sola volta* e ripetere un ciclo *più volte*;
- **Path Testing**: viene costruito un diagramma di flusso del programma e si controlla se *tutti i blocchi del diagramma vengono attraversati*.
- **Branch Testing**: ci si assicura che *ogni tipo di uscita da un'istruzione condizionale* (`if`, `else`, `else if`) sia testa almeno una volta.


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