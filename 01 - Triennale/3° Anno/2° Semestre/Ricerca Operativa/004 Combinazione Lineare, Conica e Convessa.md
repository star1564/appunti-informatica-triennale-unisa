---
aliases:
  - Spazio Generato
  - Combinazione Lineare (RO)
  - Combinazione Conica
  - Combinazione Convessa
tags:
  - corsi/matematica/ricerca_operativa
paragrafo: Vettori
cssclasses:
  - 
---
> [!error] !!! Le note di Ricerca Operativa non sono complete e in parte sono errate !!!

## Combinazione Lineare
>Un vettore $\underline{y}$ è detto **Combinazione Lineare** dei vettori $\underline{v_1}, \underline{v_2}, \dots, \underline{v_n}$ se *esistono dei coefficienti reali* $\lambda_1,\lambda_2,\dots,\lambda_n$ tali che $$\color{#CC241D}\underline{y}=\lambda_1\underline{v_1}+\lambda_2\underline{v_2}+\dots+\lambda_n\underline{v_n}$$

> [!example]- <font color="orange">Esempio</font>
>![[Pasted image 20240323175019.png|600]]
>
>![[Pasted image 20240323175036.png|600]]
>
>![[Pasted image 20240323175057.png|600]]



Prendendo in esempio due vettori $\underline{v_1},\underline{v_2}$, se $\color{#61AFEF}\underline{v_1}=k\underline{v_2}$, allora non esistono due coefficienti reali $\lambda_1,\lambda_2$ per i quali vale $\underline{y}=\lambda_1\underline{v_1}+\lambda_2\underline{v_2}$.

![[Pasted image 20240323174853.png|300]]

### Spazio Generato
>Un insieme di vettori $\underline{v_1},\underline{v_2}, \dots,\underline{v_k}$ di dimensione $n$ **genera** l'insieme di vettori $\mathbb{R}^n$, se *ogni* vettore in $\mathbb{R}^n$ *può essere rappresentato come [[004 Combinazione Lineare, Conica e Convessa#Combinazione Lineare|combinazione lineare]]* dei vettori $\underline{v_1},\underline{v_2}, \dots,\underline{v_k}$.

## Combinazione Conica
>Un vettore $\underline{y}$ è detto **Combinazione Conica** dei vettori $\underline{v_1}, \underline{v_2}, \dots, \underline{v_n}$ se esistono dei coefficienti reali $\lambda_1,\lambda_2,\dots,\lambda_n$ tali che: 
>1. $\color{#61AFEF}\lambda_1,\lambda_2,\dots,\lambda_n\geq 0$
>2. $\underline{y}=\lambda_1\underline{v_1}+\lambda_2\underline{v_2}+\dots+\lambda_n\underline{v_n}$

Ovvero se il vettore $\underline{y}$ si trova tra i vettori (area verde).

![[Pasted image 20240323175555.png|400]]


## Combinazione Convessa
>Un vettore $\underline{y}$ è detto **Combinazione Convessa** dei vettori $\underline{v_1}, \underline{v_2}, \dots, \underline{v_n}$ se *esistono dei coefficienti reali* $\lambda_1,\lambda_2,\dots,\lambda_n$ tali che: 
>1. $\lambda_1,\lambda_2,\dots,\lambda_n\geq 0$
>2. $\color{#61AFEF}\lambda_1+\lambda_2+\dots+\lambda_n= 1$
>3. $\underline{y}=\lambda_1\underline{v_1}+\lambda_2\underline{v_2}+\dots+\lambda_n\underline{v_n}$

![[Pasted image 20240323175743.png|400]]


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
- [[056 Sottospazi#COMBINAZIONE LINEARE E SOTTOSPAZIO GENERATO|Da Matematica Discreta]]