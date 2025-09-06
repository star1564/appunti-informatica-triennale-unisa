---
aliases:
  - Evento
  - Evento Certo
  - Evento Impossibile
  - Evento Necessario
tags:
  - corsi/matematica/probabilità_statistica
inglese:
  - Event
paragrafo: Eventi
cssclasses:
  - 
---
> Un **Evento** è un [[002 Sottoinsiemi|Sottoinsieme]] dello [[010 Spazio Campionario|Spazio Campionario]].

> [!example] <font color="orange">Esempio</font>
> ![[Pasted image 20230926153904.png|500]]

Consideriamo uno spazio campionario $S$ associato ad un esperimento casuale, sia $A$ un evento e sia $x$ il risultato finale dell'esperimento. Diremo che:
- L'**evento si verifica** (o è accaduto) se $x\in A$
- L'**evento non si verifica** (o non è accaduto) se $x\notin A$

> [!example]+ <font color="orange">Esempio</font>
>![[Pasted image 20230930155044.png|500]]
>L'evento $A$ si è verificato.

Più eventi (in numero finito o infinito) si dicono **necessari** se la loro unione è $S$. ^3a631f

### Eventi Certi ed Impossibili
Si hanno diversi tipi di eventi:
- L'*evento certo* è un qualsiasi evento che si verifica con certezza nell'esperimento. Un esempio è l'evento $E=S$ ^660fb4
- L'*evento impossibile* è un qualsiasi evento che non può in alcun modo verificarsi nell'esperimento. Un esempio è l'evento $E=\emptyset$ ^da36b3

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
- [Michel van Biezen](https://www.youtube.com/watch?v=WelPq6Kgf7s&list=PLX2gX-ftPVXUWwTzAkOhBdhplvz0fByqV&index=4)
- [YouMath](https://www.youmath.it/lezioni/probabilita/probabilita-discreta/5170-evento-certo-evento-aleatorio-evento-impossibile.html)