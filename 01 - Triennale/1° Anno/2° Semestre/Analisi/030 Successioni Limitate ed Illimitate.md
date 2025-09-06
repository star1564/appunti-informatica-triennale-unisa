---
aliases:
  - Successione Limitata Inferiormente
  - Successione Limitata Superiormente
tags:
  - corsi/matematica/analisi
paragrafo: Successioni
cssclasses:
  - 
---
## Successione Limitata
>Una [[029 Successioni|successione reale]] $\{a_n\}_{n\in\mathbb{N}}$ è **Limitata** se è limitata [[#Successione Limitata Superiormente|superiormente]] che [[#Successione Limitata Inferiormente|inferiormente]], cioè se esistono $m,M\in\mathbb{R}$, tali che:
>$$\color{#61AFEF}m\leq a_n\leq M,\ \forall n\in\mathbb{N}$$

> [!example]+ <font color="orange">Esempio</font>
>- Ogni successione [[031 Successioni Monotone#Successione Costante|costante]] è inferiormente limitata.
>- La successione $a_n=\sin(n)$ è limitata.
>- $\{a_n\}_{n\in\mathbb{N}}=(\frac{1}{n+1})_{n\in\mathbb{N}}$ è limitata. Si ha infatti che $0<a_n\leq1, \forall n\in\mathbb{N}$.

### Successione Limitata Superiormente
>Una [[029 Successioni|successione]] $\{a_n\}_{n\in\mathbb{N}}$ è **Limitata Superiormente** se e solo se esiste un numero $M\in\mathbb{R}$ che *sovrasta tutti i termini* della successione.
>$$\color{#61AFEF}a_n\leq M,\ \forall n\in\mathbb{N}$$
>Ovvero se esiste un [[007 Estremo Inferiore e Superiore di un insieme#ESTREMO SUPERIORE|estremo superiore]].


> [!example]+ <font color="orange">Esempio</font>
>- Ogni successione [[031 Successioni Monotone#Successione Costante|costante]] è inferiormente limitata.
>- La successione il cui termine $n$-esimo è il [[019 Definizione di Seno e Coseno#Seno|seno]] di $n$, ovvero $a_n=\sin(n)$, è inferiormente limitata. Si ha che $m=1$.


### Successione Limitata Inferiormente
>Una [[029 Successioni|successione reale]] $\{a_n\}_{n\in\mathbb{N}}$ è **Limitata Inferiormente** se e solo se esiste un numero $m\in\mathbb{R}$ che è *più piccolo di tutti i termini* della successione.
>$$\color{#61AFEF}m\leq a_n,\ \forall n\in\mathbb{N}$$
>Ovvero se esiste un [[007 Estremo Inferiore e Superiore di un insieme#ESTREMO INFERIORE|estremo inferiore]].

> [!example]+ <font color="orange">Esempi</font>
>- Ogni successione [[031 Successioni Monotone#Successione Costante|costante]] è inferiormente limitata.
>- La successione il cui termine $n$-esimo è il [[019 Definizione di Seno e Coseno#Seno|seno]] di $n$, ovvero $a_n=\sin(n)$, è inferiormente limitata. Si ha che $m=-1$.
>- $\{a_n\}_{n\in\mathbb{N}}=(n^2+1)_{n\in\mathbb{N}}$ è inferiormente limitata. Si ha che $m=1$.

## Successione Illimitata
>Se una successione *non è [[#Successione Limitata|limitata]]*, questa si dice **illimitata**.

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
- [Video - Elia Bombardelli](https://youtu.be/PtQSWL-5GCo?t=252)