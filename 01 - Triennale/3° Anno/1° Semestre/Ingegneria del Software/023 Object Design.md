---
aliases:
  - Oggetti di Applicazione
  - Oggetti di Soluzione
tags:
  - corsi/informatica/ingegneria_software
paragrafo: Progettazione - Object Design
cssclasses:
  - 
---
>L'**Object Design** è la fase di *raffinazione* dei componenti trovati nel [[019 System Design|System Design]] e degli oggetti trovati nell'[[015 Object Modeling|Object Modeling]]. Serve come base per l'implementazione e, nella sua durata, chiude il gap tra gli oggetti di applicazioni e le componenti off-the-shelf.

Una **componente off-the-shelf** è una componente *già pronta per essere utilizzata*, quindi pronta per essere venduta/consegnata.

### Oggetti di Applicazione e Soluzione
Durante la fase di [[013 Analisi dei Requisiti|Analisi]], si sono trovati gli **Oggetti di Applicazione**, che rappresentano i concetti del [[002 Sistemi, Modelli e Viste#^ae7aa9|Dominio di Applicazione]] rilevanti al sistema.

Gli **Oggetti di Soluzione** sono degli oggetti che non sono mappabili su concetti relativi al dominio dell'applicazione, ma servono a rendere funzionale il sistema, quindi rappresentano i concetti del [[002 Sistemi, Modelli e Viste#^0f8d39|Dominio di Soluzione]]. Si trovano *attraverso l'Object Design*.

### Attività dell'Object Design
L'Object Design include questi gruppi di attività:
- [[024 Riutilizzo|Riutilizzo]];
- [[027 Specifica delle Interfacce|Specifica delle Interfacce]]:
	- Identificare attributi ed operazioni mancanti;
	- Specificare la visibilità, i tipi e le signatures.
	- Specificare contratti.
- Ristrutturazione;
- Ottimizzazione;

## Ristrutturazione
Questa attività *manipola il modello del sistema per incrementare il riuso del codice o per soddisfare altri obbiettivi* di progettazione (come mantenimento, leggibilità e comprensione del modello di sistema).

Esempi di ristrutturazioni:
- Trasformare associazioni N-arie in binarie;
- Implementare associazioni binarie attraverso riferimenti;
- Unire classi simili in differenti sottosistemi in un'unica classe;
- Trasformare classi senza un comportamento in attributi;
- Decomporre classi complesse in classi più semplici;
- Aumentare l'ereditarietà ed il packaging modificando classi ed operazioni.

## Ottimizzazione
Questa attività ha lo scopo di *migliorare la performance generale del sistema*: aumentare la velocità, ridurre l'uso di memoria e diminuire la molteplicità delle associazioni.

Esempi di ottimizzazione:
- Cambiare gli algoritmi per rispondere ai requisiti di memoria e velocità;
- Ridurre le molteplicità nelle associazioni per velocizzare le query;
- Aggiungere associazioni ridondanti per aumentare l'efficienza;
- Modificare l'ordine di esecuzione;
- Aggiungere attributi derivati per migliorare il tempo di accesso agli oggetti;
- Rendere l'arhitettura [[019 System Design#^a75bda|aperta]].

Altro sull'ottimizzazione può essere trovato nel [[029 CVS - Implementazione|capitolo di implementazione]] in "[[031 Ottimizzare l_Object Design Model]]".

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