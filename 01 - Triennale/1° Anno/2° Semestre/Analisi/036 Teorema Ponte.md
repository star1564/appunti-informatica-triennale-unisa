---
aliases: 
tags:
  - corsi/matematica/analisi
paragrafo: Limiti di Successioni
cssclasses:
  - 
---
>Il **Teorema Ponte** è un importante strumento che lega i concetti di [[032 Limiti di Successioni|limiti di successioni]] ai [[037 Limiti di Funzioni|limiti di funzioni]].


> [!quote] <font color="orange">Teorema Ponte - (EAM1, pL 116)</font>
>Siano:
>- $A\subseteq \mathbb{R}$
>- $f:A\to \mathbb{R}$
>- $x_0\in \mathbb{R} \cup \left\{ -\infty, +\infty \right\}$ un [[009 Intorno di un Punto#Punto di Accumulazione|punto di accumulazione]] per $A$
>- $L\in\mathbb{R}\cup \left\{ -\infty, +\infty \right\}$
>
>Allora
>$$\lim\limits_{x \to x_0}f(x)=L$$
>*se e solo se*, per ogni successione $\{a_n\}_{n\in\mathbb{N}}$ a valori in $A-\left\{ x_{0} \right\}$ e [[032 Limiti di Successioni#Convergente|convergente]] a $x_0$, risulta che
>$$\lim\limits_{n \to +\infty}f(a_n)=L$$


> [!success]- Dimostrazione
> Dalla definizione di limite di funzione si ha che:
> $$\epsilon - l < f(x) < \epsilon + l$$
> Quindi:
> $$x_0-\delta < x < x_0 +\delta$$
> Ma allora:
> $$x_0-\delta < a_n < x_0 +\delta \iff \exists  n > N$$
> 
> Pertanto: 
> $$\epsilon - l < f(a_n) < \epsilon + l$$




___
[[000 Indice Analisi|↖ Ritorna all'indice ↖]]

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
- https://www.youtube.com/watch?v=QmxAUoN_N0M
- https://quisirisolve.com/analisi-matematica/successioni/teoria-sulle-successioni/il-teorema-ponte/