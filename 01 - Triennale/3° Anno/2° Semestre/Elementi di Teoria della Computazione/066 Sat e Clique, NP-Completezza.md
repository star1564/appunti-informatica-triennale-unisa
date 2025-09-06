---
aliases: 
  - Teorema di Cook-Levin
tags:
  - corsi/informatica/elementi_teoria_computazione
paragrafo: NP-completezza
cssclasses:
  - 
---
![[063 NP-Completezza#^68fbfb]]

## Teorema Cook-Levin $(SAT)$
> [[064 SAT e 3SAT#SAT|SAT]] è NP-completo.

Di conseguenza abbiamo che $SAT\in P\iff P=NP$.

Abbiamo già [[064 SAT e 3SAT#^cadc87|provato]] che $SAT\in NP$. 
La prova del teorema di Cook-Levin consiste nel dimostrare che ogni $A\in NP$ è riducibile in tempo polinomiale a $SAT$.

La riduzione in tempo polinomiale si ottiene definendo per ogni input $w$ una formula booleana $\phi$ che simula la [[029 Macchina di Turing Non Deterministica|Macchina di Turing non deterministica]] che decide $A$ sull'input $w$.

## Teorema $SAT_{CNF}$

> [!quote] SAT CNF
>$$SAT_{CNF}=\{\langle \phi \rangle\ |\ \phi\text{ è una formula booleana soddisfacibile in }CNF\}$$

Abbiamo che:
- $SAT_{CNF}\in NP$
- $SAT$ è riducibile in tempo polinomiale in $SAT_{CNF}$, quindi $SAT\leq_p SAT_{CNF}$

>$SAT_{CNF}$ è NP-completo.

> [!note] Nota
> La trasformazione classica di un'espressione booleana nella sua forma normale congiuntiva non definisce, in generale, una riduzione di tempo polinomiale.

## Teorema $3SAT$
>$3SAT$ è NP-completo.

Abbiamo già [[064 SAT e 3SAT#^b1992a|provato]] che $3SAT\in NP$. 

Basta dimostrare che $SAT_{CNF}\leq_p 3SAT$. Consiste nel costruire, a partire da $\phi$ in $CNF$, una formula booleana $\psi$ in $3CNF$ tale che $\phi$ è soddisfacibile se e solo se $\psi$ è soddisfacibile.

Inoltre $\psi$ può essere costruita a partire da $\phi$ in tempo polinomiale.

## Teorema  $CLIQUE$
>[[058 La Classe NP#CLIQUE|CLIQUE]] è NP-completo.

 Sappiamo che : 
1. $CLIQUE\in NP$, lo abbiamo già [[058 La Classe NP#^d1928b|provato]]
2. $3SAT$ è NP-completo
3. $3SAT\leq_p CLIQUE$, lo abbiamo già [[064 SAT e 3SAT#^42a5bf|provato]]

Quindi, per via della [[063 NP-Completezza#^68fbfb|strategia generale]], $CLIQUE$ è NP-completo.

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