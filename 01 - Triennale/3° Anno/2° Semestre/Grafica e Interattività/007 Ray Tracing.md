---
aliases: 
  - Ray Casting
  - Path Tracing
tags:
  - corsi/informatica/grafica_interattivita
paragrafo: Illuminazione
cssclasses:
  - 
---
>Il **Ray Tracing** è una tecnica per *modellare il trasporto della luce* e si utilizza in una vasta varietà di algoritmi di [[005 Radianza#Rendering|rendering]].

Funziona *simulando* il modo in cui *la luce viaggia* dal punto di vista della [[008 Telecamera|telecamera]] attraverso la scena, interagisce con gli oggetti (riflettendo, rifrangendo o proiettando ombre) e infine raggiunge le sorgenti luminose. 

Questo processo è più *efficiente* rispetto al tracciamento di ogni singolo raggio di luce e consente di ottenere [[005 Radianza#Riflessione|riflessi]], ombre e [[006 Illuminazione Locale#^a27a90|illuminazione globale]] della scena accurati.

![[Pasted image 20240314110235.png|600]]

L'algoritmo di Ray Tracing è [[028 Ricorsione (Programmazione)|ricorsivo]] e costruisce un'immagine estendendo i raggi in una scena e *facendoli rimbalzare sulle superfici* e verso le fonti di luce per approssimare il valore del colore dei pixel.

## Tecniche basate sul Ray Tracing
Esistono diverse tecniche di rendering basate sul ray tracing:

- Nel **Ray Casting** i raggi vengono proiettati dalla telecamera attraverso *ogni pixel* del piano dell'immagine. Questi raggi vengono poi *controllati* per vedere se avviene una *collisione con gli oggetti* della scena. Se un raggio colpisce un oggetto, viene calcolata la sua distanza dalla telecamera e i dati sul colore dell'oggetto influenzano il colore finale del pixel. Il raggio può anche *rimbalzare* su altri oggetti, *raccogliendo ulteriori informazioni* sul colore e sull'illuminazione lungo il suo percorso. Può essere esteso per includere ombre, riflessioni e superfici trasparenti.
 ^40888e
- Il **Path Tracing** è una versione avanzata del ray tracing in cui centinaia o migliaia di raggi vengono *tracciati attraverso ogni pixel*. Questi raggi rimbalzano o attraversano gli oggetti più volte prima di raggiungere la sorgente luminosa, consentendo di raccogliere informazioni dettagliate sul colore e sull'illuminazione.


___
[[000 Indice Grafica|↖ Ritorna all'indice ↖]]

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