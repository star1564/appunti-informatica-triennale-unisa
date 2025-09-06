---
aliases: 
tags:
  - corsi/informatica/ingegneria_software
paragrafo: Analisi dei Requisiti - Estrazione
cssclasses:
  - 
---
>Il processo di [[009 Estrazione dei Requisiti|Estrazione dei Requisiti]] parte da un **Problem Statement** (o Statement of Work) *sviluppato dal cliente con il manager del progetto* come *descrizione del problema che il sistema deve risolvere* e descrizione della *situazione attuale*. È scritto in un *linguaggio naturale*.

Non è un documento preciso o specifico del sistema.

Il Problem Statement è composto da diverse sezioni:
1. La prima parte del Problem Statement tratta il **Problem Domain**, che rappresenta la situazione corrente fornendo un *contesto*, ovvero *il problema vero e proprio da risolvere*.
2. La seconda parte parla dei **[[008 Requisiti#Requisiti Funzionali|Requisiti Funzionali]]**, che descrivono *cosa deve fare il software*. 
3. La terza parte parla dei **[[008 Requisiti#Requisiti Non Funzionali|Requisiti Non Funzionali]]**, che descrivono *come il software dovrebbe comportarsi*. Tipicamente, parla di:
	- Usabilità
	- Affidabilità
	- Prestazioni
	- Supportabilità
	- Implementazione
	- Interfaccia
	- Packaging
	- Aspetti Legali
4. Questa parte parla degli **[[011 Scenari e Casi d_Uso#Scenario|Scenari]]**, che descrivono le *situazioni* in cui il software deve essere utilizzato. 
	- Questa sezione tipicamente include diversi scenari, dove ogni scenario offre una descrizione dei passi che l'utente deve eseguire per completare una specifica attività.
5. Questa parte descrive l'**Ambiente** in cui il software verrà eseguito e tipicamente include una descrizione dell'hardware, del software esterno e della rete che il software da costruire dovrà utilizzare.
6. Questa parte definisce i **Criteri di accettazione ed i [[008 Requisiti#Vincoli|Vincoli]]** per verificare il superamento dei test. ^9fd154
7. Questa parte descrive i prodotti che devono essere **consegnati** e le **date** entro le quali devono essere consegnati.

---

> [!tip] <font color="orange">Template</font>
>```
>1. Problem Domain
>2. Requisiti Funzionali
>3. Requisiti Non Funzionali
>4. Scenari
>5. Ambiente
>6. Criteri di Accettazione e/o Vincoli
>7. Deliverable e Deadlines
>```


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