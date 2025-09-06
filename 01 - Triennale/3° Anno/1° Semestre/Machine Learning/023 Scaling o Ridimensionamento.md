---
aliases: 
tags:
  - corsi/informatica/machine_learning
paragrafo: Features Engineering
cssclasses:
  - 
---
>Lo **Scaling** (o *Ridimensionamento*) di una [[021 Features Engineering#Feature|feature]] è una pratica essenziale che permette la *standardizzazione dei dati* all'interno di un dataset. È molto utile se nei dati ci sono feature con *diverse scale di rappresentazione*.

![[Pasted image 20231216110835.png|500]]

### Motivazioni Principali

1. **Miglioramento delle Prestazioni**: diversi algoritmi di apprendimento automatico, come quelli basati sulla discesa del gradiente, k-NN e SVM, mostrano prestazioni migliori o convergono più rapidamente quando le funzionalità sono ridimensionate;

2. **Prevenzione di Instabilità ed Equità tra le Features**: il ridimensionamento garantisce che tutte le funzionalità abbiano *scale comparabili*, evitando che funzionalità su larga scala possano imporsi nel processo di apprendimento, influenzando così eccessivamente i risultati.

3. **Evitare Problemi di Overflow o Underflow**: senza il ridimensionamento, la presenza di elementi con *scale molto diverse potrebbe causare problemi* di [[Overflow]] o [[Underflow]], influenzando il corretto funzionamento degli algoritmi.

> [!example]- <font color="orange">Esempio</font>
>Notiamo nell'immagine come i valori di "Età" nei dati vanno da 20 a 40, mentre i valori della variabile "Reddito annuo" vanno da 10.000 a 150.000.
>
>Quando inseriamo queste due variabili in un modello ML, non capirà che 150.000 e 40 rappresentano cose diverse. Li vedrà entrambi semplicemente come numeri e, poiché il numero 150.000 è molto più grande di 40, potrebbe dargli maggiore importanza, indipendentemente da quale variabile sia effettivamente più utile per generare previsioni.
>
>![[Pasted image 20231216112403.png|200]]

## Tecniche di Scaling Comuni

### Normalizzazione delle Funzionalità
>La **Normalizzazione** consiste nel ridimensionare le funzionalità *in un intervallo specifico*. Questo processo è particolarmente utile quando la distribuzione delle funzionalità *non segue una distribuzione normale*.


Può avere due tipi di intervalli (*range*):
- **Scaling nel range $[0,1]$**: data una variabile $x$, i suoi valori possono essere ridimensionati per entrare in un intervallo da $0$ a $1$ con la seguente formula $$x'=\frac{\text{min}(x)}{\text{max}(x)-\text{min}(x)}$$ dove se $x$ è:
	- il valore massimo dai dati, allora $x' = 1$
	- il valore minimo dai dati, allora $x' = 0$

- **Scaling in range arbitrari $[a, b]$**: data una variabile $x$, i suoi valori possono essere ridimensionati per entrare in un intervallo da un numero $a$ ad un numero $b$ entrambi arbitrari con la seguente formula $$x'=a+\frac{(x-\text{min}(x))\cdot(b-a)}{\text{max}(x)-\text{min}(x)}$$ dove se $x$ è:
	- il valore massimo dai dati, allora $x' = b$
	- il valore minimo dai dati, allora $x' = a$



### Standardizzazione delle Funzionalità
>La **Standardizzazione** trasforma le funzionalità in modo che abbiano una *media zero* e una [[036 Varianza di una VA Continua|Varianza]] unitaria. È appropriata quando le funzionalità *seguono una distribuzione normale*.

$$x'=\frac{x-\overline{x}}{\rho}$$

Dove:
- $\overline x$ è la media della variabile $x$;
- $\rho$ è la sua derivazione standard.

#### Log Transformation
Nella maggioranza dei casi i modelli di ML si ritrovano ad essere addestrati con dati con una distribuzione distorta, la quale porta ad avere modelli non accurati e non performanti. Per mitigare l'asimmetria dei dati, una tecnica comune è la **log transformation**, la quale applica la [[F09 Funzione Logaritmica|funzione logaritmica]] alle variabili.

![[Pasted image 20231216113852.png|700]]

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