---
aliases: 
tags:
  - corsi/matematica/probabilità_statistica
inglese: 
paragrafo: Leggi Congiunte delle V.A.
cssclasses:
  - 
---
>Due [[023 Variabili Aleatorie|Variabili Aleatorie]] $X$ e $Y$ si dicono **Indipendenti** se, per ogni coppia $\mathcal{A}$ e $\mathcal{B}$ di sottoinsiemi di $\mathbb{R}$, si ha che la loro [[016 Definizione di Probabilità|probabilità]] sarà
>$$\color{#CC241D}\mathbb{P}(X\leq x, Y\leq y)=\mathbb{P}(X\leq x)\cdot \mathbb{P}(Y\leq y)$$

La condizione di indipendenza si può equivalentemente esprimere con due insiemi di numeri reali: $$\mathbb{P}(X\in \mathcal{A}, Y\in \mathcal{B})=\mathbb{P}(X\in \mathcal{A})\cdot \mathbb{P}(Y\in \mathcal{B})$$

È simile alla definizione di due [[021 Eventi Indipendenti|Eventi Indipendenti]], in quanto le Variabili Aleatorie sono degli eventi.

## Con distribuzione congiunta
In termini della [[040 Funzioni di Distribuzione Congiunte|distribuzione congiunta]] $F(x,y)$ di $(X,Y)$, si ha che $X$ e $Y$ sono indipendenti se e solo se $$F(x,y)=F_X(x)\cdot F_Y(y),\quad \forall x,y,\in\mathbb{R}$$
### Variabili Aleatorie Discrete
Se le due variabili aleatorie $X$ e $Y$ sono [[025 Variabili Aleatorie Discrete|discrete]], allora queste saranno indipendenti se e solo se la loro [[040 Funzioni di Distribuzione Congiunte#Variabili Aleatorie Discrete|densità discreta congiunta]] può essere espressa come $$p(x,y)=p_X(x)\cdot p_Y(y),\quad \forall x,y\in \mathbb{R}$$ ^e48a86


___
[[000 Indice PS|↖ Ritorna all'indice ↖]]

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