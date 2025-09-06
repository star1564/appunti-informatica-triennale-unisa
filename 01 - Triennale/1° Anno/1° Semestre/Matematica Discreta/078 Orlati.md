---
aliases: 
tags:
  - corsi/matematica/matematica_discreta
paragrafo: Matrici - Terminologie
cssclasses:
  - 
---
Sia $A$ una matrice qualunque e sia $A'$ una [[066 Sottomatrici|Sottomatrice]] di $A$.

>Un **orlato** è una matrice che *contiene come sottomatrice $A'$*, a cui si aggiungono delle righe e colonne di $A$ ad essa.

> [!example]+ <font color="orange">Esempio</font>
>$$A_{4\times 5}=
\begin{pmatrix}
\color{#61AFEF}1 & \color{#61AFEF}0 & -3 & 4 & 1\\
\color{#61AFEF}0 & \color{#61AFEF}1 & 2 & -3 & -1\\
1 & 1 & -1 & 1 & 0\\
2 & 0 & -6 & 8 & 2\\
\end{pmatrix}$$
>
>$$\textcolor{#61AFEF}{Sottomatrice}=
\begin{vmatrix}
1 & 0\\
0 & 1\\
\end{vmatrix}$$
>
>Riga **3**, Colonne variabili (3$\sim$ 5).
>$Orlato\space1_{(r=3, c=3)}=
\begin{pmatrix}
1 & 0 & -3\\
0 & 1 & 2\\
1 & 1 & -1\\
\end{pmatrix}$ $Orlato\space2_{(r=3, c=4)}=
\begin{pmatrix}
1 & 0 & 4\\
0 & 1 & -3\\
1 & 1 & 1\\
\end{pmatrix}$
>$$...$$
>
>Riga **4**, Colonne variabili (3$\sim$ 5).
>$Orlato\space4_{(r=4, c=3)}=
\begin{pmatrix}
1 & 0 & -3\\
0 & 1 & 2\\
2 & 0 & -6\\
\end{pmatrix}$ $Orlato\space5_{(r=4, c=4)}=
\begin{pmatrix}
1 & 0 & 4\\
0 & 1 & -3\\
2 & 0 & 8\\
\end{pmatrix}$
>$$...$$



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