---
aliases: 
tags:
  - corsi/matematica/analisi
paragrafo: Derivate
cssclasses:
  - 
---
> [!quote] <font color="orange">Teorema di Rolle</font>
>Sia $f:[a,b]\to \mathbb{R}$ una funzione:
>1. [[035 Funzione Continua|continua]] in $[a,b]$
>2. [[046 Derivata di una Funzione#Funzione Derivabile|derivabile]] in $(a,b)$
>
>Se la funzione assume lo stesso valore agli estremi dell'intervallo, ossia 
>$$\color{#61AFEF}f(a)=f(b)$$
>allora *esiste almeno un punto* $c\in(a,b)$ tale che 
>$$\color{#CC241D}f'(c)=0$$

![[Pasted image 20241003161141.png|500]]

> [!success]+ Dimostrazione (pL 195, Aracne)
> Si possono avere due casi:
> 1. Se il massimo e il minimo assoluti coincidono e si trovano agli estremi dell'intervallo, ovvero $$M=m$$
>    allora la funzione è [[F01 Funzione Lineare e Costante#^759638|costante]]. Si avrà che $f'(c)=0, \forall c\in (a,b)$.
>    
>    ![[Pasted image 20241003162948.png|500]]
> 
> 
> 2. Si consideri adesso una funzione che *non* è costante su $[a,b]$. Allora, per il [[051 Teorema di Weierstrass per Derivate|teorema di Weierstrass]], esistono massimo e minimo [[049 Massimi e Minimi Relativi e Assoluti#Assoluti|assoluti]]. 
>    Quindi si hanno tre casi, dove in ognuno di esso, esiste un punto $c\in (a,b)$ di massimo o di minimo, per cui, attraverso il [[050 Teorema di Fermat|teorema di Fermat]], si ha che$f'(c)=0$:
> 	- $$f(a)=f(b)\neq \max_{[a,b]} f, \quad f(a)=f(b)= \min_{[a,b]} f$$![[Pasted image 20241003164113.png|500]]
> 	- $$f(a)=f(b)= \max_{[a,b]} f, \quad f(a)=f(b)\neq \min_{[a,b]} f$$![[Pasted image 20241003164129.png|500]]
> 	- $$f(a)=f(b)\neq \max_{[a,b]} f, \quad f(a)=f(b)\neq \min_{[a,b]} f$$ ![[Pasted image 20241003164148.png|500]]








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
- [Francesco Bigolin](https://youtu.be/_Op9kuKFsvc?si=h4G7__-QkhDxNVIR)