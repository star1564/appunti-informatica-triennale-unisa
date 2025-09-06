---
aliases: 
  - Grafica Raster (Bitmap)
  - Grafica Vettoriale
tags:
  - corsi/informatica/grafica_interattivita
paragrafo: Introduzione
cssclasses:
  - 
---
>Con il termine **Computer Graphics** si fa riferimento ad una parte dell'informatica che propone *lo sviluppo di tecniche capaci di simulare visivamente la realtà* che ci circonda attraverso l'uso di [[001 Introduzione a IS#Software|Software]] e hardware specifici.

Poiché la realtà è intrinsecamente *tridimensionale* anche una sua rappresentazione fedele dovrebbe esserlo. Quindi, per rappresentare una **scena**, si utilizza un modello geometrico che rappresenta ogni suo oggetto attraverso *coordinate spaziali a tre dimensioni*.

### Rappresentazione con Raster
Qualunque sia la rappresentazione digitale di una scena o la tipologia di software utilizzati, il risultato, per poter essere visualizzato, deve essere convertito in una **bitmap** (array bidimensionale) di valori tipicamente ad *8-16-24-32 bit*, detti **pixel** (picture-element) dell'immagine. 

Un pixel è l'elemento più piccolo di una immagine su uno schermo. Un'immagine è composta da più pixel raggruppati attraverso una bitmap.

Le immagini **Raster** utilizzano bitmap per archiviare informazioni. 

![[Pasted image 20240228181730.png]]

Lo svantaggio delle immagini raster è che se *ingrandiamo* un'immagine digitale è possibile vedere i *pixel originali trasformarsi in blocchi sempre più grandi* (2x2, 4x4, 8x8, etc.), dilatando le dimensioni dell'immagine stessa ma producendo un degradamento visivo via via crescente.

### Rappresentazione con Vettori
Con la rappresentazione **Vettoriale** vengono conservate le relazioni spaziali e dimensionali tra le entità geometriche elementari che compongono la scena. Vengono quindi usati *comandi sequenziali o istruzioni matematiche* che posizionano *linee o forme* in un ambiente 2D o 3D. 

La grafica vettoriale è la migliore per la stampa poiché è composta da una serie di curve matematiche. Di conseguenza, la grafica vettoriale viene stampata in modo nitido anche quando viene ingrandita. 

- In fisica: un [[055 Spazi Vettoriali#VETTORI FISSATI|vettore]] è qualcosa che ha una grandezza e una direzione. 
- Nella grafica vettoriale: il file viene creato e salvato come sequenza di istruzioni vettoriali. Invece di avere un bit nel file per ogni pezzo del disegno della linea, utilizziamo comandi che descrivono una serie di punti da collegare. Di conseguenza, si ottiene un file molto più piccolo.

![[Pasted image 20240228182254.png]]


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