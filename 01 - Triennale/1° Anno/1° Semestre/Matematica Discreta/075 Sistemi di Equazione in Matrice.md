---
aliases: 
tags:
  - corsi/matematica/matematica_discreta
paragrafo: Matrici - Sistemi
cssclasses:
  - 
---
>Un **sistema di equazioni lineari** nelle incognite $x_1,x_2,...,x_n$, con n incognite ed m equazioni, è del tipo:
>
>$$\begin{cases}
a_{1,1}x_1+a_{1,2}x_2+...+a_{1,n}x_n = b_1\\
a_{2,1}x_1+a_{2,2}x_2+...+a_{2,n}x_n = b_2\\
\quad\quad\quad\quad\quad\quad\quad\vdots\\
a_{m,1}x_1+a_{m,2}x_2+...+a_{m,n}x_n = b_m\\
\end{cases}
>$$

Risolvere questo sistema significa determinare tutte le n-uple $(\alpha_1, ..., \alpha_n)$ di numeri reali che *soddisfano tutte le m equazioni*.

#### CASI POSSIBILI DI SOLUZIONI
Indichiamo con S l'insieme delle *soluzioni*, può succedere che:
- $S = \emptyset$, ovvero che il sistema **non ammette soluzioni**. Si dice che il sistema **non è compatibile**. ^aa07f7
- $S = \{(\alpha_1, ..., \alpha_n)\}$ è costituito di un unico elemento per incognita, pertanto il sistema ammette **una ed una sola soluzione**; ^707acb
- $S$ è infinito, cioè il sistema ammette **infinite soluzioni**.

#### ASSOCIAZIONE DI UNA MATRICE
Al sistema si può **associare una matrice**, dove il numero di righe è uguale al numero delle equazioni, mentre il numero di colonne è uguale al numero di incognite+1. *Questa si chiama matrice completa del sistema*:
$$
\begin{pmatrix}
a_{1,1}x_1+a_{1,2}x_2+...+a_{1,n}x_n = b_1\\
a_{2,1}x_1+a_{2,2}x_2+...+a_{2,n}x_n = b_2\\
\vdots\\
a_{m,1}x_1+a_{m,2}x_2+...+a_{m,n}x_n = b_m\\
\end{pmatrix}$$

Le soluzioni di questa matrice (che sono le stesse per il sistema di equazioni a cui questa matrice è stata associata) si possono trovare con il [[073 Algoritmo di Gauss|metodo di Gauss]].

*Se due matrici A e B sono equivalenti*, allora il sistema la cui matrice completa è A ed il sistema la cui matrice completa è B sono equivalenti; pertanto *hanno le stesse soluzioni*.

> [!example] <font color="orange">Esempio</font>
>![[Pasted image 20211025111314.png]]


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
- Pagina Libro: 285