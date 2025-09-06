---
aliases:
  - HTML
tags:
  - corsi/informatica/tecnologie_software_web
paragrafo: HTML
cssclasses:
  - 
---
Il [[Server]], quando effettua una [[004 Richiesta e Risposta HTTP|risposta]] contenente una pagina, invia al [[Client]] un insieme di istruzioni scritte in **HTML** (*HyperText Markup Language*).

Questo è un linguaggio utilizzato per creare pagine [[001 Introduzione a TSW|Web]].

La caratteristica principale di questo linguaggio è il suo utilizzo di **tags** ed **attributi** che descrivono entrambi al browser come formattare la pagina.
*Alcune* di queste sono:

| Tag            | Descrizione                                                            |
| -------------- | ---------------------------------------------------------------------- |
| `<!-- -->`     | *Commento*                                                             |
| `<a>`          | *Anchor*, usato per inserire link                                      |
| `<align>`      | *Allineamento* del contenuto (sinistra, destra, centro o giustificato) |
| `<body>`       | Definisce i bordi del **Corpo** del documento                          |
| `<br>`        | *Line Break* (il `/n` di HTML)                                         |
| `<center>`     | *Centra* il contenuto                                                  |
| `<form>`       | Definisce un *Form* che solitamente fornisce campi di input            |
| `<hNUM>`       | Specifica il tipo di *Heading (titolo)* del testo (dove 1<=NUM<=6)     |
| `<head>`       | Definisce i bordi dell'**Header** del documento                        |
| `<html>`       | Definisce i bordi del documento **HTML**                               |
| `<input type>` | Definisce un *Widget di Input* per un Form                             |
| `<p>`          | Un nuovo *Paragrafo*                                                   |
| `<title>`      | Il *Titolo della Pagina HTML* mostrato nella barra                     |
| `<img/>`       | Importa una *Immagine* e posizionala nel documento                     |
| `<!DOCTYPE>`   | Definisce il *Tipo di Documento*                                     |
| `<div>`        | È un tag di **Contenitore** che può contenere altri elementi HTML                                                                       |

Una volta che si è finito di modificare un elemento con uno di questi tag, lo si chiude con **`</tag>`** (dove `tag` è il nome del tag da chiudere).

> [!tip] [w3schools con tutte le definizioni dei tag](https://www.w3schools.com/tags/default.asp)

---
### TAG PRINCIPALI
Ci sono quattro tag da utilizzare sempre in un foglio HTML. Questi sono:
- `<!DOCTYPE>` (opzionale ma utile): dichiarazione che rappresenta il *tipo del documento*, aiutando il browser a mostrare correttamente la pagina HTML.
	- Appare prima del tag `<html>`.
- `<html>`: è l'elemento *radice* di una pagina HTML. 
	- Contiene tutti gli [[007 Elementi e Attributi HTML#ELEMENTO|elementi]].
- `<head>`: contiene *informazioni* sulla pagina HTML attraverso i [[Metadato|metadati]].
- `<body>`: definisce il *corpo* della pagina HTML ed è un contenitore per tutti gli elementi visibili.

---
### STRUTTURA PAGINA HTML
Questa è la struttura base di una pagina HTML:

![[Pasted image 20230325111923.png|1000]]

> [!example]- <font color="orange">Esempio</font>
>```HTML
><!-- Esempio di una pagina HTML -->
><!DOCTYPE html>
><html>
>	<head>
>		<title>A Login Page</title> <!-- A -->
>	</head>
>	
>	<body>
>		<h1 align="center">Skyler's Login Page</h1> <!-- B -->
>		
>		<p align="right">
>			<img src="SKYLER2.jpg" width="130" height="150"/> <!-- C -->
>		</p>
>		
>		<form action="date2"> <!-- D -->
>			Name: <input type="text" name="param1"/><br/>
>			Password: <input type="text" name="param2"/><br/><br/><br/>
>		
>			<center>
>				<input type="SUBMIT"> <!-- E -->
>			</center>
>		</form>
>	</body>
></html>
>```
>
>![[Immagine 2023-03-02 182914.png.jpg|600]]

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