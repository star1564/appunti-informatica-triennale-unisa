---
aliases: 
tags:
  - corsi/matematica/ricerca_operativa
paragrafo: Introduzione
cssclasses:
  - 
---
> [!error] !!! Le note di Ricerca Operativa non sono complete e in parte sono errate !!!

>La **Ricerca Operativa** si occupa dello sviluppo e dell'applicazione di *metodi matematici per la soluzione di problemi di decisione* in presenza di *risorse limitate* attraverso strumenti matematici o informatici.

L'aspetto fondamentale della Ricerca Operativa è quello di *identificare un modello matematico* con cui studiare in modo sistematico il problema decisionale.

## Approccio Modellistico
La costruzione di modelli matematici per la soluzione di problemi reali avviene attraverso diverse fasi:
- **Analisi del problema**: consiste nell'analisi della *struttura del problema* per individuare i legami logico-funzionali e gli obiettivi.

- **Costruzione del modello**: si descrivono in *termini matematici* le caratteristiche principali del problema.

- **Analisi del modello**: prevede la *deduzione per via analitica*, in riferimento a determinate classi di problemi, *di alcune importanti proprietà* quali esistenza ed unicità della soluzione ottima, condizioni di ottimalità e stabilità in caso di variazioni.

- **Soluzione numerica**: si individua, mediante opportuni *[[001 Introduzione a PA#^Algoritmo|Algoritmo]] di calcolo*, la soluzione che deve essere verificata dal punto di vista applicativo.

- **Validazione**: avviene attraverso una *verifica sperimentale* oppure con metodi di simulazione.

### Struttura dei problemi decisionali
La struttura per la risoluzione dei problemi decisionali è data dal **Modello Matematico**, composto da:
- *Variabili decisionali*
- *Funzioni Obbiettivo*
- *Vincoli*

#### Formulazione dei problemi decisionali
- *Decisione*: processo di selezione tra più alternative
- Alternative *finite o infinite*
- Alternative definite *esplicitamente o implicitamente*
- Scelta sulla base di uno o più *criteri* (obiettivi)
- Condizioni di *certezza e incertezza*

#### Caratteristiche dei problemi che saranno considerati
- Condizioni di certezza (problemi deterministici)
- Presenza di un solo criterio (singolo obiettivo)


___
[[000 Indice RO|↖ Ritorna all'indice ↖]]

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