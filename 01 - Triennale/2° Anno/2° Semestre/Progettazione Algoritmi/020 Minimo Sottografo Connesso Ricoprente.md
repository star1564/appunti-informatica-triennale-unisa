---
aliases:
  - MST
  - Minimum Spanning Tree
tags:
  - corsi/informatica/progettazione_algoritmi
paragrafo: Alberi e Grafi
cssclasses:
  - 
---
> Il problema algoritmico del **Minimo Sottografo Connesso Ricoprente** (Minimum Spanning Tree, *MST*) riguarda la ricerca di un sottografo [[012 Grafi#^bf9a77|connesso]] di un [[012 Grafi#Grafi Non Diretti (Non Orientati)|grafo non diretto]], che *include tutti i suoi nodi*, e ha il *peso totale minimo possibile*. Il peso totale di un sottografo è la somma dei pesi degli archi inclusi. 
> Inoltre, non devono essere presenti [[016 Cicli nei grafi|cicli]] nel sottografo.

In altre parole, l'obiettivo è trovare un [[013 Alberi|albero]] che connetta tutti i nodi del grafo, utilizzando il minor numero possibile di archi e minimizzando la somma dei pesi di tali archi.

![[Pasted image 20230608112406.png|600]]

Ci sono diversi algoritmi comunemente utilizzati per risolvere il problema MST, tra cui:
1. **[[021 Algoritmo di Prim|Algoritmo di Prim]]**: Questo algoritmo costruisce l'MST **partendo da un nodo di partenza** e aggiungendo gradualmente gli archi *con il peso* minimo che connettono il sottografo corrente a nodi *non ancora inclusi*.
2. **[[022 Algoritmo di Kruskal|Algoritmo di Kruskal]]**: Questo algoritmo costruisce l'MST selezionando gli archi in **ordine crescente dei loro pesi** e aggiungendoli all'albero *se non creano cicli*.

*Entrambi* gli algoritmi di Kruskal e di Prim garantiscono di trovare l'MST ottimale per un grafo non diretto.

> [!quote]+ Proprietà
>Siano i costi degli archi di un grafo $V$ *tutti diversi*. Sia $S\neq\emptyset\subset V$ un sottoinsieme dei nodi e sia $e=(x,y)$ l'arco di *costo minimo* con un estremo $x\in S$ e l'altro $y\in V-S$. Allora *ogni MST contiene l'arco $e$*. ![[Pasted image 20230608113239.png]]

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
- [GeeksForGeeks](https://www.geeksforgeeks.org/what-is-minimum-spanning-tree-mst/)