---
aliases: 
tags:
  - corsi/informatica/machine_learning
paragrafo: Features Engineering
cssclasses:
  - 
---

>La **perdita di dati**, o *data leakage*, si verifica quando un modello viene addestrato *utilizzando informazioni esterne o non appartenenti al dataset* di addestramento.
>L'inclusione di queste informazioni aggiuntive consente al modello di apprendere qualcosa che altrimenti non dovrebbe conoscere, *compromettendo le sue prestazioni previste*.

La perdita di dati può accadere in *modo inconsapevole*, poiché le prestazioni del modello possono sembrare buone in fase di valutazione, ma in realtà non si sa quali dati il modello abbia appreso e da dove provengano.

Questo fenomeno è spesso *insidioso* e *indiretto*, rendendolo difficile da individuare ed eliminare. La perdita di dati può portare un progettista a selezionare un modello non ottimale, che potrebbe essere superato da un modello privo di perdite.

## Cause comuni
### Suddivisione Casuale dei Dati
La **suddivisione casuale dei dati** è una pratica essenziale prima di addestrare un modello di machine learning. Coinvolge la *creazione di due nuovi dataset*:
- **Training**: il dataset più ampio utilizzato per *addestrare* il modello.
- **Testing**: il dataset utilizzato per *valutare le prestazioni* del modello.

Sebbene il metodo più comune sia la suddivisione secondo la [legge di Pareto 80/20](https://it.wikipedia.org/wiki/Principio_di_Pareto) (80% in Train e 20% in Test), esistono molte varianti a discrezione del progettista. Tuttavia, questa suddivisione casuale dei dati, se effettuata senza una regola specifica, **può causare perdita di dati**.

### Ridimensionamento
Un errore comune che può portare alla perdita di dati è l'applicazione delle tecniche di [[023 Scaling o Ridimensionamento|ridimensionamento]] sull'intero dataset. La [[023 Scaling o Ridimensionamento#Normalizzazione delle Funzionalità|normalizzazione]] o [[023 Scaling o Ridimensionamento#Standardizzazione delle Funzionalità|standardizzazione]] tra le istanze dovrebbe avvenire *dopo aver suddiviso i dati in set di addestramento e di test*, utilizzando solo i dati di addestramento.

Questo approccio è fondamentale perché il set di test rappresenta dati nuovi e **invisibili**, e *non dovrebbe essere accessibile durante la fase di addestramento*. Utilizzare informazioni dal set di test prima o durante l'addestramento può portare a un *pregiudizio potenziale* nella valutazione delle prestazioni del modello.

È come se si andasse a guardare la soluzione ad un esercizio: si va ad influenzare il nostro processo di elaborazione.

Quando si normalizza il set di test, è importante applicare i parametri di ridimensionamento precedentemente ottenuti dal set di addestramento così com'è, senza ricalcolarli sul set di test. Questo assicura coerenza con il modello e previsioni accurate.

### Imputazione dei Dati Mancanti
Nell'[[022 Gestione di Valori Mancanti#Imputazione|Imputazione Numerica]] potrebbero verificarsi perdite se la media o la mediana vengono calcolate *utilizzando tutti i dati disponibili*.

Anche in questo caso, i dati di test, sono sconosciuti al modello, e dovranno essere presi in considerazione solo i dati di train appena creato.

Questo tipo di perdita è simile al tipo di perdita causata dal ridimensionamento e può essere prevenuta utilizzando solo le statistiche del train per inserire i valori mancanti in tutte le suddivisioni.

### Duplicazione dei dati
Se nei dati sono presenti **duplicati** o **quasi-duplicati**, la mancata rimozione degli stessi prima della suddivisione dei dati potrebbe causare la *comparsa degli stessi
campioni sia nelle suddivisioni di training che di test*. 

La duplicazione dei dati è abbastanza comune nel settore ed è stata riscontrata anche in set di dati di ricerca popolari. Può derivare dalla raccolta di dati o dalla fusione di diverse [[010 Fonti e Formati dei dati#Fonti di dati|fonti di dati]] o a causa dell'elaborazione dei dati.

Una delle tecniche maggiormente impattanti è il [[019 Gestire Dataset Sbilanciati#Sovracampionamento|Sovracampionamento]]. Per evitare questa tipologia di problemi è buona regola *controllare sempre il set di dati*, producendo analisi e statistiche.

### Perdita di gruppo o di generalizzazione
La **perdita di gruppo** si verifica quando un gruppo di osservazioni *condividono una forte correlazione tra di loro*, e il progettista decide di *dividerle casualmente tra train e test*.

> [!example]- <font color="orange">Esempio</font>
>Dataset di pazienti con riferimenti alle TAC polmonari di due settimane
>- Un paziente potrebbe avere due TAC a una settimana di distanza, che probabilmente hanno le stesse etichette sul fatto che contengano segni di cancro ai polmoni.
>- Data la scelta del progettista, è possibile che le due TAC siano suddivise una in train e l'altra in test.

Questo tipo di perdita è comune per le attività di *rilevamento oggettivo* che contengono istanze dello stesso dato scattate a millisecondi di distanza.

È difficile evitare questo tipo di perdita di dati senza capire come siano stati generati.

## Rilevamento della Perdita dei Dati
La perdita di dati è può verificarsi in molte fasi durante le fasi di gestione dei dati:
- Generazione dei dati
- Raccolta dei dati
- Campionamento dei dati
- Suddivisione e scaling
- Elaborazione dei dati

È importante monitorare la perdita di dati *durante l'intero ciclo di vita di un progetto ML*.

Per mitigare la perdita di dati, è di vitale importanza condurre *analisi approfondite* sui dati che partecipano al problema, cosi da capire a fondo come gestire i dati a disposizione.

È buona prassi *ipotizzare su ogni variazione* delle features che appartengono ai dati, positiva e negativa, che queste possono fornire al modello.

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