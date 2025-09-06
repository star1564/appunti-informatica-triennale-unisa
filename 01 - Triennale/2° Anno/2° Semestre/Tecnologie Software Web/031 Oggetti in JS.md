---
aliases: 
tags:
  - corsi/informatica/tecnologie_software_web
paragrafo: JavaScript
cssclasses:
  - 
---
### Oggetti
>In JavaScript, gli **oggetti** sono strutture dati che consentono di *raggruppare valori e funzioni correlate in un'unica entità*. Gli oggetti sono composti da coppie *chiave-valore*, dove le chiavi sono stringhe e i valori possono essere di qualsiasi tipo di dato JavaScript, inclusi altri oggetti.

Gli oggetti in JavaScript possono essere creati in diversi modi. Uno dei modi più comuni è utilizzare la sintassi letterale dell'oggetto, racchiusa tra parentesi graffe `{}`. 

> [!example] <font color="orange">Esempio</font>
>```javascript
>var persona = {
>	nome: "Mario",
>	cognome: "Rossi",
>	eta: 30
>};
>```

In questo esempio, abbiamo creato un oggetto chiamato `persona` con tre proprietà: `nome`, `cognome` ed `eta`. I valori delle proprietà sono rispettivamente una stringa, una stringa e un numero.

È possibile accedere alle proprietà di un oggetto utilizzando la *notazione punto* (`oggetto.proprieta`) o la *notazione delle parentesi quadre* (`oggetto["proprieta"]`).

> [!example] <font color="orange">Esempio</font>
>```javascript
>console.log(persona.nome);  // Output: "Mario"
>console.log(persona["cognome"]);  // Output: "Rossi"
>```

È possibile *aggiungere nuove proprietà* a un oggetto o *modificare quelle esistenti* assegnando un valore alla chiave corrispondente.

> [!example] <font color="orange">Esempio</font>
>```javascript
>persona.eta = 35; // Modifica
>persona.professione = "Ingegnere"; // Aggiunta
>```

Inoltre, gli oggetti possono contenere anche *funzioni*, che vengono chiamate [[Metodi]] dell'oggetto.
> [!example] <font color="orange">Esempio</font>
>```javascript
>var persona = {
>	nome: "Mario",
>	cognome: "Rossi",
>	eta: 30,
>	saluta: function() {
>		console.log("Ciao, sono " + this.nome + " " + this.cognome);
>	}
>};
>
>persona.saluta();  // Output: "Ciao, sono Mario Rossi"
>```

In questo esempio, `saluta` è un metodo dell'oggetto `persona` che stampa un messaggio di saluto utilizzando le proprietà `nome` e `cognome` dell'oggetto stesso.

È possibile anche *eliminare* una proprietà da un oggetto utilizzando `delete`. Ad esempio:

> [!example] <font color="orange">Esempio</font>
>```js
>var persona = {
>	nome: "Mario",
>	cognome: "Rossi",
>	età: 30
>};
>
>delete persona.eta;
>console.log(persona); // Output: [Mario, Rossi]
>```

Per fare *debugging* su uno o più oggetti, è utile eseguire questo codice:
```js
console.log({persona1, persona2, persona3, ecc});  // Mostra i nomi ed i valori corrispondenti degli oggetti (anche se non sono oggetti uguali)
console.table([persona1, persona2, persona3, ecc]); // Permette di mostrare gli oggetti in una tabella (utile se sono tutti oggetti uguali)
```
Questo perché possiamo vedere sia i nomi degli oggetti, sia i loro valori.

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
- [Fireship](https://www.youtube.com/watch?v=Mus_vwhTCq0)