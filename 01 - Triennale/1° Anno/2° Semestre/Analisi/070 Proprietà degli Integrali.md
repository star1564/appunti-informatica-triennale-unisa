---
aliases: 
tags:
  - corsi/matematica/analisi
paragrafo: Integrali
cssclasses:
  - 
---
>Le **Proprietà degli Integrali** sono una serie di proprietà dell'[[066 Integrali#Definizione di Integrale secondo Riemann|integrale definito secondo Riemann]] e degli [[068 Integrali Indefiniti|integrali indefiniti]], relative ad alcune operazioni algebriche di base e alla relazione tra il segno di integrale e l'intervallo di integrazione.

## Definiti e Indefiniti
### Addizione e Sottrazione di Integrali

$$\int_a^b [f(x)\textcolor{#61AFEF}\pm g(x)]\,dx=\int_a^b f(x)dx\ \textcolor{#61AFEF}\pm\int_a^b g(x)\,dx$$

$$\int [f(x)\textcolor{#61AFEF}\pm g(x)]\,dx=\int f(x)\,dx\ \textcolor{#61AFEF}\pm\int g(x)\,dx$$


### Omogeneità dell'integrale

$$\int_a^b [\textcolor{#61AFEF}c\cdot f(x)]\,dx=\textcolor{#61AFEF}c\cdot \int_a^b f(x)\,dx,\quad c\in\mathbb{R}$$


$$\int [\textcolor{#61AFEF}c\cdot f(x)]\, dx=\textcolor{#61AFEF}c\cdot \int f(x)\,dx,\quad c\in\mathbb{R}$$

## Definiti
### Proprietà degli estremi di integrazione

$$\int_a^b f(x)\,dx=\textcolor{#61AFEF}-\int_\textcolor{#61AFEF}b^\textcolor{#61AFEF}a f(x)\,dx$$
### Proprietà degli estremi coincidenti
$$\int_\textcolor{#61AFEF}a^\textcolor{#61AFEF}a f(x)\,dx=0$$

### Linearità dell'integrale rispetto agli estremi
$$\int_a^b f(x)\,dx=\int_a^c f(x)\,dx\ +\int_c^b f(x)\,dx,\quad c\in[a,b]$$![[Pasted image 20220507122333.png]]

### Valore Assoluto dell'integrale

$$\Bigg|\int_a^b f(x)\,dx\Bigg|\leq\int_a^b |f(x)|\,dx$$


### Primitive di Derivate di Funzioni Composte
Sia $F(x)$ una primitiva di $f(x)$ e sia $g(x)$ una funzione [[046 Derivata di una Funzione#Funzione Derivabile|derivabile]] tale che sia possibile costruire la funzione composta $F(g(x))$. Sia ha che:
$$[F(g(x))]'=f(g(x))\cdot g'(x)$$

> [!example]+ <font color="orange">Esempio</font>
>$$\int_a^b \textcolor{green}{\underbracket{3x^2}_{g(x)}}\ \textcolor{#61AFEF}{\underbracket{\sin(x^3)}_{f(g(x))}} \,dx= \textcolor{#fdaa39}{\underbracket{-\cos(x^3)}_{F(g(x))}}$$

## Indefiniti

### Derivata dell'Integrale
Questo vale per funzione che ammette [[067 Primitiva di una Funzione|primitive]]. 

$$\frac{d}{dx}\Bigg[\int f(x)dx\Bigg]=f(x)$$


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
- [YouMath](https://www.youmath.it/lezioni/analisi-matematica/integrali/627-proprieta-degli-integrali-definiti.html)
- [Video Elia Bombardelli](https://youtu.be/4hfhVhnzuUw?t=394)
