---
aliases:
  - Intervallo illimitato
  - Intervallo illimitato superiormente
  - Intervallo illimitato inferiormente
  - Intervallo limitato
  - Intervallo Aperto
  - Intervallo Chiuso
tags:
  - corsi/matematica/analisi
paragrafo: Nozioni di base
cssclasses:
  - 
---
>Un **Intervallo** $\color{#CC241D}I$ è un [[002 Sottoinsiemi|sottoinsieme]] di $\mathbb{R}$ costituito da un *continuo di punti*, e per definirlo sono sufficienti due numeri reali $a,b$, che vengono chiamati gli *estremi dell'intervallo*.

Graficamente, è indicato come una <font color="#5cc22f">parte</font> della semiretta (o asse) che rappresenta un insieme numerico.

![[Pasted image 20240925162244.png]]

Ci sono due tipi principali di intervalli.

## Intervalli Limitati o Illimitati

### Intervalli Limitati
>Si dice che $I\subseteq \mathbb{R}$ è un **Intervallo Limitato** se *entrambi gli estremi sono valori finiti*. 

Ovvero se gli estremi sono valori reali, quindi: $$a,b\in \mathbb{R}$$ 
> [!example]+ <font color="orange">Esempio</font>
> ![[Pasted image 20240925165600.png|500]]


### Intervalli Illimitati
>Si dice che $I\subseteq \mathbb{R}$ è un **Intervallo Illimitato** se *almeno uno dei due estremi è infinito $\infty$*.

 Dove l'infinito può ha un segno per entrambe le direzioni: 
 - $-\infty$ viene applicato all'estremo sinistro
 - $+\infty$ viene applicato all'estremo destro




Ci sono tre tipi di intervalli illimitati.

#### Intervalli Illimitati Superiormente
>Un intervallo è *Illimitato Superiormente* se:
>- Ha come estremo sinistro $c\in\mathbb{R}$ 
>- Ha come estremo destro $+\infty$

> [!example]+ <font color="orange">Esempio</font>
>![[Pasted image 20240925170350.png|500]]

#### Intervalli Illimitati Inferiormente
>Un intervallo è *Illimitato Inferiormente* se:
>- Ha come estremo sinistro $-\infty$
>- Ha come estremo destro $c\in\mathbb{R}$ 

> [!example]+ <font color="orange">Esempio</font>
>![[Pasted image 20240925165636.png|500]]

#### Intervalli Illimitati Inferiormente e Superiormente
>Un intervallo è *Illimitato Inferiormente e Superiormente* (o Illimitato) se
>- Ha come estremo sinistro $-\infty$
>- Ha come estremo destro $+\infty$ 

> [!example]+ <font color="orange">Esempio</font>
>![[Pasted image 20240925170456.png|500]]


## Intervalli Aperti e Chiusi
### Intervallo Aperto
>Gli **Intervalli Aperti** indicano che i valori agli estremi sono *esclusi*.
>Vengono rappresentati con le *parentesi tonde*.

> [!example]+ <font color="orange">Esempio</font>
>![[Pasted image 20240925171209.png|500]]

#### Intervallo Aperto a Sinistra
>Si dice che $I\subseteq \mathbb{R}$ è un *Intervallo Aperto a Sinistra* se l'estremo sinistro è un valore finito e se è escluso.

#### Intervallo Aperto a Destra
>Si dice che $I\subseteq \mathbb{R}$ è un *Intervallo Aperto a Destra* se l'estremo destro è un valore finito e se è escluso.


### Intervallo Chiuso
>Gli **Intervalli Chiusi** indicano che i valori agli estremi sono *inclusi*.
>Vengono rappresentati con le *parentesi quadrate*.

> [!example]+ <font color="orange">Esempio</font>
>![[Pasted image 20240925171156.png|500]]

#### Intervallo Chiuso a Sinistra
>Si dice che $I\subseteq \mathbb{R}$ è un *Intervallo Chiuso a Sinistra* se l'estremo sinistro è un valore finito e se è incluso.

#### Intervallo Chiuso a Destra
>Si dice che $I\subseteq \mathbb{R}$ è un *Intervallo Chiuso a Destra* se l'estremo destro è un valore finito e se è incluso.



___
[[000 Indice Analisi|↖ Ritorna all'indice ↖]]

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
- [Wikipedia](https://en.wikipedia.org/wiki/Interval_(mathematics))
- [YouMath](https://www.youmath.it/lezioni/analisi-matematica/premesse-per-lanalisi-infinitesimale/53-classificazione-degli-intervalli-reali.html)