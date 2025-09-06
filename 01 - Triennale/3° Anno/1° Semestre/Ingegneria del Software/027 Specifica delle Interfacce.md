---
aliases: 
  - Interfacce dei sottosistemi
  - Servizi dei sottosistemi
tags:
  - corsi/informatica/ingegneria_software
paragrafo: Progettazione - Object Design
cssclasses: 
---
>Nella **Specifica delle Interfacce** si definiscono i servizi dei [[019 System Design#Sottosistema|Sottosistemi]] attraverso [[018 Interfacce|Classi di Interfacce]]. Qui, si possono anche identificare operazioni aggiuntive.

^1c797f

Il risultato di questa attività è una specifica completa delle interfacce per ogni sottosistema, chiamate *API del sottosistema*.

## 1 - Identificare attributi ed operazioni mancanti
Durante l'[[013 Analisi dei Requisiti|analisi]], potrebbero esserci sfuggiti molti attributi perché ci siamo concentrati sulla funzionalità del sistema: abbiamo descritto la funzionalità del sistema principalmente con il modello dei [[011 Scenari e Casi d_Uso#Caso d'Uso|casi d'uso]]. 

## 2 - Specificare la visibilità, i tipi e le signatures
### Tipi e Signature
La specificazione dei [[Tipi, Signatures e Visibilità (IS)#^a3165d|tipi]] e delle [[Tipi, Signatures e Visibilità (IS)#^ecb02b|signature]] perfeziona il [[028 Object Design Document|Modello di Object Design]] in due modi: 
- In primo luogo, si aggiungono dettagli al modello, specificando l'intervallo di ciascun attributo.
- In secondo luogo, si mappano le classi e gli attributi del modello di oggetto con i tipi incorporati forniti dall'ambiente di sviluppo.

Si considera anche la relazione tra le classi identificate e le classi dei componenti esistenti.

### Visibilità
In questa fase si determina la [[Tipi, Signatures e Visibilità (IS)#Visibilità|visibilità]] di ogni attributo e operazione. In questo modo, determiniamo quali attributi devono essere accessibili solo indirettamente tramite le operazioni della classe e quali sono pubblici e possono essere modificati da qualsiasi altra classe. 

Allo stesso modo, la visibilità delle operazioni ci permette di distinguere tra le operazioni che fanno parte dell'interfaccia della classe e quelle che sono metodi di utilità a cui si può accedere solo dalla classe.

## 3 - Specificare contratti
In questa fase, si specificano i [[Contratti]] per tutte le operazioni pubbliche di ogni classe attraverso l'[[026 Object Constraint Language|Object Constraint Language]].


___
[[000 Indice IS|↖ Ritorna all'indice ↖]]

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