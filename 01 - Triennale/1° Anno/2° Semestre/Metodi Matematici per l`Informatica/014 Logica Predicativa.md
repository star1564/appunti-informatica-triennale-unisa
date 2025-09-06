---
aliases:
tags:
  - corsi/matematica/metodi_matematici_informatica
paragrafo: Logica Predicativa
cssclasses:
  - 
---
>Un **[[017 Predicato|predicato]]** è una asserzione che *utilizza variabili*, dove il valore assegnato a queste variabili determinano la [[001 Logica Proposizionale#Variabile Proposizionale|Variabile Proposizionale]] dell'asserzione. Fanno parte della **Logica Predicativa**.

> [!example]+ <font color="orange">Esempio</font>
>L'asserzione "$x+1=2$" non è una proposizione perché non è né vera né falsa.
>Ma può diventare vera o falsa, a seconda del valore assegnato alla variabile $x$. Questo è un *predicato*.

Gli elementi della logica predicativa sono:
- [[015 Costante (MMI)|Costanti]]
- [[016 Variabile (MMI)|Variabili]]
- [[017 Predicato|Predicati]]
- [[018 Quantificatore Universale|Quantificatori Universali]]
- [[019 Quantificatore Esistenziale|Quantificatori Esistenziali]]

## Asserzioni Composte
Le [[017 Predicato#^2cbb59|funzioni proposizionali]] possono essere collegate con i [[001 Logica Proposizionale#Proposizioni Composte e Connettivi Logici|connettivi logici]] per ottenere delle **Asserzioni Composte**.

> [!example]+ <font color="orange">Esempio</font>
>- $Studente(x)\land Esame(y)$.
>- $MMI(x)\Rightarrow Matricola(y)$.

In generale, i quantificatori $\forall$ ([[018 Quantificatore Universale|Quantificatore Universale]]) ed $\exists$ ([[019 Quantificatore Esistenziale|Quantificatore Esistenziale]]) hanno *[[011 Precedenza degli Operatori Logici|precedenza]] più alta* di qualsiasi connettivo logico del calcolo proposizionale.

> [!example]+ <font color="orange">Esempio</font>
>$\forall x\ P(x)\ \lor\ Q(x) \equiv (\forall x\ P(x))\ \lor\ Q(x)$.




___
[[000 Indice MMI|↖ Ritorna all'indice ↖]]

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

