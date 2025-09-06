---
aliases: 
tags:
  - corsi/matematica/probabilità_statistica
inglese: 
paragrafo: Teoremi Limite
cssclasses:
  - 
---
>Se $X$ è una [[023 Variabili Aleatorie|Variabile Aleatoria]] che assume *solo valori non negativi*, allora per ogni numero reale $t>0$ si ha la **Disuguaglianza di Markov** se $$\mathbb{P}(X\geq t)\leq\frac{E[X]}{t}$$

In pratica si vuole sapere $\mathbb{P}(X\geq t)$, *ma si conosce solamente $E[X]$* e $t$. 
Con questi due dati, ci permette di ottenere un **limite superiore per questa probabilità**.

Ovvero che "la probabilità che $\mathbb{P}(X\geq t)$ non può essere troppo grande".
![[Pasted image 20250319143259.png]]

> [!quote] Dimostrazione
>Per $a>0$, si consideri la seguente [[028 Distribuzione di Bernoulli|variabile di Bernoulli]] $$I=\begin{cases} 1 & \text{se }X\geq a \\ 0& \text{altrimenti} \end{cases}$$
>
>![[Pasted image 20240822181425.png]]
>
>Dato che $X\geq 0$, si ha che $I\leq \frac{X}{a}$. Infatti: 
>- Se $X<a$, si ha che $I=0\leq \frac{X}{a}$
>- Se $X\geq a$, si ha che $I=1\leq \frac{X}{a}$
>
>Dalle proprietà del [[026 Valore Atteso di una VA Discreta|Valore Atteso]] segue che $$E[I]\leq \frac{E[X]}{a}$$
>
>Per la variabile di Bernoulli $I$, risulta che $E[I]=\mathbb{P}(I=1)=\mathbb{P}(X\geq a)$, quindi si ha che $$\mathbb{P}(X\geq a)\leq\frac{E[X]}{a}$$


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
- https://youtu.be/e-nAr3MkAII?t=11