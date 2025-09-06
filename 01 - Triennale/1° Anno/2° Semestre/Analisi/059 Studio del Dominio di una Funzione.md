---
aliases: 
tags:
  - corsi/matematica/analisi
paragrafo: Studio di Funzione
cssclasses:
  - 
---

> [!info] Nota (1/7) sullo [[058 Studio di una Funzione|Studio di una Funzione]]
> [[060 Studio della Parità e Disparità di una Funzione|Prossimo passo ->]]

>Il **Dominio di una funzione** (insieme di definizione) è quel sottoinsieme di $\mathbb{R}$ in cui $y=f(x)$ *è definita*.

Quindi per determinare il dominio della funzione $y=f(x)$ è sufficiente individuare in quali punti o intervalli di $\mathbb{R}$ la funzione non è definita, ossia i punti e gli insiemi di punti in cui *non ha senso valutare la funzione*.

Per farlo, si devono imporre le [[042 Campo di Esistenza|Condizioni di Esistenza]].

Una volta determinate tutte le condizioni di esistenza, bisogna metterle a sistema, per poi risolvere quest'ultimo.

L'insieme risultante sarà proprio il dominio della funzione.


> [!example]+ <font color="orange">Esempio Caso Studio</font>
> $$y=\frac{ln(x+1)}{-2-ln(x+1)}$$
> Per il logaritmo dobbiamo porre l'argomento maggiore di zero; il denominatore della frazione invece va posto diverso da zero. Otteniamo il sistema:
> $$\begin{cases} x+1>0 \\ -2-ln(x+1)\neq0 \end{cases}\iff\begin{cases} x>-1 \\ ln(x+1)\neq-2 \end{cases}$$ $$\begin{cases} x>-1 \\ x+1\neq e^{-2} \end{cases}\iff\begin{cases} x>-1 \\ x\neq e^{-2}-1 \end{cases}$$
> ![[Pasted image 20220503144347.png]]
> 
> Il sistema ammette il seguente insieme delle soluzioni: $$\{-1<x<e^{-2}-1\}\ \lor\ \{x>e^{-2}-1\}$$
> Ovvero: $$\color{orange}Dom(f)=(-1,\ e^{-2}-1)\ \cup\ (e^{-2}-1,\ +\infty)$$

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
- [YouMath](https://www.youmath.it/lezioni/analisi-matematica/studio-di-funzioni-grafico/177-step-1-dominio.html)