---
aliases:
  - Etichetta
tags:
  - corsi/informatica/machine_learning
paragrafo: Training Data
cssclasses:
  - 
---
## Etichetta
>Un'**Etichetta** (o *label*) è un'*annotazione associata a un dato*, che fornisce informazioni o *categorizza il dato in una specifica classe o categoria*. 

Nell'[[002 Apprendimento con Supervisione Umana|Apprendimento Supervisionato]], le etichette sono utilizzate per addestrare modelli di Machine Learning, indicando quale classe o categoria deve essere associata a ciascun esempio di dati nel dataset di addestramento.

![[Pasted image 20231215182910.png|500]]

Ad esempio, in un problema di classificazione di immagini, le etichette potrebbero indicare se un'immagine raffigura un cane o un gatto.

## Etichettatura dei Dati
In un modello con [[002 Apprendimento con Supervisione Umana|Apprendimento Supervisionato]] necessita di **Dati Etichettati**. Quindi in questo tipo di apprendimento, l'etichettatura dei dati è un processo fondamentale.

>L'**Etichettatura dei Dati** (*Data Labeling*) è il processo di aggiunta di *una o più etichette ai dati grezzi* per renderli identificabili in un contesto specifico. I modelli di apprendimento automatico possono quindi sfruttare queste etichette per classificare i dati in modo appropriato e apprendere dalle interazioni con i dati.

Ci sono due tecniche per etichettare i dati:
- Manualmente;
- Tecniche Automatiche.

![[Pasted image 20231215191450.png|800]]

### Etichettatura Manuale
>L'**Etichettatura Manuale** è una tecnica in cui le etichette vengono *assegnate manualmente a ciascun campione* presente nel dataset.

> [!example] <font color="orange">Esempio</font>
>Ad esempio, se vogliamo classificare 20 e-mail come "spam" o "non spam", utilizziamo questa tecnica esaminando ogni singola e-mail e assegnando un'etichetta in base a una valutazione soggettiva.

Questa tecnica presenta alcune sfide:
- **Costi Elevati**: etichettare manualmente un piccolo numero di campioni può essere relativamente economico, ma il costo aumenta significativamente quando si tratta di grandi volumi di dati. L'impiego di annotatori esperti può aumentare ulteriormente i costi.

- **Privacy**: alcuni dati potrebbero essere sensibili e non tutti gli annotatori dovrebbero avere accesso a tutte le informazioni. Ad esempio, le informazioni personali dovrebbero essere trattate con rigidi vincoli di privacy.

- **Tempi Prolungati**: etichettare manualmente grandi quantità di dati richiede tempo, influenzando la capacità del modello di adattarsi rapidamente ai cambiamenti nell'ambiente operativo.

- **Ambiguità (o Molteplicità) delle Etichette**: spesso, abbiamo la necessità di ottenere un gran numero di dati etichettati da diverse [[010 Fonti e Formati dei dati#Fonti di dati|fonti]], il che implica la presenza di annotatori diversi con varie competenze. Questa diversità può tradursi in *differenti interpretazioni dello stesso concetto*, portando alla possibilità di assegnare etichette contrastanti per uno stesso elemento. Tale situazione richiede una gestione attenta per garantire la coerenza e l'affidabilità delle etichette attribuite ai dati.

#### Data Lineage
Analizzare la *qualità dei dati provenienti da fonti diverse* è fondamentale, quindi è importante *tener traccia dell'origine dei dati* che si utilizzano. Questa tecnica viene chiamata **Data Lineage** ed ha un duplice vantaggio:
- Ci permette di *individuale potenziali bias*;
- Ci permette di fare *debug* del nostro modello facilmente.

Questo perché il problema non spesso risiede nel modello, ma nei dati utilizzati, soprattutto a causa della presenza di molti errori di etichettatura.

### Etichette Naturali
Esistono dati che non hanno bisogno di un processo di etichettatura a mano, in quanto *hanno già delle etichette assegnate*. Si dice che questi dati hanno delle **Etichette Naturali**.

I task con etichette naturali, quindi, sono task dove le predizioni del modello possono essere *automaticamente valutate dal sistema*, senza l'intervento umano.

> [!example] <font color="orange">Esempio</font>
>I sistemi di raccomandazione sono dei task con etichette naturali, poiché ogni volta che un utente clicca su un prodotto raccomandato, il sistema può assumere che questo sia un esempio "positivo", mentre altri prodotti non cliccati possono essere visti come esempi "negativi".

Ci sono due tipi di etichette in questo caso: 
- **Etichette Implicite**: la loro assegnazione *deriva dalla mancanza di feedback diretti* da parte dell'utente.
	- Per esempio, non vengono cliccati dei prodotti raccomandati. A questi prodotti vengono assegnate delle etichette negative.
- **Etichette Esplicite**: la loro assegnazione *deriva dal feedback diretto* da parte dell'utente.
	- Per esempio, quando viene cliccato un prodotto raccomandato. A questo prodotto viene assegnata una etichetta positiva.

Però questa metodologia potrebbe recare danni al nostro modello, dato che *il feedback dell'utente potrebbe essere erroneo* (click per sbaglio, ecc...).

Quindi si introduce la **Lunghezza del ciclo di feedback**, ovvero *il tempo che passa* da quando una predizione viene fornita fino a quando viene generato un feedback su di essa. La scelta della giusta lunghezza implica un *compromesso tra accuratezza e velocità*.

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