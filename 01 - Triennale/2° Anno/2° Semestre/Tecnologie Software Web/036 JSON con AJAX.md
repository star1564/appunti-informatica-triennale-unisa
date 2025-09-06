---
aliases: 
tags:
  - corsi/informatica/tecnologie_software_web
paragrafo: JavaScript
cssclasses:
  - 
---
## JSON
>**JSON** (*JavaScript Object Notation*) è un formato di dati leggero e indipendente dal linguaggio utilizzato per *rappresentare dati strutturati in modo leggibile* sia per gli esseri umani che per le macchine. È ampiamente utilizzato per lo *scambio di dati tra un server e un client web*, ma può essere utilizzato anche in altri contesti.

La sintassi di JSON è basata sulla sintassi degli [[031 Oggetti in JS|oggetti in JavaScript]] e può essere definita informalmente come un array di oggetti.

Le caratteristiche principali di JSON sono le seguenti:

1. **Sintassi semplice**: JSON utilizza una sintassi molto chiara e semplice, basata su coppie *chiave-valore*. I dati sono rappresentati come coppie di "chiave" e "valore", separate da due punti e delimitate da virgole. È anche possibile inserire array (oppure oggetti contenenti array). Ad esempio:
```json
{
  "nome": "Mario",
  "cognome": "Rossi",
  "età": 30,
  "città": "Roma",
  "cibo": {
	"preferito": ["pizza", "pasta"],
	"odiato": ["ananas sulla pizza"]
  }
}
```

2. **Supporto per vari tipi di dati**: JSON supporta diversi tipi di dati, tra cui stringhe, numeri, booleani, array e oggetti. Questo permette di rappresentare dati complessi e strutturati in un formato standard.

3. **Facile da leggere e scrivere**: JSON è progettato per essere facilmente leggibile sia dagli esseri umani che dalle macchine. La sua sintassi semplice e ben strutturata lo rende adatto per la lettura e la scrittura manuale, ma può anche essere facilmente elaborato da programmi e script.

4. **[[Interoperabilità]]**: JSON è supportato nativamente da molti linguaggi di programmazione e framework. Ciò consente un'agevole integrazione tra diversi sistemi e componenti software.

## JSON con AJAX

