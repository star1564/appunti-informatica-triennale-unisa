---
aliases: 
tags:
  - corsi/matematica/probabilità_statistica
inglese: 
paragrafo: Distribuzioni V.A. Discrete
cssclasses:
  - 
---
>La [[024 Funzione di Distribuzione|distribuzione]] **Uniforme Discreta** attribuisce *la stessa probabilità ad ogni elemento* dello [[010 Spazio Campionario|Spazio Campionario]] $S$.

#### Densità Discreta
>La sua [[025 Variabili Aleatorie Discrete#Densità Discreta|Densità Discreta]] può essere espressa in questo modo:
>$$\color{#CC241D}p(k)=\frac{1}{N}$$
>Dove:
>- $k=1,2,\ldots, N$
>- $N=|S|$

> [!example]- <font color="orange">Esempio</font>
>Il lancio di un dado non truccato: ognuno dei valori 1, 2, 3, 4, 5 e 6 ha eguale probabilità $1/6$ di verificarsi.
>
>![[Pasted image 20240820172230.png|700]]


#### Funzione di Distribuzione
La sua [[024 Funzione di Distribuzione|Funzione di Distribuzione]] è la seguente:
$$F(x)=\begin{cases} 0, & x<1 \\ \sum_{r\leq x}p(r)=\sum_{r=1}^{k} \frac{1}{N}= \frac{k}{N}, & k\leq x< k+1,\ k=1,2,\ldots, N-1 \\ 1, & x\geq N \end{cases}$$
#### Valore Atteso
Il suo [[026 Valore Atteso di una VA Discreta|Valore Atteso]] è il seguente:
$$\color{#CC241D}E[X]= \frac{N+1}{2}$$


#### Varianza
La sua [[027 Varianza di una VA Discreta|Varianza]] è la seguente:
$$\color{#CC241D}\text{Var}(X)=\frac{N^2+1}{12}$$



___
[[000 Indice PS|↖ Ritorna all'indice ↖]]

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
- [Wikipedia](https://it.wikipedia.org/wiki/Distribuzione_discreta_uniforme)