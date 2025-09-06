---
aliases: 
tags:
  - corsi/matematica/matematica_discreta
paragrafo: Matrici - Operazioni
cssclasses:
  - 
---
>Questo algoritmo serve per **ridurre a [[061 Matrici a Scala|scala]]** una matrice.
>Data una [[058 Matrici|Matrice]] qualunque, se questa non è nulla significa che c'è almeno una colonna non nulla. 

1. Si va a prendere la prima colonna non nulla da sinistra;

$$A_{3\times 4}=
\begin{pmatrix}
\color{#61AFEF}1 & 2 & 3 & 1\\
\color{red}4 & 5 & 6 & 1\\
\color{red}7 & 8 & 8 & 1\\
\end{pmatrix}$$

2. Si considera l'intera matrice. Si individuano il *pivot* e gli **elementi al di sotto da eliminare**. Si modificano le righe con operazioni elementari. Alpha si trova in questo modo: 

**numero da eliminare** + (*numero pivot della [[066 Sottomatrici|Sottomatrice]]* $\times \space \alpha$)

![[Pasted image 20211025104202.png]]

![[Pasted image 20211025104932.png]]

3. Si considera la sottomatrice (da $M_{1,1}$ a $M_{3,4}$). Si individuano il *pivot* e gli **elementi al di sotto da eliminare**. Si modificano le righe con le operazioni elementari. <font color="#8a6700">Non si considerano le righe al di fuori della sottomatrice</font>.
  
![[Pasted image 20211025105018.png]]

4. ...Si continua fino a quando i pivot sono finiti.

$$A_{3\times 4}=
\begin{pmatrix}
\color{green}1 & \color{green}2 & \color{green}3 & \color{green}1\\
0 & \color{green}-3 & \color{green}-6 & \color{green}-3\\
0 & 0 & \color{green}-1 & \color{green}0\\
\end{pmatrix}$$

___
[[000 Indice MD|↖ Ritorna all'indice ↖]]

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
- Pagina Libro: 