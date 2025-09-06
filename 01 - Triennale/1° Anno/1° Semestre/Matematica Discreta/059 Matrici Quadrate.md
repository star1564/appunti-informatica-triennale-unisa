---
aliases:
  - Matrice Quadrata
  - Diagonale Principale di una Matrice Quadrata
tags:
  - corsi/matematica/matematica_discreta
paragrafo: Matrici - Tipologie
cssclasses:
  - 
---
>Data una [[058 Matrici|Matrice]] $A$ sui Reali $m\times n$, con $m$ righe ed $n$ colonne, *se $m = n$* allora $A$ è una **matrice quadrata** di ordine $m = n$.

> [!example] <font color="orange">Esempio</font>
>$$A_{\color{red}3\times 3}=
>\begin{pmatrix}
>1 & -3 & 0\\
>0 & 0 & \sqrt{5}\\
>-4 & \frac{1}{6} & 7\\
>\end{pmatrix}$$ 
>Matrice quadrata di ordine **3**.

## Diagonale Principale
Se la matrice è quadrata, si dice **diagonale principale** l'insieme di elementi *con indice di riga uguale all'indice di colonna*.

$$A_{m\times n}=
\begin{pmatrix}
\color{red}a_{1, 1} & \dots & a_{1, n}\\
\vdots & \color{red}\ddots & \vdots\\
a_{m, 1} & \dots & \color{red}a_{m, n}\\
\end{pmatrix}$$

## Matrice Identità
La **matrice identità** di una matrice quadrata, è una matrice quadrata che ha *tutti 1 sulla diagonale principale* e *zeri* nelle restanti posizioni.

$$A_{n\times n}=
\begin{pmatrix}
\textcolor{#CC241D}1 & 0 & \dots & 0 \\
0 & \textcolor{#CC241D}1 & \dots & 0 \\
\dots & \dots & \textcolor{#CC241D}\dots & \dots \\
0 & 0 & \dots & \textcolor{#CC241D}1 \\
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