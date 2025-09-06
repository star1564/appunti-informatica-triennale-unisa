---
aliases: 
tags:
  - corsi/matematica/analisi
paragrafo: Nozioni di base
cssclasses:
  - 
---
## Massimo
>Si dice che $y\in\mathbb{R}$ è il **Massimo** di un insieme $X\subseteq \mathbb{R}$ se: 
>1. $y$ è l'[[007 Estremo Inferiore e Superiore di un insieme#Estremo Superiore|estremo superiore]] di $X$
>2. $y\in X$.
>
>$$\max(X)=y\iff y=\sup(X)\land y\in X$$

![[Pasted image 20240927112555.png|800]]

Ovvero, se l'estremo superiore di un insieme *appartiene all'insieme stesso*, allora quello è anche il massimo dell'insieme.

![[Pasted image 20240927112642.png|800]]


> [!tip] Essenzialmente, è il massimo valore che può assumere $x$ per far parte dell'insieme $X$

> [!example]+ <font color="orange">Esempio</font>
>$$(1, 10]$$
>Si ha che:
>- $10$ è l'estremo superiore
>- $10$ è il massimo

## Minimo
>Diremo che $y\in\mathbb{R}$ è il **Minimo** di un insieme $X\subseteq \mathbb{R}$ se: 
>1. $y$ è l'[[007 Estremo Inferiore e Superiore di un insieme#ESTREMO INFERIORE|estremo inferiore]] di $X$
>2. $y\in X$
>
>$$\min(X)=y\iff y=\inf(X)\land y\in X$$

![[Pasted image 20240927112941.png|800]]

Ovvero se l'estremo inferiore di un insieme *appartiene all'insieme stesso*, allora quello è anche il minimo dell'insieme.

![[Pasted image 20240927113101.png|800]]

>[!tip] Essenzialmente, è il minimo valore che può assumere $x$ per far parte dell'insieme $X$

> [!example]+ <font color="orange">Esempio</font>
>$$(1, 10]$$
>Si ha che:
>- $1$ è l'estremo inferiore
>- $1$ NON è il minimo

## Unicità del Massimo e del Minimo

> [!quote] <font color="orange">Teorema - Unicità del Massimo e del Minimo</font>
>Se il massimo o il minimo di un insieme numerico esistono, questi sono unici.

> [!success] Dimostrazione per Assurdo
>Supponiamo per assurdo che ci siano due massimi (o minimi) per un sottoinsieme $X\subseteq \mathbb{R}$, chiamati $$y_1, y_2$$
>
>Allora:
>$$\begin{cases} x\leq y_1, \forall x\in X \\ x\leq y_2, \forall x\in X \end{cases} \Longrightarrow \begin{cases} y_2\leq y_1 \\ y_1\leq y_2 \end{cases}\Longrightarrow\color{#61AFEF} y_1 = y_2$$
>
>Quindi i due massimi (o minimi) sono uguali, pertanto può esistere solo un massimo (o minimo).

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
- [[033 Massimo (MD)]]
- [[034 Minimo (MD)]]