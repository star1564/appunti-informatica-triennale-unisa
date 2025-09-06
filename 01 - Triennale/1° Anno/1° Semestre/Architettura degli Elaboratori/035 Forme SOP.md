---
aliases: 
tags:
  - corsi/informatica/architettura_degli_elaboratori
paragrafo: Componenti Logici
cssclasses:
  - 
---
## FORMA NORMALE SOP

Un'espressione booleana è in **Forma Normale SOP** quando c'è una Somma di Prodotti (**S**um **O**f **P**roducts), ovvero quando c'è l'OR di AND di [[025 Funzioni Booleane (AdE)#Letterale|letterali]].

<font color="orange">Esempio</font>: $f=\{x_1, x_2, x_3\}$
$x_1\overline{x_2}x_3+x_1\overline{x_3}+x_1x_3$

## FORMA CANONICA SOP

Una espressione normale SOP è in **Forma Canonica SOP** se i suoi termini *sono tutti [[Mintermine|mintermini]]*.

<font color="orange">Esempio</font>: $f=\{x_1, x_2, x_3\}$
$x_1\overline{x_2}x_3+x_1x_2\overline{x_3}+x_1x_2x_3$

## CONVERSIONI FORME SOP
### FORMA CANONICA → FORMA NORMALE
- Si applica la proprietà *[[025 Funzioni Booleane (AdE)#^7cebf1|distributiva]]*, *sciogliendo tutte le parentesi*;
- *Si eliminano i termini ridondanti*, usando le proprietà di [[025 Funzioni Booleane (AdE)#^e30afa|idempotenza]] e [[025 Funzioni Booleane (AdE)#^fbfeb7|complementarietà]].

### FORMA NORMALE → FORMA CANONICA
- Se un termine prodotto non contiene il letterale $x_i$, si moltiplica per *($x_i+\overline{x_i}$)*;
- Si applica la proprietà *[[025 Funzioni Booleane (AdE)#^7cebf1|distributiva]]*, *sciogliendo tutte le parentesi*;
- *Si eliminano i termini ridondanti*, usando le proprietà di [[025 Funzioni Booleane (AdE)#^e30afa|idempotenza]] e [[025 Funzioni Booleane (AdE)#^fbfeb7|complementarietà]].

### TAVOLA DI VERITÀ → FORMA CANONICA SOP
Se la funzione logica è un minitermine, la sua tavola di verità *ha un solo 1* e viceversa.

Se invece la tavola di verità ha più *corrispondenze di 1*, troviamo i minitermini corrispondenti e li sommiamo.

> [!example]- <font color="orange">Esempio</font>
>
| $x_3$ | $x_2$ | $x_1$ | **$f$** |
| ----- | ----- | ----- | ------- |
| 0     | 0     | 0     | 0       |
| 0     | 0     | 1     | 0       |
| 0     | 1     | 0     | 0       |
| *0*   | *1*   | *1*   | *1*     |
| 1     | 0     | 0     | 0       |
| *1*   | *0*   | *1*   | *1*     |
| 1     | 1     | 0     | 0       |
| 1     | 1     | 1     | 0       |
>Allora la forma canonica SOP è:
>$f=\overline{x_3}\cdot x_2\cdot x_1 + x_3\cdot \overline{x_2}\cdot x_1$


___
[[000 Indice AdE|↖ Ritorna all'indice ↖]]

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