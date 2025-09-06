---
aliases:
  - insiemi disgiunti
tags:
  - corsi/matematica/matematica_discreta
paragrafo: Teoria degli Insiemi - Operazioni
cssclasses:
  - 
---

> L'**Intersezione** è un'operazione che consente di *estrarre gli elementi comuni da due insiemi* in un nuovo insieme. Quindi, dati due insiemi qualsiasi $S$ e $T$, l'intersezione di questi è l'insieme degli elementi che *appartengono ad ENRAMBI gli insiemi*.
>$$S \textcolor{red}\cap T := \{x\space|\space x\in S \land x\in T\}$$

![[4b79a4d4-0885-42f6-9faa-f1bb20c8a51b.png|400]]

Dove: 
$S\cap T \subseteq S$ e $S\cap T \subseteq T$.

$x\in S\cap T \iff x\in S \land x\in T$
$x\notin S\cap T \iff \neg(x\in S \land x\in T) \iff x\notin S \lor x\notin T$

> [!example]+ <font color="orange">Esempio</font>
>A = {1, 2, -3, a, b}, B = {b, c, -3}: $\quad$A $\cap$ B = \{-3, b\}

Due insiemi si dicono **Disgiunti** se la loro intersezione è un insieme vuoto $S \cap T = \emptyset$. 
‎ %%← Empty Char%%
![[Pasted image 20240228141009.png|400]]^754b5c

#### PROPRIETÀ
- **Commutativa**: $S\cap T = T\cap S$, perché
$x\in S\cap T \iff x\in S \land x\in T \iff x\in T \land x\in S \iff x\in T\cap S$

- **Associativa**: $(S\cap T)\cap V = S\cap(T\cap V)$, perché
$x\in (S\cap T)\cap V \iff x\in S\cap T \land x\in V \iff x\in S \land x\in T \land x\in V$$\iff x\in S \land x\in T\cap V \iff x\in S\cap (T\cap V)$
- **Iterativa**: $S \cap S = S$;
- **Distributiva**: <font color="green">Dimostrazione (pL 31, 1.4.6)</font>
$\cup$ è distributiva rispetto a $\cap$: $\quad$ $S\cup(T\cap V) = (S \cup B) \cap (S \cup V)$
$\cap$ è distributiva rispetto a $\cup$: $\quad$$S\cap(T\cup V) = (S \cap B) \cup (S \cap V)$
- Dati diversi n insiemi S, la loro intersezione sarà:
**$S1\cap S2\cap ...\cap Sn = \{x\space|\space x\in S1 \land x\in S2 \land ... \land x\in Sn\}$**;
- Dati due insiemi S e T, dove $S \subseteq T$, la loro unione sarà:
**$S\cap T = S$**;
- Dati due insiemi S e T, **$T \subseteq S \iff S\cap T = T$**, <font color="green">Dimostrazione (pL 30, 1.4.4)</font>.



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