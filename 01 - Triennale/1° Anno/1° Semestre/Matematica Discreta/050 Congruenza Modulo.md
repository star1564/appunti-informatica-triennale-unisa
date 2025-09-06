---
aliases:
  - Modulo
  - Congruenza Modulo
tags:
  - corsi/matematica/matematica_discreta
paragrafo: Aritmetica
cssclasses:
  - 
---
>Si parlerà ora di una classe di notevoli relazioni di equivalenza nell'insieme degli interi, dette **congruenze modulo un intero $m$**.

Sia $m$ un intero, e si consideri in $\mathbb{Z}$ la relazione $m\mathbb{Z}$ definita ponendo, con $a,b\in \mathbb{Z}$:
$$a(m\mathbb{Z})b\iff m|b-a$$
$$a(m\mathbb{Z})b\iff \exists k \in \mathbb{Z}: b-a = mk$$

La relazione $m\mathbb{Z}$ è di equivalenza.

> [!tip]- Dimostrazione *IMPORTANTE*
>**RIFLESSIVA**
>$\forall a \in\mathbb{Z}$
>$a-a=0, \quad m|0\Rightarrow \color{orange}a(m\mathbb{Z})a$
>**SIMMETRICA**
>$\forall a,b \in\mathbb{Z}$
>$a(m\mathbb{Z})b\iff m|b-a \iff \exists k\in\mathbb{Z}: b-a=mk$ 
>$\Rightarrow \exists (-k)\in\mathbb{Z}: -(b-a)=m(-k)$ 
>$\Rightarrow -(mk)$[^1]$=m(-k)$
>$\Rightarrow m(-k)=m(-k) \Rightarrow m|a-b \Rightarrow \color{orange} b(m\mathbb{Z})a$
>**TRANSITIVA**
>$\forall a,b,c \in\mathbb{Z}$
>$\begin{cases}
>a(m\mathbb{Z})b\\
>b(m\mathbb{Z})c\\
>\end{cases}
>\iff \begin{cases}
>m|b-a\\
>m|c-b\\
>\end{cases}
>\iff \exists h,k\in\mathbb{Z}: \begin{cases}
>b-a = mh\\
>c-b=mk\\
>\end{cases}$
>
>Consideriamo: 
>$c - b + b - a = mk + mh$
>$\Rightarrow c-a = m(k+h)$
>$\Rightarrow \exists t=k+h \in \mathbb{Z}: c-a = mt$
>$\Rightarrow m|c-a \iff \color{orange} a(m\mathbb{Z})c$

[^1]: Dove $b-a$ viene sostituito con la sua definizione $mk$.

> [!quote] Definizione
>Quanto provato, ovvero 
>$$\color{#CC241D}a(m\mathbb{Z})b\iff m|b-a$$
>$$\color{#CC241D}a(m\mathbb{Z})b\iff \exists k \in \mathbb{Z}: b-a = mk$$
>giustifica il termine **congruenza modulo $m$** usato per denotare la relazione $m\mathbb{Z}$. Se $a(m\mathbb{Z})b$ si scrive anche:
>$$a\equiv b(mod\space m)$$
>e si legge "$a$ è congruo $b$ modulo $m$".

> [!example]- <font color="orange">Esempio</font>
>- $11\equiv 2 (mod\space 3)$, perchè:
>$2-11=-9,\quad -9\in 3\mathbb{Z}$
>
>- $-10\equiv -2 (mod\space 4)$, perché:
>$-2-(-10)=-2+10=8,\quad 8\in 4\mathbb{Z}$

> [!note] Casi
>- Se $m=0,\quad a\equiv b(mod\space 0)\iff a=b$;
>- $a\equiv b(mod\space m) \iff a\equiv b(mod\space -m)$ con $m>0$.

___
[[000 Indice MD|↖ Ritorna all'indice ↖]]

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