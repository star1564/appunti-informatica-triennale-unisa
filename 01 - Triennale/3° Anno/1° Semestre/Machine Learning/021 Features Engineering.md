---
aliases: Feature, Feature Categorica
tags:
  - corsi/informatica/machine_learning
paragrafo: Features Engineering
cssclasses:
  - 
---
## Feature
>Una **Feature** è una *variabile o caratteristica* che l'algoritmo di machine learning prende in considerazione quando deve effettuare una predizione.

Ogni dataset può contenere varie tipologie di features:
- **Numeriche** (standard, dette *quantitative*): hanno un significato matematico e possono essere ordinabili.
- **Categoriche** (o *qualitative*): rappresentano caratteristiche, gruppi distinti e un numero contato di opzioni, tutto con o senza avere un ordine logico;
	- Possono anche assumere valori numerici (per esempio il numero del mese, invece del nome).

![[Pasted image 20231216133454.png|600]]

> [!example]- <font color="orange">Esempio</font>
>Un modello per la predizione del rischio di arresto cardiaco potrebbe avere le seguenti feature:
>- Età
>- Genere
>- Peso
>- È fumatore
>- È diabetico
>- ecc...

## Feature Engineering

>Con il termine **Feature Engineering** si intende l'insieme dei processi di *selezione, manipolazione e trasformazione di features* ottenute dai dati grezzi, in modo da poter utilizzare le funzionalità per l'addestramento e la previsione.

È una parte fondamentale del processo di creazione di un [[Algoritmo e Modello ML#^f330fe|modello]] di machine learning, il quale può richiedere più tempo rispetto a tutti gli altri processi.

> [!example]- <font color="orange">Esempio</font>
>Abbiamo una feature stringa con punteggiatura, contrazioni o altre caratteristiche che il modello potrebbe trovare difficili da decifrare. Il feature engineering permette di semplificare la feature in modo che sia più comprensibile dal modello. Questo è un problema tipico del NLP (Natural Language Processing).
>
>![[Pasted image 20231216095454.png]]
### Processi di Feature Engineering

Il Feature Engineering comprende diversi processi volti a migliorare la qualità e l'efficacia delle caratteristiche utilizzate nei modelli di machine learning:

1. **Creazione o Rimozione di Variabili**: coinvolge la *generazione* di nuove variabili o la *rimozione* di quelle non rilevanti per migliorare le prestazioni del modello.

2. **Trasformazione di Variabili**: è definita come una funzione che *converte le caratteristiche* da una rappresentazione a un'altra.
	- L'obiettivo è tracciare e visualizzare i dati, accelerare l'addestramento o aumentare la precisione del modello.

3. **Estrazione delle Funzionalità**: coinvolge processi che *estraggono informazioni utili* da un dataset.
	- L'obiettivo è comprimere i dati in quantità gestibili senza distorcere le relazioni originali o le informazioni significative.

4. **Analisi Esplorativa dei Dati (EDA)**: strumento potente per *migliorare la comprensione dei dati* mediante rappresentazioni grafiche come istogrammi e diagrammi.
	- Applicato per creare nuove ipotesi o identificare relazioni nei dati, specialmente su grandi quantità di dati qualitativi o quantitativi non precedentemente analizzati.

### Operazioni del Feature Engineering
Le tecniche più usate per la progettazione delle caratteristiche (strettamente legate al dominio del problema) includono:
- [[022 Gestione di Valori Mancanti|Gestione di Valori Mancanti]]
- [[023 Scaling o Ridimensionamento|Scaling o Ridimensionamento]]
- [[024 Discretizzazione|Discretizzazione]]
- [[025 Feature Crossing|Feature Crossing]]
- [[026 Codifica delle Features Categoriche|Codifica delle Features Categoriche]]
- [[027 Perdita di Dati|Perdita di Dati]]





___
[[000 Indice ML|↖ Ritorna all'indice ↖]]

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