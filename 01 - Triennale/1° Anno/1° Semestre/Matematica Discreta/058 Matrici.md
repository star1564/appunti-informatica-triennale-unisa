---
aliases:
  - Matrice
  - Matrici
tags:
  - corsi/matematica/matematica_discreta
paragrafo: Matrici - Tipologie
cssclasses:
  - 
---
>Una **Matrice** è una struttura dati *bidimensionale* utilizzata per organizzare e rappresentare insiemi di dati numerici o simbolici in *forma tabulare*.

Le matrici sono costituite da *righe* e *colonne*, e ogni elemento di una matrice è posizionato in una specifica riga e colonna, identificato da due indici.

>La dimensione di una matrice è solitamente indicata come $m \times n$, dove $\textcolor{#FF6611}m$ rappresenta il <font color="#FF6611">numero di righe</font> e $\textcolor{#00C575}n$ il <font color="#00C575">numero di colonne</font> ed entrambi sono [[004 Insiemi Numerici Infiniti|numeri positivi interi]].

Il **termine di posto ($i, j$)** in una matrice $A_{m\times n}$ è l'unico numero reale che appartiene alla riga i-esima e alla colonna j-esima di A. Si indica con $a_{i, j}$.

> [!example] <font color="orange">Esempio</font>
>$$A_{3\times 4}=
>
>\begin{pmatrix}
>\sqrt{2} & -4 & 0 & \frac{1}{2}\\
>3 & 0 & 0 & 1\\
>\pi & -\frac{2}{3} & 1 & 0\\
>\end{pmatrix}$$
>
>3 righe e 4 colonne.


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

<center><font color="rainbow"><i>"The Matrix has you..."</i></font></center>

Altri collegamenti: 
- Pagina Libro: 257