---
aliases: [Matrici Trasposte, Trasposta]
tags:
  - corsi/matematica/matematica_discreta
paragrafo: Matrici - Tipologie
cssclasses:
  - 
---
>Data una [[058 Matrici|Matrice]] $A$ sui Reali $m\times n$, con $m$ righe ed $n$ colonne, si chiama **Trasposta** di $A$ la matrice che si ottiene *scambiando le righe e le colonne* di $A$, ovvero $n\times m$. Si indica con $$\color{red} A^T$$

Se la matrice è [[059 Matrici Quadrate|quadrata]], allora $A^T$ avrà ordine $n = m$.

In generale $A \neq A^T$, ma *se $A = A^T$* si dice che A è **simmetrica**.

> [!example]+ <font color="orange">Esempio</font>
$A_{4\times 3}=
\begin{pmatrix}
\color{red}-5 & \color{cyan}0 & \color{yellow}2\\
\color{red}1 & \color{cyan}\frac{1}{2} & \color{yellow}-3\\
\color{red}0 & \color{cyan}11 & \color{yellow}0\\
\color{red}1 & \color{cyan}-1 & \color{yellow}1\\
\end{pmatrix},\quad %-----------MATRICE 2% 
A^T_{3\times 4}=
\begin{pmatrix}
\color{red}-5 & \color{red} 1 & \color{red} 0 & \color{red} 1\\
\color{cyan} 0 & \color{cyan} \frac{1}{2} & \color{cyan} 11 & \color{cyan} -1\\
\color{yellow} 2 & \color{yellow} -3 & \color{yellow} 0 & \color{yellow} 1\\
\end{pmatrix}$

#### Proprietà
- $(A^T)^T=A$
- $(A+B)^T=A^T+B^T$ (quando la [[070 Somma tra Matrici|somma]] è definita)

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
- Pagina Libro: 258