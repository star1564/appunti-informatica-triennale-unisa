---
aliases:
  - Successione Monotona Costante
  - Successione Monotona Crescente
  - Successione Monotona Strettamente Crescente
  - Successione Monotona Decrescente
  - Successione Monotona Strettamente Decrescente
  - Successione Monotona Estratta
tags:
  - corsi/matematica/analisi
paragrafo: Successioni
cssclasses:
  - 
---

> [!tip] Simile alle [[015 Funzione Monotona|funzioni monotone]]

## Successioni Monotone Crescenti
>Una successione $\{a_n\}_{n\in\mathbb{N}}$ è detta **Monotona Crescente** se per ogni numero naturale $n$ si ha che:
>$$\color{#61AFEF}a_n\leq a_{n+1}$$

Ovvero che, al crescere di $n$, si ottengono valori sempre più grandi.

![[Pasted image 20240930111358.png|400]]

### Successioni Monotone Strettamente Crescenti
>Una successione $\{a_n\}_{n\in\mathbb{N}}$ è detta **Monotona Strettamente Crescente** se per ogni numero naturale $n$ risulta che:
>$$\color{#61AFEF}a_n<a_{n+1}$$

![[Pasted image 20240930111320.png|400]]

> [!example]+ <font color="orange">Esempio</font>
>La successione $\{n\}$ i cui primi termini sono $(0, 1, 2, 3, 4, ..., n, ...)$ è una successione monotona strettamente crescente.

## Successioni Monotone Decrescenti
>Una successione $\{a_n\}_{n\in\mathbb{N}}$ è detta **Monotona Decrescente** se per ogni numero naturale $n$ si ha che:
>$$\color{#61AFEF}a_n\geq a_{n+1}$$

Ovvero al crescere di $n$ otterremo valori sempre più piccoli.

![[Pasted image 20240930111749.png|400]]

### Successioni Monotone Strettamente Decrescenti
>Una successione $\{a_n\}_{n\in\mathbb{N}}$ è detta **Monotona Strettamente Decrescente** se per ogni numero naturale $n$ risulta che:
>$$\color{#61AFEF}a_n>a_{n+1}$$

![[Pasted image 20240930111803.png|400]]

> [!example]+ <font color="orange">Esempio</font>
>La successione $\{\frac{1}{n+1}\}$ i cui primi termini sono $(1, \frac{1}{2}, \frac{1}{3}, \frac{1}{4}, ..., \frac{1}{n+1}, ...)$ è una successione monotona strettamente decrescente.

## Successione Costante
>Si ha una **successione costante** se $$\color{#61AFEF}\{a_n\}_{n\in\mathbb{N}}=a\in\mathbb{R}$$


![[Pasted image 20240930111106.png|400]]

In questo caso la successione è sia [[#Successioni Monotone Crescenti|crescente]] che [[#Successioni Monotone Decrescenti|decrescente]].

## Successioni Estratte
>Si dice che $b_n$ è una successione **Estratta** da $a_n$ se esiste una successione [[#Successioni Monotone Strettamente Crescenti|strettamente crescente]] di numeri naturali $k_n$ tale che $b_n=a_{k_n}$.

Ovvero la successione estratta prende solo alcuni termini della successione originaria.

![[Pasted image 20240930112443.png|400]]

> [!example]+ <font color="orange">Esempio</font>
>- $b_n=\sqrt{2n}$ è estratta dalla successione $a_n=\sqrt{n}$ con $k_n=2n$;
>- $b_n=n$ è estratta dalla successione $a_n=\sqrt{n}$ con $k_n=n^2$.


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
- [Video - Elia Bombardelli](https://youtu.be/PtQSWL-5GCo?t=357)