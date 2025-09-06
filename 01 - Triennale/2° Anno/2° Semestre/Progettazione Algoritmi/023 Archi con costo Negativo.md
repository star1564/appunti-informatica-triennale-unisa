---
aliases: 
tags:
  - corsi/informatica/progettazione_algoritmi
paragrafo: Alberi e Grafi
cssclasses:
  - 
---
Finora abbiamo considerato solo archi con costo maggiore o uguale a zero. Introduciamo adesso quelli con **costo negativo**.

Dalla sua definizione, l'[[019 Algoritmo di Dijkstra|Algoritmo di Dijkstra]], *non funziona correttamente* per grafi con archi con costo negativo. Si basa infatti sull'assunzione che tutti i costi degli archi siano non negativi.

La ragione principale è che l'algoritmo di Dijkstra utilizza una strategia [[011 Algoritmi Greedy|greedy]] selezionando sempre il percorso più economico in base ai costi degli archi finora visitati. L'introduzione di archi con costo negativo può causare **[[016 Cicli nei grafi|cicli]] negativi**, cioè cicli in cui la somma dei costi degli archi è *negativa*. Quando un ciclo negativo è presente nel grafo, l'algoritmo di Dijkstra può cadere in un *loop infinito*, poiché continua a trovare percorsi più economici nel ciclo negativo.

> Quindi, se nel grafo $G=(V,E)$ *esiste* un cammino dal nodo $s$ al nodo $t$ che contiene un ciclo di costo totale negativo, allora **NON esiste** alcun cammino di costo totale minimo da $s$ a $t$.
>
> Viceversa, se ogni cammino da $s$ a $t$ *NON contiene* alcun ciclo di costo totale negativo, allora **esiste** un cammino di costo totale minimo da $s$ a $t$ ed esso contiene al più $n-1$ archi.

![[Pasted image 20230608153104.png]]

Infatti, se in un cammino da $s$ a $t$ esistesse un ciclo $W$ di costo totale $c(W)<0$, del tipo rappresentato qui, allora ripassando *sempre più volte* sul ciclo $W$ potremmo abbassare sempre di più il costo del percorso da $s$ a $t$, senza mai fermarci ad un minimo.

Per gestire grafi con archi con costo negativo, esistono altri algoritmi come l'algoritmo di **[[024 Algoritmo di Bellman-Ford|Bellman-Ford]]**, che sono in grado di gestire cicli negativi e trovare i cammini più brevi corretti anche in presenza di archi con costo negativo. Questi algoritmi utilizzano approcci diversi rispetto a Dijkstra per garantire la correttezza dei risultati.

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