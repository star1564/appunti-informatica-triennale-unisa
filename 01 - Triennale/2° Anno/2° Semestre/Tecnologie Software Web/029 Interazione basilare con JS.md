---
aliases: 
tags:
  - corsi/informatica/tecnologie_software_web
paragrafo: JavaScript
cssclasses:
  - 
---
Ci sono diversi modi per *interagire con l'utente* in una pagina HTML utilizzando [[028 Basi per JavaScript]].

Questi sono solo alcuni modi.

---
### Alert
L'*alert* è una semplice finestra di dialogo pop-up che mostra un messaggio all'utente. Puoi utilizzare la funzione *`alert()`* per visualizzare un messaggio e un pulsante OK. Ad esempio:

```javascript
alert("Ciao! Benvenuto sulla mia pagina web.");
```

---
### Console
Se stai lavorando nella *console del browser*, puoi utilizzare il metodo *`console.log()`* per stampare messaggi di debug o informazioni per l'utente. Ad esempio:

```javascript
console.log("Questo è un messaggio di log.");
```

---
### Prompt
Il *prompt* è una finestra di dialogo che consente all'utente di inserire un valore. Puoi utilizzare la funzione *`prompt()`* per visualizzare un messaggio e chiedere all'utente di inserire una risposta. Il valore inserito dall'utente verrà restituito come stringa.

```javascript
var nome = prompt("Inserisci il tuo nome:");
console.log("Ciao, " + nome + "!");
```

---
### Confirm
Il confirm è una finestra di dialogo che richiede all'utente di confermare o annullare un'azione. Puoi utilizzare la funzione `confirm()` per visualizzare un messaggio e ottenere la risposta dell'utente come un valore booleano (`true` se l'utente ha confermato, `false` se ha annullato). Ad esempio:

```javascript
var conferma = confirm("Sei sicuro di voler eliminare questo elemento?");
if (conferma) {
	alert("confermato");
} else {
	alert("non confermato");
}
```


___
[[000 Indice TSW|↖ Ritorna all'indice ↖]]

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