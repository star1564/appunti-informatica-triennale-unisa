---
aliases: 
tags:
  - corsi/matematica/ricerca_operativa
paragrafo: Vettori
cssclasses:
  - 
---
> [!error] !!! Le note di Ricerca Operativa non sono complete e in parte sono errate !!!

## Indipendenza
>I vettori $\underline{v_1},\underline{v_2}, \dots,\underline{v_n}$ sono **Linearmente Indipendenti** se  $$\color{#CC241D}\lambda_1\underline{v_1}+\lambda_2\underline{v_2}+\dots+\lambda_n\underline{v_n}=0$$ il che implica *solamente* che $$\color{#61AFEF}\lambda_1=0,\lambda_2=0,\dots,\lambda_n=0$$

In altre parole, se $\underline{v_1},\underline{v_2}, \dots,\underline{v_n}$ sono linearmente indipendenti, allora *l'unico modo* per ottenere il [[002 Vettori#Vettore Nullo|Vettore Nullo]], dalla loro [[004 Combinazione Lineare, Conica e Convessa#Combinazione Lineare|combinazione lineare]], è quello di porre tutti i coefficienti $\lambda$ a zero.

## Dipendenza
>I vettori $\underline{v_1},\underline{v_2}, \dots,\underline{v_n}$ sono **Linearmente Dipendenti** se esistono dei coefficienti reali *non tutti nulli* $\lambda_1,\lambda_2,\dots,\lambda_n$ tali che $$\color{#CC241D}\lambda_1\underline{v_1}+\lambda_2\underline{v_2}+\dots+\lambda_n\underline{v_n}=0$$con $$\color{#61AFEF}\lambda_1\neq0\lor\lambda_2\neq0\lor\dots\lor\lambda_n\neq0$$ 


In particolare, i vettori $\underline{v_1},\underline{v_2}, \dots,\underline{v_n}$ sono Linearmente Dipendenti se uno di essi può essere espresso come [[004 Combinazione Lineare, Conica e Convessa#Combinazione Lineare|combinazione lineare]] degli altri.

> [!example]- <font color="orange">Esempio</font>
>$$\underline{v}_1^T=[1\ 2\ 3], \underline{v}_2^T=[-1\ 1\ -1], \underline{v}_3^T=[0\ 3\ 2]$$
>Sono linearmente dipendenti perché $\lambda_1\underline{v_1}+\lambda_2\underline{v_2}+\dots+\lambda_n\underline{v_n}=0$ quando $\color{#61AFEF}\lambda_1=\lambda_2=1, \lambda_3=-1$.
>
>$$1\cdot[1\ 2\ 3]+ 1\cdot[-1\ 1\ -1]- 1\cdot[0\ 3\ 2] = [0\ 0\ 0]$$
>
>Infatti si ha che $\underline{v}_1^T+\underline{v}_2^T=\underline{v}_3^T$.
>
>---
>I vettori $\underline{y}$, $\underline{v_1}$ e $\underline{v_2}$ sono linearmente dipendenti.
>![[Pasted image 20240323181017.png|500]]

___
[[000 Indice RO|↖ Ritorna all'indice ↖]]

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