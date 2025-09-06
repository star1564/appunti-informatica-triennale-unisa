---
aliases: 
  - Experiment Tracking
  - Versioning
tags:
  - corsi/informatica/machine_learning
paragrafo: Model Development and Training
cssclasses:
  - 
---
È importante tenere traccia di tutte le informazioni necessarie per *ricreare un esperimento* e i relativi artefatti durante la scelta di un modello.

Un **artefatto** è un file generato durante un esperimento: ad esempio, i file che mostrano la curva di perdita o i risultati intermedi di un modello durante il processo di addestramento.

Il confronto di diversi esperimenti può aiutare a capire come *piccole modifiche influenzano le performance di un modello*, il che, a sua volta, offre una maggiore visibilità sul funzionamento dello stesso.

## Experiment Tracking
È importante *tenere traccia di ciò che accade durante l'addestramento*, non solo per individuare e risolvere problemi di [[Underfitting]] o [[Overfitting]], ma anche per
valutare se il modello sta imparando qualcosa di utile. 

>Il *processo di tracciamento dei progressi e dei risultati* di un esperimento prende il nome di **Experiment Tracking**. Questo consente anche il confronto tra gli esperimenti, osservando come la modifica di un componente influisce sulle performance di un modello.

Questi sono gli aspetti da tracciare per un esperimento:
- Le **metriche di prestazione del modello**: come [[019 Gestire Dataset Sbilanciati#^363748|Accuracy]], [[019 Gestire Dataset Sbilanciati#^7c5678|F1 Score]], perplessità;
- Il log del **campione, della previsione e dell'etichetta di verità corrispondente**: è utile per le analisi ad hoc e per il controllo della correttezza;
- La **velocità** del modello, valutata dal numero di step al secondo o, se i dati sono testuali, dal numero di token elaborati al secondo;
- **Misurazioni delle prestazioni del sistema**: come l'utilizzo della memoria e della CPU/GPU,  importanti per identificare i colli di bottiglia ed evitare di sprecare le risorse del sistema;
- I valori nel tempo di tutti i **parametri e [[Iper-parametro|iper-parametri]]** le cui variazioni possono influenzare le performance del modello, come ad esempio il learning rate.

## Versioning
>Il *processo di memorizzazione di tutti i dettagli* di un esperimento allo scopo di riprodurlo in seguito o di confrontarlo con altri esperimenti prende il nome di **Versioning**.

I sistemi di ML sono in parte codice e in parte dati, quindi non è sufficiente effettuare il versioning solo del codice, ma bisogna farlo anche dei dati. Ci sono alcuni motivi per cui il versioning dei dati è una sfida. 

Uno di questi è che, poiché i dati sono spesso molto più grandi del codice, non possiamo usare la stessa strategia che di solito si usa con il codice.

### Versioning di Codice
Il versioning del codice viene effettuato tenendo traccia di tutte le modifiche apportate a una base di codice.
Una modifica è nota come **diff**, abbreviazione di difference, che viene misurata con un confronto per righe.

Una riga di codice è di solito abbastanza corta affinché il confronto per righe abbia senso.

Gli strumenti di versioning del codice consentono agli utenti di tornare a una versione precedente mantenendo le copie di tutti i vecchi file (come su [[015 Git e GitHub|Github]]).

### Versioning dei dati
In una riga di dati, soprattutto se memorizzati in formato binario, il confronto può essere indefinitamente lungo.

Un set di dati potrebbe essere così grande che duplicarlo più volte (per avere più versioni precedenti, come nel codice) potrebbe non essere fattibile.


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