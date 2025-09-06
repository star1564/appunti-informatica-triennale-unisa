---
aliases: 
tags:
  - corsi/informatica/progettazione_algoritmi
paragrafo: Tipi di Algoritmi
cssclasses:
  - 
---
>La tecnica della **Programmaziopne Dinamica** è molto simile a quella del [[009 Algoritmi Divide-et-Impera|Divide-et-Impera]], ma *si risolvono i vari sottoproblemi una sola volta*.

Pertanto gli [[001 Introduzione a PA#^Algoritmo|Algoritmi]] basati sulla programmazione dinamica risultano essere più efficienti nel caso in cui i sottoproblemi di un dato problema tendono a ripetersi.
Questo lo si può ottenere *memorizzando i risultati dei sottoproblemi in una* ***tabella***, in modo tale che questi possano essere usati in seguito se necessario.

> [!summary] Passi per la programmazione dinamica
> 1. **Formulare il problema in termini [[008 Algoritmi Ricorsivi|ricorsivi]]**: scrivere una espressione per la soluzione all'intero problema che sia una combinazione di soluzioni a sottoproblemi di taglia minore;
> 2. (*Per Algoritmi Ricorsivi*) **Calcolare la soluzione globale al problema in modo ricorsivo**: far precedere ciascuna chiamata ricorsiva con un controllo per verificare se la soluzione al relativo sottoproblema è stata già calcolata.
> 3. (*Per Algoritmi Iterativi*) **Calcolare le soluzioni ai sottoproblemi in maniera "bottom-up"**: scrivere un algoritmo che parta con i casi base della ricorrenza e proceda via via considerando (e risolvendo problemi di taglia sempre maggiore, considerandoli nell'ordine corretto).

Si prenda in considerazione la successione di [[028 Ricorsione (Programmazione)#ESEMPIO 2: FIBONACCI|fibonacci]] realizzata con un algoritmo ricorsivo normale. Queste sono le chiamate effettuate:

![[Pasted image 20230325104845.png|700]]

Si può notare che molte chiamate ricorsive sono calcolate diverse volte.

Per rendere questo algoritmo efficiente, basta memorizzare in un array i valori della chiamata `fib(i)`, con $i<n$ quando li si calcola per la prima volta. In modo che, in future chiamate ricorsive, non sarà più necessario calcolare `fib(i)` ma basterà leggerli dall'array.

```C
memFib(n){
	if((n==0) || (n==1))
		return 1;
	else {
		if(a[n] non è definito)
			a[n] = memFib(n-1) + memfib(n-2);
	}
	return a[n];
}
```

![[Immagine 2023-03-25 110157.png|600]]

> [!note]+ Lista o HashTable invece di un Array
> Per rendere questo fattibile in una situazione reale, la tabella deve essere capace di essere molto grande e flessibile nella sua dimensione. Pertanto sarebbe utile l'utilizzo di [[020 Liste nel C|liste concatenate]] o [[026 Tabelle Hash|hashtable]] al posto di un array.

Questa tecnica può anche funzionare con un algoritmo iterativo.



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