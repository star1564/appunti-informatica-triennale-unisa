---
aliases: 
tags:
  - corsi/matematica/matematica_discreta
paragrafo: Relazioni - Ordine
cssclasses:
  - 
---
>Una **Relazione d'Ordine Usuale** ($\color{#CC241D}\leq$) si riferisce a un tipo specifico di [[029 Relazioni d_Ordine|relazione d'ordine]] che è comunemente utilizzata per definire l'*ordinamento naturale* o canonico tra gli elementi di un insieme. 

Questo tipo di relazione d'ordine è spesso intuitivo e corrisponde all'idea dove un numero è minore o uguale ad un altro.

Su insieme dei numeri interi $\mathbb{Z}$, la relazione d'ordine usuale è definita dall'operatore "minore o uguale di" ($\leq$) oppure "minore di" ($<$).

> [!note] Relazione invece di $\leq$
> Per semplificare si potrebbe usare $R$ invece di $\leq$.

> [!example]- <font color="orange">Esempio</font>
>Si considera la seguente Relazione:
>- $a, b\in \mathbb{Z}, aRb :\iff \exists h\in \mathbb{N_0}: b=h+a$
>
>Dove: **$aRb\iff a\leq b$**.
>
><font color="green">Dimostrazione</font>
>
>**RIFLESSIVA**
>$\forall a\in \mathbb{Z}, \color{#61AFEF}aRa$ 
>perché $a=0+a$ con $0\in \mathbb{N_0}$
>
>**ASIMMETRICA**
>$\forall a, b\in \mathbb{Z}:
>\begin{cases}
>aRb\\
>bRa\\
>\end{cases}\Rightarrow
>\begin{cases}
>\exists h\in \mathbb{N_0}: b= a+h \\
>\exists k\in \mathbb{N_0}: a= b+k \\
>\end{cases}$
>$\Rightarrow a = a+h+k$ 
>$\Rightarrow h+k = 0$
>$\Rightarrow \color{#61AFEF}h=k = 0$
>
>**TRANSITIVA**
>$\forall a, b, c\in \mathbb{Z}:
>\begin{cases}
>aRb\\
>bRc\\
>\end{cases}\Rightarrow
>\begin{cases}
>\exists h\in \mathbb{N_0}: b= a+h \\
>\exists k\in \mathbb{N_0}: c= b+k \\
>\end{cases}$
>$\Rightarrow \exists h, k\in \mathbb{N_0}: c = (a+h)+k$ 
>$\Rightarrow \color{#61AFEF}aRc$
#### MINORE ($<$)
Il simbolo del minore rappresenta che:
$$a<b\iff a\leq b \land a\neq b \iff (aRb \land a\neq b)$$

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
- Pagina Libro: 199