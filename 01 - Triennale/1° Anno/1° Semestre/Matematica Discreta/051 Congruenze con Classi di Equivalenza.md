---
aliases: 
tags:
  - corsi/matematica/matematica_discreta
paragrafo: Aritmetica
cssclasses:
  - 
---
La [[027 Classi di Equivalenza|classe di equivalenza]] [[050 Congruenza Modulo|Modulo]] $m\mathbb{Z}$ di un qualunque $x\in\mathbb{Z}$ viene di solito denotata con **$[x]_m$**, oppure $\overline{x}$.

L'[[027 Classi di Equivalenza#INSIEME QUOZIENTE|insieme quoziente]] $\mathbb{Z}/m\mathbb{Z}$ è denotato anche con il simbolo **$\mathbb{Z}_m$** ed è detto l'**insieme degli interi modulo $m$**.

$\forall x,m\in\mathbb{Z}$
$$[x]_m = \{x+mk:k\in\mathbb{Z}\}$$

Possiamo inoltre considerare che con *$x\equiv rest(x,m)(mod\space m)$*, abbiamo *i possibili resti della divisione euclidea per m*:
$$[x]_m = [rest(x,m)]_m$$

Quindi con $m>0$ si ha che:
$$\mathbb{Z}_m = \{[0]_m,[1]_m,...,[m-1]_m\}\space e \space |\mathbb{Z}_m| = m$$

con elementi a due a due distinti.

Si ha, con $0\leq a,b\leq m-1$, che:
$[a]_m=[b]_m\iff a\equiv b(mod\space m)\iff a=b$

## COMPATIBILITÀ CON LE OPERAZIONI
La [[Compatibilità delle Operazioni]] della relazione $m\mathbb{Z}$ con *+* e *$\cdot$* ci permette di definire delle operazioni nell'insieme $\mathbb{Z}_m$. Quindi abbiamo:
$\forall a, b\in \mathbb{Z}$
$$[a]_m + [b]_m = [a+b]_m$$
$$[a]_m \cdot [b]_m = [a\cdot b]_m$$

> [!example]- <font color="orange">Esempio</font>
>$m=4, \mathbb{Z}_4=\{\overline{0},\overline{1},\overline{2},\overline{3}\}$. Posso considerare (dove $\overline{4}=\overline{0}$ e per esempio $\overline{6}=\overline{4}+\overline{2}=\overline{2}$) le seguenti tabelle:
>
>| $+$              | $\overline{0}$            | $\overline{1}$            | $\overline{2}$            | $\overline{3}$            |
>| -------------- | ------------------------- | ------------------------- | ------------------------- | ------------------------- |
>| $\overline{0}$ | $\color{red}\overline{0}$ | $\color{red}\overline{1}$ | $\color{red}\overline{2}$ | $\color{red}\overline{3}$ |
>| $\overline{1}$ | $\color{red}\overline{1}$ | $\color{red}\overline{2}$ | $\color{red}\overline{3}$ | $\color{red}\overline{0}$ |
>| $\overline{2}$ | $\color{red}\overline{2}$ | $\color{red}\overline{3}$ | $\color{red}\overline{0}$ | $\color{red}\overline{1}$ |
>| $\overline{3}$ | $\color{red}\overline{3}$ | $\color{red}\overline{0}$ | $\color{red}\overline{1}$ | $\color{red}\overline{2}$ |
>
> $\space$
>
>| $\cdot$        | $\overline{0}$            | $\overline{1}$            | $\overline{2}$            | $\overline{3}$            |
>| -------------- | ------------------------- | ------------------------- | ------------------------- | ------------------------- |
>| $\overline{0}$ | $\color{red}\overline{0}$ | $\color{red}\overline{0}$ | $\color{red}\overline{0}$ | $\color{red}\overline{0}$ |
>| $\overline{1}$ | $\color{red}\overline{0}$ | $\color{red}\overline{1}$ | $\color{red}\overline{2}$ | $\color{red}\overline{3}$ |
>| $\overline{2}$ | $\color{red}\overline{0}$ | $\color{red}\overline{2}$ | $\color{red}\overline{0}$ | $\color{red}\overline{2}$ |
>| $\overline{3}$ | $\color{red}\overline{0}$ | $\color{red}\overline{3}$ | $\color{red}\overline{2}$ | $\color{red}\overline{1}$ |

## PROPRIETÀ DELLE OPERAZIONI
### PROPRIETÀ OPERAZIONI $+$
- $+$ è **Commutativa** in $\mathbb{Z}_m$: $\forall [a], [b]\in \mathbb{Z}_m\quad [a]+[b]=[b]+[a] \iff [a+b]=[b+a]$

- $+$ è **Associativa** in $\mathbb{Z}_m$: $\forall [a], [b], [c]\in \mathbb{Z}_m\quad ([a]+[b])+c = [a+b]+[c]=[(a+b)+c]$ $= [a+(b+c)]=[a]+[b+c]=[a]+([b]+[c])$

- $[0]$ è l'**Elemento Neutro** rispetto a $+$ in $\mathbb{Z}_m$:
$\forall [a]\in \mathbb{Z}_m\quad [a]+[0]=[a]=[0]+[a]$

- Ogni elemento ha un **Opposto**:
$[a]\in \mathbb{Z}_m, \exists [b]\in \mathbb{Z}_m: [a]+[b]=[0] \Longrightarrow -[a]_m = [m-a]_m$

### PROPRIETÀ OPERAZIONI $\cdot$
- $\cdot$ è **Commutativa** in $\mathbb{Z}_m$: $\forall [a], [b]\in \mathbb{Z}_m\quad [a][b]=[b][a] \iff [ab]=[ba]$

- $\cdot$ è **Associativa** in $\mathbb{Z}_m$: $\forall [a], [b], [c]\in \mathbb{Z}_m\quad [a]\cdot ([b]\cdot[c]) = ([a]\cdot[b])\cdot[c]$

- $[1]$ è l'**Elemento Neutro** rispetto a $\cdot$ in $\mathbb{Z}_m$:
$\forall [a]\in \mathbb{Z}_m\quad [a]\cdot[1]=[a]=[1]\cdot[a]$

- Si parlerà che tutti gli elementi di $\mathbb{Z}_m-\{[0]\}$ sono **Invertibili** se e solo se *$m$ è [[042 Numeri Primi|primo]]*.
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