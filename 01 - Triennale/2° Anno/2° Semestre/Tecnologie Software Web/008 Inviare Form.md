---
aliases: 
tags:
  - corsi/informatica/tecnologie_software_web
paragrafo: HTML
cssclasses:
  - 
---
>Per inviare dati al server si utilizzano i **Form** (il tag è **`<form>`**). Invia i dati (racchiusi in un pacchetto) al [[Server]], che eventualmente invia una risposta.

I form hanno vari [[007 Elementi e Attributi HTML#ELEMENTO|elementi]] di input ([[029 Componenti di GUI Java|campi di testo, checkbox, calendario...]]). 

```HTML
<form action="sito target" method="POST o GET">  
...  
elementi form  
...  
</form>
```

L'elemento `<form>` è un contenitore per i diversi tipi di *elementi di input* ed è formato da:
- `action`: specifica a quale indirizzo mandare i dati inseriti;
- `method` (default se non specificato `GET`): specifica con quale [[004 Richiesta e Risposta HTTP#RICHIESTA|mezzo di richiesta]] bisogna inviare i dati.

Al suo interno, si possono specificare vari campi di input con i propri [[007 Elementi e Attributi HTML|attributi]] `<input type="tipo" ...attributi... >`:
- *Testo* (`text`): mostra una area di testo ad una riga per l'input. 
- *Pulsanti Radio* (`radio`): mostra un pulsante radio (dove si seleziona solo una opzione su molte).
- *Checkbox* (`checkbox`): mostra una checkbox (dove si seleziona da zero a molte opzioni).
- *Pulsante Invio Richiesta* (`submit`): mostra un pulsante di invio di richiesta al server.
- *Pulsante* (`button`): mostra un pulsante cliccabile.
- [Altri tipi di input](https://www.w3schools.com/html/html_form_input_types.asp).

> [!info] Attributo `name` di `<input>`
> Da notare che ogni campo di input (ad eccezione per `submit`) deve avere un attributo chiamato **`name`**. Se questo viene omesso, il valore del campo di input non verrà mandato al server.

> [!example]- <font color="orange">Esempio</font>
>```HTML
><!DOCTYPE html>
><html>
>	<head>
>		<title>Enter the Contest</title>
>	</head>
>	<body>
>		<form action="http://wickedlysmart.com/hfhtmlcss/contest.php" method="POST">
>			<p>Just type in your name (and click Submit) to enter the contest:</br>
>			First name: <input type="text" name="firstname" value=""></br>
>			Last name: <input type="text" name="lastname" value=""></br>
>			<input type="submit">
>			</p>
>		</form>
>	</body>
></html>
>```
> *Sul Sito*:
><form action="http://wickedlysmart.com/hfhtmlcss/contest.php" method="POST">
><p>Just type in your name (and click Submit) to enter the contest:</br>
>First name: <input type="text" name="firstname" value=""></br>
>Last name: <input type="text" name="lastname" value=""></br>
><input type="submit">

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
- [w3school - Form](https://www.w3schools.com/html/html_forms.asp)