---
aliases: 
tags:
  - corsi/informatica/tecnologie_software_web
paragrafo: JavaScript
cssclasses:
  - 
---
>**jQuery** è una *libreria* JavaScript popolare e potente che *semplifica l'interazione con il [[030 JS ed elementi HTML#^fdd485|DOM]]* e fornisce una vasta gamma di funzionalità per manipolare elementi HTML, gestire eventi, effettuare chiamate [[035 AJAX|AJAX]] e altro ancora. È progettato per semplificare lo sviluppo di applicazioni web interattive, fornendo un'API più concisa e coerente rispetto a JavaScript puro.

Panoramica su come utilizzare jQuery:

1. *Includi la libreria* jQuery: può essere scaricata dal [sito ufficiale](https://jquery.com/) (vai alla pagina di download, scegli una versione, `CTRL` + `S` per salvare quello che esce in un file `.js`) e si include nel documento HTML utilizzando un tag `<script>`.

```html
<script src="jquery.min.js"></script>
```

2. Utilizza la *sintassi* ***`$`*** o `jQuery` *per selezionare gli elementi* nel DOM: SI possono usare [[024 CSS#Selettore di Id e Classi|selettori CSS]] per selezionare elementi HTML specifici o gruppi di elementi.

![[Pasted image 20230615162511.png|300]]

```javascript
// Seleziona un elemento con un determinato ID
var element = $("#myElement");

// Seleziona tutti gli elementi <p> all'interno di un determinato elemento
var paragraphs = $("#myContainer p");

// Seleziona tutti gli elementi con una determinata classe
var elementsWithClass = $(".myClass");
```

3. Utilizza i metodi e le funzioni fornite da jQuery per *manipolare gli elementi selezionati*: Ad esempio, si può modificare il contenuto di un elemento, aggiungere o rimuovere classi, modificare gli attributi e molto altro ancora.

```javascript
// Modifica il testo di un elemento
element.html("<p>Nuovo testo</p>");

// Aggiungi una classe a un elemento
element.addClass("myClass");

// Rimuovi una classe da un elemento
element.removeClass("myClass");

// Modifica un attributo di un elemento
element.attr("href", "http://www.example.com");
```

4. *Gestisci gli eventi* utilizzando i metodi di event handling forniti da jQuery. Puoi assegnare funzioni di callback agli eventi e rispondere alle azioni dell'utente.

```javascript
// Assegna un evento di clic a un elemento
element.click(function() {
  // Codice da eseguire quando l'elemento viene cliccato
});

// Assegna un evento di submit a un form
form.submit(function(e) {
  // Codice da eseguire quando il form viene inviato
  e.preventDefault(); // Impedisce l'invio del form
});
```

5. Sfrutta le funzionalità avanzate di jQuery, come le animazioni, le richieste [[035 AJAX|AJAX]] e le operazioni sulle collezioni di elementi.

```javascript
// Esegui un'animazione su un elemento
element.animate({ opacity: 0.5 }, 1000);

// Effettua una richiesta AJAX
$.ajax({
  url: "example.php",
  success: function(data) {
    // Codice da eseguire quando la richiesta ha successo
  }
});

// Utilizza metodi per lavorare con collezioni di elementi
var elements = $(".myClass");
elements.each(function() {
  // Codice da eseguire per ogni elemento nella collezione
});
```

Questo è solo un'introduzione di base all'utilizzo di jQuery. La libreria offre molte altre funzionalità e metodi che puoi esplorare nella documentazione ufficiale di jQuery (https://api.jquery.com/).


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