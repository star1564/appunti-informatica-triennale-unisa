---
aliases:
  - ciclo in un grafo
tags:
  - corsi/informatica/progettazione_algoritmi
paragrafo: Alberi e Grafi
cssclasses:
  - 
---
>Un **Ciclo** in un grafo $G=(V,E)$ è una sequenza di nodi $v_1,v_2,\dots, v_n$ tale che $$(v_i,v_{i+1})\in E,\quad \forall i=1,\dots,n-1\quad \land\quad v_1=v_n$$
>Ovvero, è un percorso che *inizia e termina nello stesso nodo*, attraversando una sequenza di nodi distinti lungo il percorso.

Un grafo $G$ non diretto senza cicli ha una struttura molto semplice:
- È un albero (se $G$ è connesso);
- Oppure ogni sua componente connessa è un albero.

Normalmente nei grafi non diretti ci sono molti cicli. È molto più complicato invece vedere se ci sono cicli in un grafo diretto.

>Un *grafo diretto senza cicli* viene chiamato **DAG** (directed acyclic graph). Se un grafo diretto non è un DAG, allora ci potrebbero essere problemi.

> [!example]- <font color="orange">Esempio</font>
> Il seguente grafo diretto è un DAG, perché non ha cicli e non è un albero. 
> 
> ![[Pasted image 20230607144332.png]]
> Diciamo che questo grafo rappresenta vari "compiti" $c_1,c_2,\dots,c_n$ da eseguire, con la condizione che per certe coppie ($c_i, c_j$) di tali compiti è necessario che il compito $c_i$ debba essere eseguito prima di $c_j$.
>
> Dato il seguente grafo che non è un DAG abbiamo un problema fondamentale: non sappiamo quale compito eseguire prima. 
> 
> ![[Pasted image 20230607144607.png]]
> Quindi se il grafo diretto che descrive i vincoli sull'ordine di esecuzione dei compiti *non è un DAG* allora non vi è alcun modo di eseguire tutti i compiti rispettando i vincoli sul loro ordine di esecuzione relativo.

Dato un DAG descrivente i vincoli sull'ordine di esecuzione di certi compiti, vogliamo determinare una *numerazione* $n(v_1), n(v_2),\dots,n(v_n)$ dei compiti. Questa numerazione viene chiamata **Ordinamento Topologico**.

Questo è quindi un concetto utilizzato per *ordinare i nodi* di un grafo DAG in un modo tale che *ogni arco nel grafo punti da un nodo precedente a un nodo successivo nell'ordinamento*.

Inoltre, se $G$ ammette un ordinamento topologico, allora $G$ è necessariamente *senza cicli*.

> [!summary] Algoritmo: TopSort(Grafo)
> 1. Trova un nodo senza archi entranti (nessun arco che punta ad esso) e inseriscilo nell'ordinamento;
> 2. Rimuovi il nodo e tutti gli archi uscenti da esso;
> 3. Effettua una chiamata ricorsiva alla funzione passando il grafo senza il nodo rimosso.

> [!example]- <font color="orange">Esempio</font>
> Dato il seguente grafo, trovare l'ordinamento topologico. 
> ![[Pasted image 20230607145601.png|700]]
>Risoluzione: ![[Pasted image 20230607151011.png|700]]
>Ordine: $1,2,3,4,5,6,7$

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