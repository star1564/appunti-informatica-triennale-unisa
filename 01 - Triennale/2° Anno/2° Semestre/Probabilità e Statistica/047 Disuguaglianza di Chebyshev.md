---
aliases: 
tags:
  - corsi/matematica/probabilità_statistica
inglese: 
paragrafo: Teoremi Limite
cssclasses:
  - 
---
> [!note] Nota
> - $E[X]=\mu$
> - $\text{Var}(X)=\sigma^2$

>Se $X$ è una [[023 Variabili Aleatorie|Variabile Aleatoria]] con [[026 Valore Atteso di una VA Discreta|valore atteso]] $\mu$ e [[027 Varianza di una VA Discreta|varianza]] $\sigma^2$, entrambe finite, allora per ogni numero reale $t>0$ si ha una **Disuguaglianza di Chebyshev** se 
>
>$$\mathbb{P}(|X-\mu|\geq t)\leq \frac{\sigma^2}{t^2}$$

Ovvero che "la probabilità che $X$ sia ad almeno $t$ distante dal suo valore atteso non può essere troppo grande".

![[Pasted image 20250319151151.png]]

Questa disuguaglianza:
- Richiede il [[026 Valore Atteso di una VA Discreta|Momento di Ordine 2]] (la varianza)
- È valida per ogni variabile aleatoria (anche con valori negativi) con una varianza definita

È un'applicazione della [[046 Disuguaglianza di Markov|Disuguaglianza di Markov]].

> [!quote] Dimostrazione
> Essendo $(X-\mu)^2$ una variabile aleatoria non negativa, possiamo applicare la [[046 Disuguaglianza di Markov|Disuguaglianza di Markov]], con $a=k^2$, dove $\mathbb{P}(X\geq a)\leq\frac{E[X]}{a}$ diventa 
> $$\mathbb{P}((X-\mu)^2\geq k^2)\leq \frac{E[(X-\mu)^2]}{k^2}$$
> 
> Essendo $(X-\mu)^2\geq k^2$ se e solo se $|X-\mu|\geq k$, si ricava che $$\frac{\sigma^2}{k^2}$$ ovvero la tesi originale della disequazione di Chebyshev.
> 
> Si può notare che $$\mathbb{P}(|X-\mu|\geq k)=1-\mathbb{P}(|X-\mu|< k)\leq \frac{\sigma^2}{k^2}$$ da cui segue $$\mathbb{P}(\mu-k\lt X\lt \mu+k)=\mathbb{P}(-k\lt X-\mu\lt k)=\mathbb{P}(|X-\mu|\lt k)\geq1-{\frac{\sigma^{2}}{k^{2}}}$$


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
- https://www.youtube.com/watch?v=mlelI1LA9o4