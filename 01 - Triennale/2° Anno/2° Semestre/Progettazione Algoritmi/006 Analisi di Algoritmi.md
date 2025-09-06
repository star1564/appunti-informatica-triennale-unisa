---
aliases: 
tags:
  - corsi/informatica/progettazione_algoritmi
paragrafo: Analisi Algoritmi
cssclasses:
  - 
---
Con il termine **Analisi di Algoritmi** si intende il *calcolo delle risorse* usate da [[001 Introduzione a PA#^Algoritmo|algoritmi]] per risolvere un dato problema.

Per **Risorse** si intende (generalmente) il *tempo impiegato dall'algoritmo* e la *quantità di memoria usata dall'algoritmo*. Queste si valuteranno a meno di una costante moltiplicativa (si useranno quindi le notazioni [[003 Notazione O|O]], [[004 Notazione Ω|Ω]] e [[005 Notazione θ|θ]]).

Il tempo di esecuzione dipende da svariati fattori e viene calcolato in base al [[007 Caso Peggiore|Caso Peggiore]].

##### NUMERO DI OPERAZIONI ELEMENTARI
Si misurerà la complessità di un algoritmo in termini del *numero di operazioni elementari* eseguite, che richiedono un tempo costante per la loro esecuzione. Queste sono:
- addizione di numeri;
- moltiplicazione di numeri (se piccoli);
- confronto tra elementi;
- seguire un puntatore:
- ...

> [!example] <font color="orange">Esempio</font>
>Per trovare un elemento $x$ in una lista, l'operazione elementare da eseguire è il confronto tra elementi.

##### TAGLIA DELL'INPUT
Il tempo di esecuzione di un algoritmo in generale dipende *da quanto è grande l'input* (taglia o dimensione dell'input). Questa taglia si valuta con i seguenti criteri:
- Criterio di costo logaritmico: la taglia dell'input è il numero di bit necessari per rappresentarlo;
- Criterio di costo uniforme (quello più usato): la taglia dell'input è il numero di "elementi" che lo costituiscono.

Dato che ogni istruzione del computer richiede un'unità di tempo e che un numero o "elemento" richiede una [[050 Istruzioni di Trasferimento Dati MIPS#^61b5d3|parola]] di memoria, le due misure coincidono a meno di una costante moltiplicativa.

> [!example] <font color="orange">Esempio</font>
>Per trovare un elemento $x$ in una lista, la taglia dell'input è il numero di elementi della lista.

##### COMPOSIZIONE DELL'INPUT
La complessità di un algoritmo può dipendere anche da come è fatto l'input. 

> [!example] <font color="orange">Esempio</font>
>Ordinare $n$ elementi può essere fatto più velocemente se essi sono già (quasi) ordinati.

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