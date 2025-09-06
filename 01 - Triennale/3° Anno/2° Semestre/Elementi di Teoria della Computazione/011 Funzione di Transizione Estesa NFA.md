---
aliases: 
tags:
  - corsi/informatica/elementi_teoria_computazione
paragrafo: Automi Non Deterministici
cssclasses:
  - 
---
La funzione di transizione estesa negli NFA rappresenta tutti gli stati raggiungibili sia con le $\epsilon$-transizioni, sia con le transizioni su simboli.

> [!quote] Definizione Formale [HUM, 2.5.4]
>Sia $A=(Q,\Sigma,\delta,q_0,F)$ un automa finito non deterministico. La **funzione di transazione estesa** per gli *NFA* $\hat\delta:Q\times\Sigma^*\to\mathcal{P}(Q)$ è definita ricorsivamente come segue
>- **Passo Base**: $\forall q\in E(q)$ 
>$$\color{#61AFEF}\hat\delta(q,\epsilon)=E(q)$$
>Se l'etichetta del cammino è $\epsilon$, si possono seguire solo archi etichettati e uscenti da $q$; questo è proprio il modo in cui la [[010 ε-chiusura negli NFA|epsilon-chiusura]] viene definita.
>- **Passo Ricorsivo**: $\forall q \in Q, w\in\Sigma^*, a\in\Sigma$,
>$$\color{#61AFEF}\hat\delta(q,wa)=E\bigg(\bigcup_{p\in\hat\delta(q,w)}\delta(p,a)\bigg)=\bigcup_{p\in\hat\delta(q,w)}E(\delta(p,a))$$

In generale, *NON* è vero che $\hat\delta(q,a)=\delta(q,a)$.

> [!example]- <font color="orange">Esempio</font>
>![[Pasted image 20240320173046.png]]
>$ca$ non viene accettata.


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