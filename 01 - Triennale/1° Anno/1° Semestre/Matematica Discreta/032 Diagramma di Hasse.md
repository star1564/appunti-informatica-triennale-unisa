---
aliases: 
tags:
  - corsi/matematica/matematica_discreta
paragrafo: Relazioni - Ordine
cssclasses:
  - 
---
>Un **Diagramma di Hasse** è una *rappresentazione grafica* di un [[031 Tipi di Insiemi di Relazioni#Insieme Ordinato|Insieme Ordinato]] (finito) che visualizza le [[029 Relazioni d_Ordine|relazioni di ordinamento]] tra gli elementi dell'insieme in modo conciso e intuitivo. 

Questi diagrammi sono utilizzati principalmente per rappresentare relazioni d'ordine in un formato gerarchico, che facilita la comprensione della struttura dell'insieme e delle relazioni tra i suoi elementi.

### Caratteristiche
1. **Elementi**: Ogni elemento dell'insieme ordinato è rappresentato da un nodo (o punto) nel diagramma.
2. **Relazioni d'ordine**: Le relazioni d'ordine tra gli elementi sono rappresentate da linee tra i nodi.
3. **Gerarchia**: I nodi sono organizzati in modo che gli elementi che sono in relazione di ordine siano posizionati in modo gerarchico, in modo che gli elementi "superiori" siano posizionati più in alto nel diagramma rispetto agli elementi "inferiori".
4. **Mancanza di duplicati**: I nodi duplicati non sono inclusi. Se più elementi sono equivalenti (ovvero, hanno lo stesso rapporto d'ordine), vengono rappresentati come un singolo nodo.

> [!example]- <font color="orange">Esempio (Divide)</font>
>$A = \{4, 2, 3, 4, 5, 6\}, (A, |)$
>$a, b\in A, aRb \iff a|b$
>
>![[Pasted image 20211130143722.png]]


> [!example]- <font color="orange">Esempio con una [[030 Relazione d_Ordine Usuale|Relazione d'Ordine Usuale ≤]]</font>
>$A = \{4, 2, 3, 4, 5, 6\}, (A, \leq)$
>Si ha una [[031 Tipi di Insiemi di Relazioni#Insieme Totalmente Ordinato (Catena)|Catena]]
>
>![[Pasted image 20211130143733.png]]



___
[[000 Indice MD|↖ Ritorna all'indice ↖]]

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
- Pagina Libro: 91