> [!success] [Video consigliato](https://www.youtube.com/watch?v=rJesac0_Ftw)


> [!example]- <font color="orange">Esempio</font>
>```js
>// Crea un oggetto XMLHttpRequest
>var ourRequest = new XMLHttpRequest();
>
>// Ottiene il file json
>ourRequest.open('GET', 'https://learnwebcode.github.io/json-example/animals-1.json');
>
>// Funzione che viene eseguita quando il file viene caricato
>ourRequest.onload = function() {
>    var ourData = JSON.parse(ourRequest.responseText); // Converte il file json in un oggetto javascript
>    console.log(ourData[0]); // Stampa il primo elemento dell'oggetto
>}
>
>// Invia la richiesta
>ourRequest.send();
>```

> [!example]- <font color="orange">Esempio 2 (attraverso JQuery)</font>
>HTML:
>```HTML
><!DOCTYPE html>
><html lang="en">
><head>
>    <meta charset="UTF-8">
>    <meta name="viewport" content="width=device-width, initial-scale=1.0">
>    <title>JSON and AJAX</title>
></head>
><body>
>    <div class="header">
>        <h1>JSON and AJAX</h1>
>        <button id="btn">Fetch Info for 3 New Animals</button>
>    </div>
>    <div id="animal-info"></div>
>    <script src="jquery-3.7.0.min.js"></script> <!-- Includi JQuery -->
>    <script src="main.js"></script>
></body>
></html>
>```
>
>JSON (`animals-1`)
>```json
>[
>  {
>    "name": "Meowsy",
>    "species" : "cat",
>    "foods": {
>      "likes": ["tuna", "catnip"],
>      "dislikes": ["ham", "zucchini"]
>    }
>  },
>  {
>    "name": "Barky",
>    "species" : "dog",
>    "foods": {
>      "likes": ["bones", "carrots"],
>      "dislikes": ["tuna"]
>    }
>  },
>  {
>    "name": "Purrpaws",
>    "species" : "cat",
>    "foods": {
>      "likes": ["mice"],
>      "dislikes": ["cookies"]
>    }
>  }
>]
>```
>
>JS:
>```js
>/**
> * Variabile che contiene il numero di iterazione
> */
>var pageCounter = 1;
>
>/**
> * Variabile che contiene lo spazio dove inserire i dati
> */
>var animalContainer = $("#animal-info");
>
>/**
> * Variabile che contiene il pulsante
> */
>var btn = $("#btn");
>
>// Al click del pulsante
>btn.click(function(){
>    var ourRequest = new XMLHttpRequest();
>    // Prendi i dati dal file json della pagina corrente
>    ourRequest.open('GET', 'https://learnwebcode.github.io/json-example/animals-' + pageCounter + '.json');
>    // Quando i dati sono caricati
>    ourRequest.onload = function() {
>        // Converte i dati in un oggetto JSON
>        var ourData = JSON.parse(ourRequest.responseText);
>        renderHTML(ourData);
>    };
>    //  Invia la richiesta
>    ourRequest.send();
>    pageCounter++;
>    if (pageCounter > 3) {
>        // Disabilita il bottone
>        btn.prop("disabled", true);
>        // Nascondi il bottone
>        btn.animate({opacity: 0}, 1000);
>        // Esegui dopo 1.1 secondi
>        setTimeout(function() {
>            btn.remove();
>        }, 1100);
>    }
>});
>
>/**
> * Funzione che inserisce i dati nel file html
> */
>function renderHTML(data) {
>    var htmlString = "";
>    for (i = 0; i < data.length; i++){
>        htmlString += "<p>" + data[i].name + " is a " + data[i].species + " that likes to eat "
>        + data[i].foods.likes + " and dislikes " + data[i].foods.dislikes
>        + ".</p>";
>    }
>    animalContainer.append(htmlString);
>}
>```


> [!example]- <font color="orange">Esempio 3 (utilizza JQuery anche per AJAX)</font>
>```js
>/**
>* Variabile che contiene il numero di iterazione
>*/
>var pageCounter = 1;
>
>/**
>* Variabile che contiene lo spazio dove inserire i dati
>*/
>var animalContainer = $("#animal-info");
>
>/**
>* Variabile che contiene il pulsante
>*/
>var btn = $("#btn");
>
>// Al click del pulsante
>btn.click(function() {
>    // Effettua una richiesta AJAX
>    $.ajax({
>        url: 'https://learnwebcode.github.io/json-example/animals-' + pageCounter + '.json',
>        method: 'GET',
>        dataType: 'json',
>        success: function(resultJson) {
>            renderHTML(resultJson);
>        },
>        error: function() {
>            console.log('Errore nella richiesta AJAX');
>        }
>    });
>
>    pageCounter++;
>
>    if (pageCounter > 3) {
>        // Disabilita il bottone
>        btn.prop("disabled", true);
>        // Nascondi il bottone
>        btn.animate({ opacity: 0 }, 1000);
>        // Esegui dopo 1.1 secondi
>        setTimeout(function() {
>            btn.remove();
>        }, 1100);
>    }
>});
>
>
>/**
>* Funzione che inserisce i dati nel file html
>*/
>function renderHTML(data) {
>	var htmlString = "";
>	for (i = 0; i < data.length; i++) {
>		htmlString += "<p>" + data[i].name + " is a " + data[i].species + " that likes to eat "
>			+ data[i].foods.likes + " and dislikes " + data[i].foods.dislikes
>			+ ".</p>";
>	}
>	animalContainer.append(htmlString);
>}
>```


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