---
aliases: 
tags:
  - corsi/informatica/elementi_teoria_computazione
paragrafo: Linguaggi
cssclasses:
  - 
---
>Un **Linguaggio** su un [[030 Ricorsione di Stringhe#Alfabeto|alfabeto]] $\Sigma$ è un *insieme di [[030 Ricorsione di Stringhe#Stringa|stringhe]]*. Si denota spesso con $\color{#61AFEF}L$.

![[Pasted image 20240310111041.png|500]]

L'*insieme vuoto $\emptyset$* è il linguaggio che non contiene alcuna stringa:
- $\emptyset=\{\}$
- $\epsilon\not\in\emptyset$
- $\emptyset\neq\{\epsilon\}$

> [!example]- <font color="orange">Esempio</font>
>Siano $\Sigma=\{a,b\}, \Sigma^*=\{\epsilon,a,b,aa,bb,ab,\dots\}$.
>- $L_1=\{aa,aaa\}$
>- $L_2=\{\epsilon, aba,aab\}$
>- $L_3=\{\ \}=\color{#CC241D}\emptyset \neq \{\epsilon\}$
>- $L_4=\{w\in\Sigma^*\ :\ |w|_a=|w|_b \}=\{\epsilon,ab,ba,aabb,bbaa,\dots\}$
>- $L_5=\{a^nb^n\ |\ n\in\mathbb{N}, n\geq 0\}=\{\epsilon, ab,aabb,aaabbb,\dots\}$
>
>Dove: 
>- $\epsilon$ è la [[030 Ricorsione di Stringhe#Stringa vuota|stringa vuota]]
>- $|w|_a$ rappresenta il [[034 Numero di Occorrenze di un Carattere in una Stringa|numero di occorrenze nella stringa]] $w$ del carattere $a$
>- $L_4\neq L_5$

## Linguaggi come Insiemi
>Dato che i linguaggi sono degli [[001 Insieme|Insiemi]], un linguaggio è un [[002 Sottoinsiemi|sottoinsieme]] di $\Sigma^*$: $$\color{#CC241D}L\subseteq \Sigma^*$$

> [!example]- <font color="orange">Esempio</font>
>Se $\Sigma=\{a,b\},\Sigma^*=\{\epsilon,a,b,aa,bb,ab,\dots\}$
>
>Allora $L=\{aa,ab\}\subseteq\Sigma^*$

### Linguaggi Finiti ed Infiniti
Come gli insiemi, i linguaggi possono essere [[001 Insieme#Insieme Finito|finiti]] o [[001 Insieme#Insieme Infinito|infiniti]].

### Cardinalità di un Linguaggio
La **[[001 Insieme#Cardinalità (Ordine) di un Insieme Finito|Cardinalità]]** di un linguaggio finito è *il numero delle sue stringhe*.

> [!example]- <font color="orange">Esempio</font>
> $$L=\{aba,aab\},\quad |L|=|\{aba,aab\}|=2$$
> 

> [!note]+ Considerare la stringa vuota
> Se $L=\{\epsilon\}$, allora $|L|=1$


___
[[000 Indice ETC|↖ Ritorna all'indice ↖]]

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