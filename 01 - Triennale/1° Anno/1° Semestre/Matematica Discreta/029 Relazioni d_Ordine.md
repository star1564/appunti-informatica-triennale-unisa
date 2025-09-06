---
aliases: Relazione d'Ordine
tags:
  - corsi/matematica/matematica_discreta
paragrafo: Relazioni - Ordine
cssclasses:
  - 
---
>Una **Relazione d'Ordine** è una [[025 Relazioni Binarie|relazione binaria]] su un insieme che sfrutta la nozione di *ordinamento* o gerarchia tra gli elementi di quell'insieme e stabilisce un modo di classificare o confrontare gli elementi in base a criteri di "*minore di*" o "*maggiore di*".

> [!quote] Definizione Formale
>Sia $S\neq \emptyset$ un insieme e sia $R \subseteq S\times S$ una relazione binaria in $S$.
>Si dice che $R$ è una **Relazione d'Ordine** in $S$ se:
>1. $R$ è [[026 Relazioni di Equivalenza#Proprietà Riflessiva|Riflessiva]];
>2. $R$ è [[026 Relazioni di Equivalenza#Proprietà Transitiva|Transitiva]];
>3. $R$ è **Asimmetrica**.

### Proprietà Asimmetrica
>Data una relazione $R \subseteq A\times A$, se un elemento è in relazione con un secondo elemento, allora il secondo elemento *non può essere in relazione* con il primo elemento. 
>
>Quindi, se i due elementi sono in relazione, devono essere *uguali* per poter soddisfare la relazione.

> [!quote] Definizione Formale
> $a,b \in A, (a,b)\in R \implies (b,a)\not\in R \iff \color{#CC241D}a,b\in A, aRb \implies b\not R a$
> 
> Oppure, se esiste una relazione:
> 
> $a, b\in A, (a,b),(b,a)\in R \implies a=b$


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
- Pagina Libro: 56, 90