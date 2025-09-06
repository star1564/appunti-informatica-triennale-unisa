---
aliases: 
tags:
  - corsi/matematica/matematica_discreta
paragrafo: Altro
cssclasses:
  - 
---
> [!warning] !!! VERSIONE MIGLIORATA [[046 Principio di Induzione Matematica|QUI (MMI)]] !!!

Il **principio di induzione** è una proprietà fondamentale dell'insieme dei numeri naturali, che fornisce un metodo dimostrativo insostituibile.

Se si vuole dimostrare per induzione che una certa uguaglianza P<sub>n</sub> è vera per ogni numero naturale $n\geq m$ dovrò procedere come segue:
1) **Base dell'Induzione**: Devo verificare che P<sub>m</sub> sia vera.
2) *Passo Induttivo*: Devo supporre che per un certo $n\geq m$, $P(n)$ sia vera (<font color="green">Ipotesi Induttiva</font>). Poi bisogna dimostrare che $P(n+1)$ sia vera.


> [!example]- <font color="orange">Esempio</font>
>Proviamo a dimostrare per induzione su $n$ che:
>$P_n: 2^1+2^2+...+2^n = 2(2^n-1), \quad \forall n\geq 1$
>
>1. **Passo Base**: $P_1: 2 = 2(2^1-1)=2(1)=2.\quad$ 
>$P_1$ Vera.
>2. *Passo Induttivo*: Supponiamo che: $P_n\space vera \Longrightarrow P_{n+1}\space vera$.
>
>
><font color="green">Ipotesi Induttiva</font>: $P_n: 2+2^2+...+2^n=2(2^n-1)$.
>
>Devo provare che: $P_{n+1}: 2+2^2+...+2^n+2^{n+1}=\color{yellow}2(2^{n+1}-1)$.
>
>$\textcolor{green}{2+2^2+...+2^n}+2^{n+1}=\textcolor{green}{2(2^n-1)}+2^{n+1} =$
>$=2(2^n-1)+2*2^n = \quad\quad$ Proprietà per separare $=2^{n+1}$ in $2*2^n$
>$=2((2^n-1)+2^n)=\quad\quad$ Si mette in evidenza il 2
>$=2(2*2^n-1) =\quad\quad$ Si uniscono i $2^n$ raddoppiandolo
>$=\color{yellow}2(2^{n+1}-1).\quad\quad$ Proprietà per unire $2*2^n$ in $2^{n+1}$
>
>Si è provata l'affermazione $P_n, \forall n\geq 1$.

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
- Pagina Libro: 20