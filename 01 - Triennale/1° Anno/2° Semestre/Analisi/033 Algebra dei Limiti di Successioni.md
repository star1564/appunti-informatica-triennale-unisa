---
aliases: 
tags:
  - corsi/matematica/analisi
paragrafo: Limiti di Successioni
cssclasses:
  - 
---
> [!info] Limiti di Funzioni
> Per i limiti delle funzioni andare [[037 Limiti di Funzioni|qui]].

## Operazioni sui Limiti di Successioni Convergenti
Si supponga di avere due [[029 Successioni|successioni reali]] $\{a_n\}_n$ e $\{b_n\}_n$ che *[[032 Limiti di Successioni#Convergente|convergono]]* a due numeri reali $a, b\in\mathbb{R}$, ovvero che:
1. $\lim\limits_{n \to +\infty}a_n=a$
2. $\lim\limits_{n \to +\infty}b_n=b$

### Somma e Sottrazione
$$\lim\limits_{n \to +\infty}(a_n\pm b_n)=\lim\limits_{n \to +\infty}a_n\pm \lim\limits_{n \to +\infty}b_n=\color{#61AFEF}a \pm b$$

### Prodotto tra due Limiti 
$$\lim\limits_{n \to +\infty}(a_n\cdot b_n)=\lim\limits_{n \to +\infty}a_n\cdot \lim\limits_{n \to +\infty}b_n=\color{#61AFEF}a \cdot b$$

### Divisione tra due Limiti
$$\lim\limits_{n \to +\infty}\Big(\frac{a_n}{b_n}\Big)=\frac{\lim\limits_{n \to +\infty}a_n}{\lim\limits_{n \to +\infty}b_n}=\color{#61AFEF}\frac{a}{b},\quad b\neq0$$

### Prodotto del Limite per uno Scalare
$$\lim\limits_{n \to +\infty}\lambda a_n=\lambda\lim\limits_{n \to +\infty} a_n=\color{#61AFEF}\lambda a$$

## Operazioni sui Limiti di Successioni Divergenti
Si supponga di avere una [[029 Successioni|successione]] $\{a_n\}_n$ che *[[032 Limiti di Successioni#Divergente|diverge]]*. Allora, si hanno le seguenti operazioni, che vanno sostituite nelle equazioni pertinenti (dove $\infty=n$ ed $a$ è un numero qualsiasi):
- $a+\infty=+\infty$
- $a-\infty=-\infty$
- $+\infty+\infty=+\infty$
- $-\infty-\infty=-\infty$
- $a\cdot \infty=\pm\infty$ (dipende dal segno di $a$)
- $\frac{a}{\pm\infty}=0$
- $\frac{\infty}{a}=\pm\infty$ (dipende dal segno di $a$)


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
- Pagina Libro: 107
- [YouMath](https://www.youmath.it/lezioni/analisi-matematica/successioni/425-algebra-dei-limiti-di-successioni-parte-2.html)