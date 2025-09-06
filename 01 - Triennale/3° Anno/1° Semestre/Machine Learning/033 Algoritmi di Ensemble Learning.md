---
aliases: 
tags:
  - corsi/informatica/machine_learning
paragrafo: Model Development and Training
cssclasses:
  - 
---
Quando si considera una soluzione di ML per il proprio problema, si potrebbe iniziare con un sistema che contiene un solo modello. Dopo aver sviluppato un singolo modello, si potrebbe pensare a come migliorare le performance del sistema. Una soluzione sarebbe quella di usare l'ensemble.

>L'**Apprendimento in Ensemble** consiste nel *combinare le previsioni di più modelli*, nel tentativo di aumentare le prestazioni di previsione, dove ogni modello dell'ensemble è chiamato **base learner**.

I metodi di ensembling *sono meno favoriti in produzione* perché sono *più complessi da distribuire e più difficili da manutenere*. Tuttavia, sono ancora comuni per le attività in cui un piccolo aumento delle prestazioni può portare un enorme guadagno economico, come la previsione del tasso di clic per gli annunci pubblicitari.

> [!example]- <font color="orange">Esempio</font>
>Immaginiamo di avere tre classificatori di spam via email, ciascuno con un'accuratezza del 70%. Supponendo che ogni classificatore abbia la stessa probabilità di fare una previsione corretta e che i classificatori non siano correlati, tramite l'utilizzo del voto a maggioranza possiamo ottenere un'accuratezza del 78,4%.
>
>Per ogni email, ogni classificatore ha il 70% di probabilità di essere corretto. L'ensemble sarà corretto se almeno due classificatori sono corretti. 
>La seguente tabella mostra le probabilità dei diversi risultati possibili dell'ensemble per un'email.
>
>![[Pasted image 20240105104911.png]]
>
>Pertanto, l'ensemble avrà un'accuratezza di $0,343 + 0,441 = 0,784$, ovvero del 78,4%.

Se tutti i classificatori dell'ensemble sono perfettamente correlati, questi calcolano la stessa previsione per ogni caso e si avrà la stessa accuratezza di ogni singolo classificatore.

Quando si crea un ensemble, *minore è la correlazione tra i base learner, migliore sarà l'ensemble*. Per questo motivo, è comune scegliere tipi di modelli molto diversi
per un ensemble.

Esistono tre modi per creare un ensemble.

## Bagging
>Il **Bagging** (*bootstrap aggregating*) è stato progettato per migliorare la stabilità dell'addestramento e l'accuratezza degli algoritmi di ML. Esso permette di ridurre la [[036 Varianza di una VA Continua|Varianza]] e aiuta ad evitare l'[[Overfitting|overfitting]].

Dato un set di dati, invece di addestrare un classificatore sull'intero set di dati, si [[013 Training Data|campiona]] con sostituzione *per creare diversi set di dati*, chiamati **bootstrap**. Il campionamento con sostituzione garantisce che ogni bootstrap sia creato **indipendentemente**.

![[Pasted image 20240105105700.png|500]]


| <font color="green">Vantaggi<font> | <font color="red">Svantaggi<font> |
| ---- | ---- |
| Migliora l'accuratezza | Può essere costoso addestrare molti modelli deboli in parallelo |
| Riduce la varianza | Un alto numero di modelli deboli potrebbe ridurre l'interpretabilità |
| Migliora la robustezza del modello sul rumore dei dati | Può degradare leggermente le performance di metodi stabili <br>(come il K-Nearest Neighbors) |
| Migliora generalmente i modelli instabili <br>(come le reti neurali, i classification e regression tree, <br>e la selezione di sottoinsiemi nella regressione lineare) |  |


### Random Forest
Un **random forest** è un esempio di bagging. In particolare, esso è un ensemble composto da un insieme di [[028 Alberi di Decisione|Decision Tree]] costruiti sia con il bagging che con la tecnica feature randomness, dove ogni albero può scegliere solo da un sottoinsieme casuale di feature da utilizzare.

## Boosting
>Il **Boosting** è una famiglia di algoritmi di ensemble *iterativi* che *convertono i learner deboli in learner forti*. Ogni learner di questo ensemble è addestrato sullo stesso insieme di campioni, ma essi sono ponderati in modo diverso nelle iterazioni. Questi campioni sono ponderati *in base agli errori dei learner precedenti nella sequenza*.

Di conseguenza, i futuri learner deboli si concentrano maggiormente sugli esempi che i precedenti learner deboli hanno classificato in modo errato.

![[Pasted image 20240105110141.png|600]]

Il boosting prevede le seguenti fasi:
1. Si inizia addestrando il primo classificatore debole sul set di dati originale;
2. I campioni vengono pesati in base al grado di classificazione del primo classificatore, ad esempio, ai campioni mal classificati viene attribuito un peso maggiore;
3. Viene addestrato il secondo classificatore sul set di dati pesato. L'ensemble è ora composto dal primo e dal secondo classificatore;
4. I campioni sono pesati in base all'accuratezza con cui l'ensemble riesce a classificarli;
5. Viene addestrato il terzo classificatore sul set di dati pesato e viene aggiunto all'ensemble;
6. 🔁 Tali step vengono ripetuti per un certo numero di iterazioni;
7. Si genera un classificatore forte come combinazione pesata dei classificatori esistenti: i classificatori con errori di addestramento minori hanno pesi maggiori.


| <font color="green">Vantaggi<font> | <font color="red">Svantaggi<font> |
| ---- | ---- |
| Migliora l'accuratezza | Può essere costoso |
| Riduce il [[Bias]] andando a migliorare le <br>debolezze del modello precedente | È sensibile ai dati con rumore |
|  | Potrebbero replicarsi degli errori tra i vari modelli |


### Gradient Boosting Machine
Un esempio di algoritmo di boosting è il **Gradient Boosting Machine** (*GBM*), il quale produce un modello di previsione a partire da decision tree deboli.

Costruisce il modello in modo graduale, come fanno altri metodi di boosting, e li generalizza consentendo l'ottimizzazione di una funzione di perdita differenziabile arbitraria.

## Stacking

>Lo **Stacking** consiste nell'addestrare dei base learner sui dati di addestramento e poi creare un **meta-learner** che *combina i risultati dei base learner* per produrre le previsioni finali.

Il meta learner può essere molto semplice: 
- *si prende il voto della maggioranza* (per i task di [[007 Task di Classificazione|classificazione]]);
- *si prende il voto medio* (per i task di [[008 Task di Regressione|regressione]]).

Oppure, questo può essere un altro modello, come un modello di regressione logistica o un modello di regressione lineare, addestrato sulle predizioni dei base learner.

![[Pasted image 20240105110822.png|500]]


| <font color="green">Vantaggi<font> | <font color="red">Svantaggi<font> |
| ---- | ---- |
| Combina i benefici di diversi modelli in uno solo | Potrebbe essere necessario più tempo per addestrare ed <br>aggregare le predizioni di diversi modelli |
| Migliora l'accuratezza | L'addestramento di base learner e del meta-modello aumenta la complessità |



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
- https://www.baeldung.com/cs/bagging-boosting-stacking-ml-ensemble-models