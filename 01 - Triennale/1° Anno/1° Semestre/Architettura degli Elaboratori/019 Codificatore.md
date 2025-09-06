---
aliases:
tags:
  - corsi/informatica/architettura_degli_elaboratori
paragrafo: Componenti Logici
cssclasses:
  -
---

>Componente con *m input* ed **n output**, dove:
>$$2^{\color{red}n}\geq \color{#61AFEF}m$$

*Associa ad ogni elemento input* una sequenza di n bit.

*Solo uno degli input può essere 1*, che genererà la combinazione in output. Tutti gli altri saranno 0.

![[Pasted image 20211029123430.png|500]]

Il funzionamento del codificatore è gestito dalla sua [[Tabella di Verità]], dove ad ogni elemento in input viene associato una variazione di bit in output.

Il disegno all'interno del blocco logico sarà composto da molti OR.

> [!example]- <font color="orange">Esempio</font>
> Dati 7 input, ad ognuno di questi viene assegnato un numero da rappresentare in binario con i 4 bit in output.
> Uno solo degli input sarà attivo alla volta.
> 
> |              | $y_3$ | $y_2$ | $y_1$ | $y_0$ |
> | ------------ | ----- | ----- | ----- | ----- |
> | $x_0$ (→ 3)  | 0     | 0     | 1     | 1     |
> | $x_1$ (→ 5)  | 0     | 1     | 0     | 1     |
> | $x_2$ (→ 6)  | 0     | 1     | 1     | 0     |
> | $x_3$ (→ 9)  | 1     | 0     | 0     | 1     |
> | $x_4$ (→ 10) | 1     | 0     | 1     | 0     |
> | $x_5$ (→ 12) | 1     | 1     | 0     | 0     |
> | $x_6$ (→ 13) | 1     | 1     | 0     | 1     |
>
> Si ottiene la [[035 Forme SOP#TAVOLA DI VERITÀ → FORMA CANONICA SOP|forma canonica SOP]] e la si [[034 Sintesi della Rete#MINIMIZZAZIONE|minimizza]].
> 
> ![[Pasted image 20211103090019.png|450]]
> 
> Il disegno si può esprimere più facilmente [[027 OR (AdE)#^bd4d7e|in questo modo]]:
> 
> ![[Pasted image 20211103090114.png|450]]


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