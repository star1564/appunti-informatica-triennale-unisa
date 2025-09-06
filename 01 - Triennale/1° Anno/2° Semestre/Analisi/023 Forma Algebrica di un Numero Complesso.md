---
aliases:
  - Forma Cartesiana
tags:
  - corsi/matematica/analisi
paragrafo: Numeri Complessi
cssclasses:
  - 
---
>La **Forma Algebrica** (o *cartesiana*, *aritmetica*) di un [[022 Numeri Complessi|numero complesso]] è una *rappresentazione* che permette di esprimere un *qualsiasi numero complesso* $z$ esplicitandolo nella seguente [[MB - 00 Equazioni e Uguaglianze#Equazione|equazione]]: $$\color{#CC241D}z=a+ib$$
>
>Dove: 
>- $a\in \mathbb{R}$ si chiama *Parte Reale* 
>- $b\in \mathbb{R}$ si chiama *Parte Immaginaria*
>- $i$ è l'[[022 Numeri Complessi#Unità Immaginaria|Unità Immaginaria]]

> [!example]+ <font color="orange">Esempio</font>
>- $1+3i$
>- $\sqrt[]{2}-7i$
>- $\ln(5)+12i$
>- $\dots$

### Ricavare la Forma Algebrica
Partendo dal numero complesso $(a,b)\in\mathbb{C}$ si può ricavarne la *forma algebrica* nel modo seguente utilizzando la [[024 Operazioni e Proprietà dei Numeri Complessi#Somma|somma di due numeri complessi]]:

$$
\begin{align*}
\textcolor{#CC241D}{z}&=(a,b) \\
&= (a,0)+(0,b) \\
&= (a,0)+(0\cdot b-1\cdot 0, b\cdot1+0\cdot0) \\
&= (a,0)+(0,1)(b,0) \\
&= \color{#CC241D}a+ib \\
\end{align*}
$$


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
- [YouMath](https://www.youmath.it/lezioni/analisi-matematica/numeri-complessi/2698-numeri-complessi-in-forma-algebrica.html)