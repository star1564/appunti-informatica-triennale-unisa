---
aliases: 
tags:
  - corsi/matematica/metodi_matematici_informatica
paragrafo: Dimostrazioni per induzione
cssclasses:
  - 
---
Per usare il [[048 Principio di Induzione Strutturale|principio di induzione strutturale]] per dimostrare che in un [[037 Ricorsione di Alberi|albero]] la proprietà $P(T)$ è vera per ogni albero radicato $T$, dobbiamo seguire i seguenti passi.

>**PASSO BASE**: Mostrare che $P(T)$ è vera se $T=(\{r\}, \emptyset)$. Ovvero prendere in considerazione solo un nodo.
>
>**PASSO RICORSIVO**: Sia $T$ un albero costruito a partire dagli alberi $T_1, ..., T_k$. Mostrare che se $P(T_1), ..., P(T_k)$ sono vere, allora $P(T)$ è vera.

> [!example]+ <font color="orange">Esempio</font>
> Dimostrare la seguente affermazione $S(T)$ mediante l'induzione strutturale:
> >Per ogni albero $T(V,E)$ risulta $|V|=|E|+1$.
> 
> Sia $T(V,E)$ un albero.
> **PASSO BASE**: Se $T$ è un albero con un solo nodo $$T=(\{r\}, \emptyset)$$allora $|V|=1$ ed $|E|=0$. Quindi $|V|=1=0+1=|E|+1$ e la base è dimostrata.
> 
> **PASSO INDUTTIVO**: Sia $T$ un albero con più di un nodo. Per la definizione di albero, $T$ è costruito a partire da $k$ alberi $T_1=(V_1, E_1), ..., T_k=(V_k, E_k)$. Quindi:
> $|V|=\sum^{k}_{j=1}|V_j|+1$
> $=\sum^{k}_{j=1}|E_j|+k+1$
> (per ipotesi induttiva $|V_j|=|E_j|+1$, per $1\leq j\leq k$)
> $=|E|+1$
>
> Per il principio di induzione strutturale, abbiamo dimostrato che, per ogni albero $T=(V,E)$, risulta $|V|=|E|+1$.




___
[[000 Indice MMI|↖ Ritorna all'indice ↖]]

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

