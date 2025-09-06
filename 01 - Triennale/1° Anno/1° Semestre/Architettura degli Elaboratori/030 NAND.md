---
aliases: 
tags:
  - corsi/informatica/architettura_degli_elaboratori
paragrafo: Componenti Logici
cssclasses:
  - 
---
NAND (x, y), con x, y variabili che possono assumere 1 o 0, è la negazione di un AND.

NAND (x, y) corrisponde alla **negazione del prodotto $x\cdot y$**, ovvero **$\overline{x\cdot y}$**.

> [!warning] Se non si vede la linea sopra la $x\cdot y$, ingrandisci il testo con `CTRL` + `rotellina mouse su`

Tavola di Verità:

| x   | y   | $x\cdot y$ | **$\overline{x\cdot y}$** |
| --- | --- | ---------- | ------------------------- |
| 0   | 0   | 0          | 1                         |
| 0   | 1   | 0          | 1                         |
| 1   | 0   | 0          | 1                         |
| 1   | 1   | 1          | 0                         |

Disegno:
![[Pasted image 20211029113743.png|400]]

## RETI ALL NAND

È possibile ricreare le porte AND, OR e NOT solamente con diverse porte NAND.

#### NOT
$NOT(x)=\overline{x}=\overline{x\cdot x} = NAND(x, x)$

![[Pasted image 20211029114652.png|400]]

#### AND
$AND(x, y)= x\cdot y = \overline{\overline{x\cdot y}} =$ 
$= \overline{\overline{x\cdot y}\cdot \overline{x\cdot y}} = NAND(NAND(x,y), NAND(x,y))$

![[Pasted image 20211029120008.png|550]]

#### OR
$OR(x, y)= x + y = \overline{\overline{x + y}} =$ 
$= \overline{\overline{x\cdot x + y\cdot y}} = \overline{\overline{x\cdot x} \cdot \overline{y\cdot y}} = NAND(NAND(x,x), NAND(x,x))$

![[Pasted image 20211029120105.png|550]]


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