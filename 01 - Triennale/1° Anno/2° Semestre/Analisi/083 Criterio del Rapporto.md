---
aliases: 
tags:
  - corsi/matematica/analisi
paragrafo: Serie Numeriche
cssclasses:
  - 
---
Sia$\displaystyle\sum_{n=0}^{+\infty} a_n$:
1. Una Serie Numerica a [[078 Serie Numeriche#^c8052e|Termini Positivi]] 
2. Si ha che $$\lim\limits_{n \to +\infty}\frac{a_{n+1}}{a_n}=l,\quad\quad l\in[0,+\infty)\ \text{ oppure }\ l=+\infty$$


> Ci sono tre casi:
> - $\color{#CC241D}l<1$: la serie [[079 Carattere di una Serie Numerica#Serie Numerica Convergente|converge]]
> - $\color{#CC241D}l>1$: la serie [[079 Carattere di una Serie Numerica#Serie Numerica Divergente|diverge]]
> - $\color{#CC241D}l=1$: non si può dire nulla sulla serie, bisogna quindi cambiare criterio

> [!warning] Se il risultato del limite è 1, il criterio non è conclusivo



> [!example]+ <font color="orange">Esempio</font>
> $$\displaystyle\sum_{n=0}^{+\infty} \frac{n^5}{3^n}$$
> 
> $$\begin{align*}\lim\limits_{n \to +\infty}\frac{a_{n+1}}{a_n}&=\lim\limits_{n \to +\infty}\frac{\frac{(n+1)^5}{3^{n+1}}}{\frac{n^5}{3^n}}\\ &=\lim\limits_{n \to +\infty}=\frac{(n+1)^5}{3\cdot \cancel{3^n}}\cdot\frac{\cancel{3^n}}{n^5}\\ &=\lim\limits_{n \to +\infty} \frac{1}{3} \cdot \underbracket{\frac{(n+1)^5}{n^5}}_{\text{il limite è }1}\\ &=\frac{1}{3}<1 \Longrightarrow \text{ Converge}\end{align*}$$


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
- [Elia Bombardelli](https://youtu.be/W7GWf3En0M4?&t=243)