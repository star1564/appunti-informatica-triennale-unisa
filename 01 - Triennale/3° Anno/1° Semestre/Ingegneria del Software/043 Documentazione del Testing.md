---
aliases:
  - Test Plan
  - Test Case Specification
tags:
  - corsi/informatica/ingegneria_software
paragrafo: Testing
cssclasses:
  - 
---
Per fare si che il sistema soddisfi i [[008 Requisiti#Requisiti Funzionali|Requisiti Funzionali]] e [[008 Requisiti#Requisiti Non Funzionali|Requisiti Non Funzionali]], bisogna **pianificare le attività di testing**.

Ci sono quattro tipi di documenti divisi in due tipi.

## Planning Documents
### Test Plan
Il **Test Plan** si concentra sugli *aspetti gestionali* del testing. Documenta gli ambiti, l'approccio, le risorse ed il programma delle attività di test.

In questo documento vengono identificati i requisiti ed i [[035 CVS - Testing#^ef6c59|componenti]] da testare.

> [!tip]- <font color="orange">Template - Test Plan</font>
>```markup
>1. Introduzione
>2. Relazione con gli altri documenti
>3. Panoramica del Sistema
>4. Feature da testare/da non testare
>5. Criteri di Pass/Fail
>6. Approccio
>7. Materiali per il Testing (requisiti hardware/software)
>8. Test cases
>9. Schedule del Testing
>```
>1. Descrive gli obbiettivi del testing.
>2. Spiega le relazioni del documento con il [[017 Requirement Analysis Document|RAD]], [[022 System Design Document|SDD]] e [[028 Object Design Document|ODD]].
>3. Aspetto strutturale del testing, una panoramica del sistema in termini dei componenti testati.
>4. Aspetto funzionale del testing, identifica tutte le feature (componenti, requisiti) e combinazioni di feature da testare.
>5. Specifica i criteri di passaggio e fallimento per ciascun test.
>6. Vengono discusse le ragioni della strategia di [[041 Integration Testing|test di integrazione]] scelta.
>7. Identifica le risorse necessarie per iniziare e continuare il testing.
>8. La parte principale, dove vengono listati i [[037 Concetti di Testing#Test Case|Test Case]] usati per il testing. Ognuno verrà descritto in dettaglio nel prossimo documento.
>9. Una agenda che descrive quando eseguire i test.

### Test Case Specification
Il **Test Case Specification** si occupa di *descrive ogni test case presente nel Test Plan*. Contiene gli input, [[037 Concetti di Testing#Test Stub e Drivers|Test Stub e Driver]], gli output attesi e le attività da svolgere.

> [!tip]- <font color="orange">Template - Test Case Specification</font>
>```markup
>1. Identificatore per i Test Case
>2. Componenti da Testare
>3. Specifiche per l'Input
>4. Specifiche per l'Output
>5. Necessità dell'Ambiente
>6. Requisiti Speciali Procedurali
>7. Dipendenze tra Test Case
>```
>
>1. Indica i nomi per i test case, per distinguerli dagli altri test case.
>2. Lista i componenti da testare.
>3. Lista gli input richiesti da ogni Test Case.
>4. Lista gli output attesi dal testing.
>5. Specifica la piattaforma hardware e software necessaria per eseguire il test, inclusi i Test Stub e Test Driver.
>6. Specifica qualsiasi vincolo necessario per eseguire i test, come il tempo di risposta, carico oppure intervento di un operatore.
>7. Indica le relazioni di dipendenza tra i Test Case.

## Execution Documents
### Test Incident Report
Il **Test Incident Report** contiene i *output effettivi* e le *differenze con gli output attesi* per ogni test eseguito che è risultato *fallito*.
Viene spiegato anche il motivo del fallimento.

### Test Report Summary
Il **Test Report Summary** lista *tutte le [[035 CVS - Testing#^bb2926|Failure]] individuate* durante la fase di testing. In questo documento gli sviluppatori analizzano e danno una priorità a ogni failure e si pianificano i cambiamenti nel sistema e nei modelli.

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