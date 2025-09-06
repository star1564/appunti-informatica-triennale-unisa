---
aliases: Mesh Poligonale
tags:
  - corsi/informatica/grafica_interattivita
paragrafo: Introduzione
cssclasses:
  - 
---
>Uno dei principali scopi della CG 3D è la produzione di immagini [[001 Introduzione a Grafica#Rappresentazione con Raster|Raster]] *a partire da una descrizione geometrica* (**modello tridimensionale**) di una scena. 

Dal momento che un modello tridimensionale è anche una rappresentazione [[001 Introduzione a Grafica#Rappresentazione con Vettori|Vettoriale]] della scena, è necessario introdurre delle primitive in grado di descrivere la scena stessa. In generale possiamo dire che ogni forma tridimensionale $O$ è descrivibile attraverso una sua rappresentazione approssimata e discreta *basata su un insieme di punti* ($V_1, V_2,\dots, V_n$) ciascuno con coordinate ($V_ix, V_iy, V_iz$) rispetto ad un dato sistema di riferimento 3D (**nuvola o insieme di punti**).

Questa “nuvola” di punti *è connessa in modo da formare una superficie poligonale* (**mesh**) in cui i punti sono i vertici dei poligoni. In termini geometrici e computazionali c'è un notevole vantaggio ad usare unicamente i poligoni più semplici: *triangoli* e *quadrilateri*.

![[Pasted image 20240228191025.png]]

Ogni entità (mesh) che compone un modello poligonale è caratterizzata da *Vertici, Lati e Facce*. 

![[Pasted image 20240228191242.png]]


### Risoluzione di una Mesh Poligonale
Similarmente alle immagini, anche le mesh poligonali hanno il concetto di **risoluzione**, legato al *livello di approssimazione* con cui un certo numero di vertici e poligoni riesce a descrivere una superficie continua.

Ovviamente con il crescere del numero dei poligoni (e dei vertici) si avrà una simulazione più fedele anche di forme complesse, questo vantaggio sarà tanto più evidente quanto più morfologicamente complessa (ricca di curve) sarà la forma da descrivere.

![[Pasted image 20240228191618.png]]

### Rappresentazione Wireframe
Dal momento che ogni modello tridimensionale è una rete connessa di poligoni, la prima rappresentazione visiva che storicamente è stata adottata a fini visualizzativi è il cosiddetto **Wireframe**, che descrive un oggetto *solamente tramite i lati di tutti i poligoni che lo compongono*. 

Tale rappresentazione, ispirata originariamente da esigenze di semplicità computazionale, è tuttora adottata perché consente una completa comprensione della geometria elementare del modello.

![[Pasted image 20240228191859.png]]


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