---
aliases: 
tags:
  - corsi/informatica/progettazione_algoritmi
paragrafo: Tipi di Algoritmi
cssclasses:
  - 
---
Per la risoluzione di *Problemi di Ottimizzazione* (ovvero problemi per cui desideriamo trovare la “migliore” soluzione all'interno di un insieme di possibili soluzioni), si possono usare, oltre gli algoritmi basati su [[010 Programmazione Dinamica|Programmazione Dinamica]], anche gli algoritmi cosiddetti **Greedy**.  

>Un algoritmo **greedy** è un algoritmo di ottimizzazione che fa scelte *localmente ottimali in ogni passo*, sperando di trovare una soluzione globalmente ottimale. In altre parole, l'algoritmo sceglie l'opzione migliore in quel momento senza preoccuparsi di come questa scelta influenzerà le scelte future.

Il funzionamento di un algoritmo greedy può essere descritto in pochi *passi*:
1. Inizializzare l'algoritmo con una *soluzione vuota* o una soluzione parziale iniziale.
2. Scegliere la *migliore opzione disponibile al momento*. 
	- La scelta viene fatta sulla base di una funzione obiettivo che deve essere massimizzata o minimizzata (ad es., in un problema di ottimizzazione di massimo, potrebbe essere quella che ci dà il maggior incremento di valore alla soluzione parziale finora calcolata, in un problema di minimo potrebbe essere quella che causa il minor incremento di costo).
3. Aggiornare la soluzione parziale con l'opzione scelta.
4. Verificare se la soluzione è completa. Se la soluzione è completa, l'algoritmo termina. Altrimenti, torna al passo 2.

Il successo di un algoritmo greedy dipende dalla scelta della funzione obiettivo e dalla capacità di trovare una soluzione locale ottimale che porti a una soluzione globale ottimale. In alcuni casi, un algoritmo greedy può fornire *una soluzione sub-ottimale*, ma in altri casi può fornire una soluzione ottimale.


___
[[000 Indice PA|↖ Ritorna all'indice ↖]]

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