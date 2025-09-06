---
aliases:
  - Cardinalità come una Funzione
  - Cardinalità insiemi INFINITI
tags:
  - corsi/informatica/elementi_teoria_computazione
paragrafo: Indecidibilità
cssclasses:
  - 
---
## Cardinalità come una Funzione
> Due insiemi $X$ e $Y$ hanno la **stessa [[001 Insieme#Cardinalità (Ordine) di un Insieme Finito|cardinalità]]** se esiste una funzione [[021 Applicazioni Biettive|biettiva]] $f:X\to Y$ di $X$ su $Y$. $$\color{#CC241D}|X|=|Y|\iff \text{esiste una funzione biettiva } f:X\to Y $$

![[Pasted image 20211029143407.png|400]]

> [!tip]+ Determinare l'insieme "più grande"
>Esistono le seguenti condizioni che *si possono anche combinare tra di loro*:
>- <font color="#FF6611">Non esiste una funzione [[021 Applicazioni Biettive|biettiva]] $f:X\to Y$</font> $\iff|X| \textcolor{#FF6611}\ne |Y|$
>- <font color="#FF4C4E">Esiste una funzione [[020 Applicazioni Iniettive|iniettiva]] $f:X\to Y$</font> $\iff|X| \textcolor{#FF4C4E}\le |Y|$
>- $\textcolor{#DD53DF}{|X|\le|Y|,|X|\ne|Y|}\Longrightarrow|X| \textcolor{#DD53DF}< |Y|$
>- $\textcolor{#9BDF53}{|X|\le|Y|,|Y|\le|X|}\Longrightarrow|X| \textcolor{#9BDF53}= |Y|$

^8e6455


## Metodo della diagonalizzazione per la Cardinalità
Il **metodo della diagonalizzazione** è stato creato per confrontare la [[001 Insieme#Cardinalità (Ordine) di un Insieme Finito|Cardinalità]] di due *insiemi infiniti* e vedere quali dei due è il più "grande".

Per due insiemi finiti che hanno la stessa cardinalità, si può notare che gli elementi dell'uno possono essere *messi in corrispondenza uno a uno* con quelli dell'altro. Questo metodo confronta le dimensioni senza ricorrere al conteggio.

> [!example]- <font color="orange">Esempio</font>
>Se $A=\{1,2,3\},B=\{4,5\}$
>
>| $A$ | $B$ |
>| --- | --- |
>| $1$ | $4$ |
>| $2$ | $5$ |
>| $3$ |     |
>
>In questo caso $|A|\neq|B|$.

Questo concetto viene esteso agli insiemi infiniti.


Il metodo della diagonalizzazione fu successivamente usato per dimostrare che *[[040 Linguaggi Non Turing-Riconoscibili|esistono linguaggi NON Turing-Riconoscibili]]*.



___
[[000 Indice ETC|↖ Ritorna all'indice ↖]]

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