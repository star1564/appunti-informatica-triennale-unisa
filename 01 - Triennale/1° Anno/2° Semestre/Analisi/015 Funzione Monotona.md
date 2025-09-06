---
aliases:
  - Funzione Strettamente Monotona
  - Funzione Crescente
  - Funzione Strettamente Crescente
  - Funzione Decrescente
  - Funzione Strettamente Decrescente
tags:
  - corsi/matematica/analisi
paragrafo: Funzioni
cssclasses:
  - 
---
>Una **Funzione Monotona** è una [[011 Funzioni|funzione]] che *presenta sempre lo stesso andamento*. Può seguire *solo uno* dei due andamenti:
>- Crescita
>- Decrescita

Se invece cresce su una porzione del dominio e decresce altrove, si dice che la funzione **non è monotona**.

### Funzione Strettamente Monotona
Una Funzione Strettamente Monotona è [[#Funzione Strettamente Crescente|strettamente crescente]] oppure [[#Funzione Strettamente Decrescente|strettamente decrescente]].

## Funzione Crescente
>Una **Funzione Crescente** su un intervallo è una funzione che assume *valori crescenti* al crescere dei valori di ascissa. Ovvero se
>$$\forall x_1, x_2\in D,\quad \color{#61AFEF}x_1< x_2\Rightarrow f(x_1)\leq f(x_2)$$

![[Pasted image 20220311113330.png|500]]


### Funzione Strettamente Crescente
Si dice che una funzione è **Strettamente Crescente** se 
$$x_1< x_2\Rightarrow f(x_1)< f(x_2)$$


## Funzione Decrescente

>Una **Funzione Decrescente** è una funzione che assume *valori decrescenti* al crescere dei valori di ascissa nell'intervallo. Ovvero se
>$$\forall x_1, x_2\in D,\quad \color{#61AFEF}x_1< x_2\Rightarrow f(x_1)\geq f(x_2)$$



![[Pasted image 20220311113532.png|500]] 
### Funzione Strettamente Decrescente
Si dice che una funzione è **Strettamente Decrescente** se 
$$x_1< x_2\Rightarrow f(x_1)> f(x_2)$$


# %%FinePagina%%
___
[[000 Indice Analisi|↖ Ritorna all'indice ↖]]

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
- [YouMath](https://www.youmath.it/lezioni/analisi-matematica/le-funzioni-da-r-a-r-in-generale/28-monotonia-delle-funzioni-ovvero-crescita-e-decrescita-ii-parte.html)