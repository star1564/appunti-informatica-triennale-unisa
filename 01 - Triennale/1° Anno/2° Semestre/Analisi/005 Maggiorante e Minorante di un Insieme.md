---
aliases: 
tags:
  - corsi/matematica/analisi
paragrafo: Nozioni di base
cssclasses:
  - 
---
## Maggiorante
>Si dice che $y\in\mathbb{R}$ è un **Maggiorante** di un insieme $X\subseteq \mathbb{R}$ se
>$$\color{#CC241D}\forall x\in X:\ y\geq x$$
> Ovvero, è $y$ è *un* maggiorante se *è più grande di tutti gli elementi* di $X$, oppure se è l'elemento più grande di $X$.

![[Pasted image 20240927101831.png|800]]


Pertanto si prendono in considerazione tutti i numeri minori o uguali dell'estremo destro.

## Minorante
>Si dice che $y\in\mathbb{R}$ è un **Minorante** di un insieme $X\subseteq \mathbb{R}$ se
>$$\color{#CC241D}\forall x\in X:\ y\leq x$$
> Ovvero, è $y$ è *un* minorante se *è più piccolo di tutti gli elementi* di $X$, oppure se è l'elemento più piccolo di $X$.

![[Pasted image 20240927102421.png|800]]

Pertanto si prendono in considerazione tutti i numeri minori o uguali dell'estremo sinistro.

# %%Esempi%%

---
> [!example]+ <font color="orange">Esempio</font>
>Dato l'[[004 Intervalli|intervallo]] $(1, 10]$:
>- $\dots, -100, -1, 0, 1$ sono tutti minoranti dell'insieme, e più in generale qualsiasi numero $y\leq 1$ è un minorante dell'insieme;
>
>- $10, 11, 500, \dots$ sono tutti maggioranti dell'insieme, e più in generale qualsiasi numero $y\geq 10$ è un maggiorante dell'insieme.
>
>
>Dato l'[[004 Intervalli|intervallo]] $(-\infty, 10)$:
>- non ci sono minoranti ($-\infty$ non è un numero reale);
>
>- qualsiasi numero $y\geq 10$ è un maggiorante dell'insieme.

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
- [[037 Maggiorante (MD)]]
- [[038 Minorante (MD)]]
- [YouMath](https://www.youmath.it/lezioni/analisi-matematica/premesse-per-lanalisi-infinitesimale/40-massimo-e-minimo-di-sottoinsiemi-di-r-definizioni.html)