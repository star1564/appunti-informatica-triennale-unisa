---
aliases: 
tags:
  - corsi/matematica/metodi_matematici_informatica
paragrafo: Logica Proposizionale
cssclasses:
  - 
---
>Nella logica proposizionale, l'**Equivalenza Bicondizionale** (*IFF*, `if and only if`) di due [[001 Logica Proposizionale|proposizioni]] $p$ e $q$ rappresenta una condizione `se e solo se` ed è definita con: $$\color{#CC241D}p\Leftrightarrow q$$

La proposizione composta si legge "$p$ `se e solo se` $q$" ed è: 
- `Vera` - se $p$ e $q$ *hanno lo stesso valore*
- `Falsa` - altrimenti


| $p$ | $q$ | $\color{#CC241D}p\Leftrightarrow q$ |
| --- | --- | ----------------------------------- |
| V   | V   | V                                   |
| V   | F   | F                                   |
| F   | V   | F                                   |
| F   | F   | V                                   |

> [!quote] Doppia implicazione
> L'equivalenza bicondizionale è [[007 Equivalenza Logica|logicamente equivalente]] alla [[002 AND (MMI)|congiunzione]] delle seguenti due [[006 Implicazione|implicazioni]] vere: $$\color{#CC241D}p\Leftrightarrow q\equiv (p\Rightarrow q)\ \land\ (q\Rightarrow p)$$

> [!quote] Equivalenza con il negato
> $$\textcolor{#CC241D}{p\Leftrightarrow q \equiv} (p\Rightarrow q)\ \land\ (q\Rightarrow p)$$
> $$\equiv (\neg q \Rightarrow \neg p) \land (\neg p \Rightarrow \neq q)$$
> $$\equiv (\neg p \Rightarrow \neg q) \land (\neg q \Rightarrow \neq p)$$
> $$\color{#CC241D}\equiv \neg p\Leftrightarrow \neg q$$

^63c4d0


___
[[000 Indice MMI|↖ Ritorna all'indice ↖]]

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