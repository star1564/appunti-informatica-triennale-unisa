---
aliases: 
  - Massimi e Minimi Locali
tags:
  - corsi/matematica/analisi
paragrafo: Derivate
cssclasses:
  - 
---
>I **Massimi e Minimi Relativi e Assoluti** di una [[011 Funzioni|funzione]] sono rispettivamente i [[008 Massimo e Minimo di un insieme#Massimo|massimi]] ed i [[008 Massimo e Minimo di un insieme#Minimo|minimi]] valori che una funzione realizza localmente o globalmente.

Le corrispondenti ascisse vengono dette *punti di massimo e di minimo* (relativi o assoluti).

![[Pasted image 20241003144123.png|600]]


## Relativi (o Locali)
### Massimo Relativo (o Locale)
>Sia $f(x)$ una funzione definita nel dominio $A$. Il punto $x_0\in A$ si dice **massimo relativo** per $f$ se esiste un [[009 Intorno di un Punto|intorno]] $I(x_0, \epsilon)$ con raggio $\epsilon$ e centro $x_0$ tale che $$f(x_0)\geq f(x),\quad\ \forall x\in I(x_0, \epsilon)$$

In pratica, è il punto all'interno di un *intervallo* in cui la funzione *assume il valore più grande*.

![[Pasted image 20241003142619.png|500]]

### Minimo Relativo (o Locale)
>Sia $f(x)$ una funzione definita nel dominio $A$. Il punto $x_0\in A$ si dice **minimo relativo** per $f$ se esiste un intorno $I(x_0, \epsilon)$ con raggio $\epsilon$ e centro $x_0$ tale che $$f(x_0)\leq f(x),\quad\ \forall x\in I(x_0, \epsilon)$$

In pratica, è il punto all'interno di un *intervallo* in cui la funzione *assume il valore più piccolo*.

![[Pasted image 20241003143317.png|500]]

## Assoluti
### Massimo Assoluto
>Sia $f(x)$ una funzione definita nel dominio $A$. Il punto $x_0\in A$ si dice **punto di massimo assoluto** per $f$, e che $f(x_0)$ è il *massimo assoluto* di $f$, se $$f(x_0)\geq f(x),\quad\ \forall x\in A$$

In pratica, è il punto all'interno di *tutto il dominio* in cui la funzione *assume il valore più grande*.

![[Pasted image 20241003143804.png|500]]

### Minimo Assoluto
>Sia $f(x)$ una funzione definita nel dominio $A$. Il punto $x_0\in A$ si dice **punto di minimo assoluto** per $f$, e che $f(x_0)$ è il *minimo assoluto* di $f$, se $$f(x_0)\leq f(x),\quad\ \forall x\in A$$

In pratica, è il punto all'interno di *tutto il dominio* in cui la funzione *assume il valore più piccolo*.

![[Pasted image 20241003144017.png|500]]





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

- pagina 193, Aracne
- [YouMath](https://www.youmath.it/lezioni/analisi-matematica/derivate/258-massimi-e-minimi-relativi-e-assoluti-definizioni.html)