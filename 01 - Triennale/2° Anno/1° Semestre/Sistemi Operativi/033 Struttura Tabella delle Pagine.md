---
aliases: 
tags:
  - corsi/informatica/sistemi_operativi
paragrafo: Memoria
cssclasses:
  - 
---
La maggior parte dei moderni calcolatori dispone di uno spazio d'[[Indirizzo Logico|indirizzi logici]] molto grande, quindi in un ambiente di [[031 Paginazione|paginazione]] la tabella delle pagine potrebbe essere *eccessivamente grande*.

Una soluzione semplice a questo problema consiste nel suddividere la tabella delle pagine in parti più piccole. Questo lo si può ottenere attraverso:
- *Paginazione Gerarchica*;
- *Tabella delle Pagine con Hashing*;
- *Tabella delle Pagine Invertita*.

---
### PAGINAZIONE GERARCHICA
La **Paginazione Gerarchica** consiste nell'adottare un algoritmo di paginazione a *due livelli*, in cui le diverse tabelle delle pagine vengono messe in una nuova tabella.

![[Pasted image 20221201141048.png|500]]
<center>Schema di una tabella delle pagine a due livelli</center>

In un sistema a 32 bit con pagine di 4 KB, un indirizzo logico è diviso in:
- Un numero di pagina di **20 bit**, diviso a sua volta in:
	- Un numero di pagina da *10 bit*;
	- Un offset nella pagina da *10 bit*.
- Un offset della pagina di **12 bit**.

![[Pasted image 20221201141409.png|550]]

Dove $p_1$ è un indice per la tabella esterna e $p_2$ rappresenta lo spostamento nella pagina della tabella esterna.

![[Pasted image 20221201141633.png|500]]
<center>Traduzione degli indirizzi per un'architettura a 32 bit con paginazione a due livelli</center>

---
### TABELLA DELLE PAGINE CON HASHING
L'utilizzo di una **Tabella delle Pagine con Hashing** è un metodo di gestione molto comune degli spazi d'indirizzi maggiore di 32 bit.

Il numero di *pagina virtuale* $\color{#61AFEF}p$ è l'input di una [[Funzione Hash]]. Il valore hash prodotto viene usato come indice nella tabella.
Ogni elemento della tabella contiene una lista di coppie (numero pagina, frame).

Il numero di pagina $p$ viene confrontato con i numeri di pagina di questa lista, cercando una corrispondenza. Se viene trovata, il numero di frame fisico associato al numero di pagina viene estratto.

![[Pasted image 20221201142508.png|600]]

---
### TABELLA DELLE PAGINE INVERTITA
Una **Tabella delle Pagine Invertita** ha *un elemento per ogni frame* della memoria centrale. Ciascun elemento è costituito dell'indirizzo virtuale della pagina memorizzata in quel frame fisico, con informazioni sul processo a cui appartiene quella pagina.

Questo schema permette di *diminuire la quantità di memoria* centrale necessaria *per la memorizzazione delle tabelle*, però aumenta il tempo necessario per cercare nella tabella quando si verifica un riferimento alla pagina.

È possibile usare una tabella con hashing per limitare la ricerca a uno a al più pochi elementi nella tabella delle pagine.

![[Pasted image 20221201142923.png|530]]

___
[[000 Indice SO|↖ Ritorna all'indice ↖]]

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