---
aliases: 
tags:
  - corsi/matematica/analisi
paragrafo: Nozioni di base
cssclasses:
  - 
---
## Estremo Superiore

>Se $X\subseteq \mathbb{R}$ è un insieme [[006 Insieme Illimitato e Limitato#Insieme Limitato Superiormente|limitato superiormente]], si dice che $y\in\mathbb{R}$ è l'**Estremo Superiore** dell'insieme $X$ se e solo se:
>1. $y$ è un *[[005 Maggiorante e Minorante di un Insieme#Maggiorante|maggiorante]]* di $X$
>2. $y$ è il *più piccolo maggiorante* di $X$
>$$\sup(X)=y\iff\begin{cases} x\leq y, & \forall x\in X \\ \exists x\in X:x>z, & \forall z<y \end{cases}$$

![[Pasted image 20240927104217.png|800]]

Ovvero, è l'elemento più grande di $X$.

Se $X$ è un insieme [[006 Insieme Illimitato e Limitato#Insieme Illimitato Superiormente|illimitato superiormente]] si dice che l'estremo superiore di $X$ è *infinito* ($+\infty$).

> [!example]+ <font color="orange">Esempio</font>
>$$\sup(\ (1,10]\ )=10$$

## Estremo Inferiore
>Se $X\subseteq \mathbb{R}$ è un insieme [[006 Insieme Illimitato e Limitato#Insieme Limitato Inferiormente|limitato inferiormente]], si dice che $y\in\mathbb{R}$ l'**Estremo Inferiore** dell'insieme $X$ se e solo se:
>1. $y$ è un *[[005 Maggiorante e Minorante di un Insieme#Minorante|minorante]]* di $X$;
>2. $y$ è il *più grande minorante* di $X$.
>$$\inf(X)=y\iff\begin{cases} y\leq x, & \forall x\in X \\ \exists x\in X:x<z, & \forall z>y \end{cases}$$

![[Pasted image 20240927105628.png|800]]

Ovvero, è l'elemento più grande di $X$.

Se $X$ è un insieme illimitato inferiormente (non ammette minoranti) diremo che l'estremo inferiore di $X$ è meno infinito ($-\infty$).

> [!example]+ <font color="orange">Esempio</font>
>$$\inf(\ (1,10]\ )=1$$

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