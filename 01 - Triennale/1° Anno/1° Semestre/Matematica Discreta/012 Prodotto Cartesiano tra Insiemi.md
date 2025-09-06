---
aliases: 
tags:
  - corsi/matematica/matematica_discreta
paragrafo: Teoria degli Insiemi - Operazioni
cssclasses:
  - 
---
> [!info]+ Coppia Ordinata
>Una **coppia ordinata** è composta dalla prima coordinata x e dalla seconda coordinata y: (x, y).
>
>In generale se:
>$(x,y) = (a, b) \iff (x = a) \land (y=b)$
>$(x,y)\neq(y,x)$ ma $\{x, y\}\neq\{y,x\}$

^87b6fd

>Il **Prodotto Cartesiano** è un'operazione che consente di *combinare gli elementi due insiemi* per creare un nuovo insieme *che rappresenta tutte le possibili coppie ordinate di elementi*. Quindi, dati due insiemi qualsiasi $S$ e $T$, il prodotto cartesiano di questi è l'insieme di coordinate $(x, y)$ tale che:
> $$S\textcolor{red}\times T=\{(x,y)\space | \space x \in S \land y \in T\}$$

![[c1dff12f-e28e-4909-be9c-88b7c2571627.png|400]]

Dove:
$(x, y)\in S\times T\iff a\in S \land b\in T$
$(x, y)\notin S\times T\iff \neg(a\in S \land b\in T)= a\notin S \lor b\notin T$

Se |A| = n, |B| = m, allora $\quad$|A$\times$B| = n$\cdot$m = |B$\times$A|

In generale quindi $\color{#CC241D}A\times B \neq B\times A$.

> [!example]+ <font color="orange">Esempio</font>
>A = {a, b, c}, B = {3, 4}: $\quad$A$\times$B = {(a,3), (a,4), (b,3), (b,4), (c,3), (c,4)}

#### DIAGONALE DEL PRODOTTO CARTESIANO
Sia S un insieme non vuoto.
S$\times$ S avrà una **Diagonale del Prodotto Cartesiano**, ovvero:
>$$\textcolor{red}\Delta S := \{(x,y)\in S\times S\space | \space x = y\} = \{(x, x)\space |\space x \in S\}$$

> [!example]- <font color="orange">Esempio</font>
>$S = \{a, b\}$
>$S\times S=\{\textcolor{yellow}{(a, a)}, (a, b), (b, a), \textcolor{yellow}{(b, b)}\}$
>$\Updelta S=\{\textcolor{yellow}{(a,a)}, \textcolor{yellow}{(b,b)}\}$

#### PROPRIETÀ
- $S\times \emptyset = \emptyset = \emptyset \times S$;
- $S\subseteq V, T\subseteq W \iff S\times T \subseteq V\times W$, <font color="green">Dimostrazione (pL 43, 1.5.1)</font>;
- $S\times T = T\times S \iff S = \emptyset \lor T = \emptyset \lor S = T$, <font color="green">Dimostrazione (pL 44, 1.5.2)</font>.
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