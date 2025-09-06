---
aliases: 
tags:
  - corsi/informatica/machine_learning
paragrafo: Features Engineering
cssclasses:
  - 
---
>La **Discretizzazione** è il *processo di trasformazione di una variabile continua in una variabile discreta*, noto anche come quantizzazione o binning. Sebbene *comporti una perdita di informazioni*, è ampiamente utilizzato nei processi di Machine Learning e nell'addestramento di modelli.

![[Pasted image 20231216114037.png|400]]

L'obiettivo della discretizzazione è ridurre la complessità dei dati e semplificare il modello, *rendendo più gestibili le variabili continue*, andando a *determinare il minor numero di intervalli possibile* senza perdere in modo significativo le informazioni, determinandone i *punti limite*.

![[Pasted image 20231216114117.png|400]]

Diversi modelli di [[008 Task di Regressione|Regressione]] e [[007 Task di Classificazione|Classificazione]] funzionano meglio con valori discreti, facendo anche *accelerare il processo di addestramento*, riducendo al minimo l'influenza dei *valori anomali*.

> [!example]- <font color="orange">Esempio</font>
>![[Pasted image 20231216120054.png]]
>
>Si può fare la discretizzazione del reddito annuale:
>- inferiore: $x<35.000$
>- medio: $35.000\leq x \leq 100.000$
>- superiore: $x>100.000$

## Metodi di Discretizzazione
### Discretizzazione ad uguale ampiezza
>La **Discretizzazione ad uguale ampiezza** consiste nel dividere l'intervallo di valori continui *in $k$ intervalli di uguale dimensione*.

> [!example]- <font color="orange">Esempio</font>
>Data una variabile il cui range di valori va da 0 a 100:
>- $k=2 \to$ (0-50), (50-100)
>- $k=5 \to$ (0-20), (20-40), (40-60), (60-80), (80-100)

Questo metodo *non altera in modo drammatico* la distribuzione delle variabili. Se una variabile è distorta *prima della discretizzazione*, sarà comunque *distorta* dopo la discretizzazione.

### Discretizzazione ad uguale frequenza
>La **Discretizzazione ad uguale frequenza** ordina la variabile continua in intervalli *con lo stesso numero di osservazioni*. La larghezza dell'intervallo è determinata dai *quantili*.

È particolarmente utile per le *variabili asimmetriche*, poiché *distribuisce equamente* le osservazioni sui diversi gruppi.

### Discretizzazione con approcci non supervisionati
>La **Discretizzazione con approcci non supervisionati** è una tecnica *non supervisionata* perché i limiti dell'intervallo vengono trovati in *modo autonomo* da un algoritmo.

Per creare intervalli che raggruppano osservazioni simili, possiamo usare *algoritmi di clustering* (come il k-means). In essi, le partizioni sono dei *cluster* che vengono identificati dall'algoritmo, avendo come input il numero di cluster $k$. ^707338

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