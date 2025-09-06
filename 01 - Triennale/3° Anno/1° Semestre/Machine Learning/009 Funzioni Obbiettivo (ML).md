---
aliases: 
- Loss Function
- Funzione Obbiettivo
- Funzione di Loss
tags:
  - corsi/informatica/machine_learning
paragrafo: Design di Sistemi ML
cssclasses:
  - 
---
>Un modello di ML richiede una **Funzione Obiettivo** (*funzione di loss*) per guidare il processo di apprendimento. Questa funzione aiuta il modello a *minimizzare gli errori* nelle sue previsioni durante l'apprendimento [[002 Apprendimento con Supervisione Umana#Supervisionato|automatico supervisionato]]. 

Le funzioni di perdita più comuni a questo scopo sono le seguenti.

- **RMSE** (root mean square error) o **MAE** (mean absolute error) per la [[008 Task di Regressione|Regressione]]; 
- **Logistic Loss** (o log loss) per [[007 Task di Classificazione#Classificazione Binaria|Classificazione Binaria]];
- **Cross Entropy** per [[007 Task di Classificazione#Classificazione Multi-classe|Classificazione Multi-classe]], la cui formula è la seguente: 

$$Loss(p,q)=-\displaystyle\sum_{i=1}^{n} (p_{i}\cdot \log(q_{i}) + (1-p_{i}) \cdot \log(1-q_{i}))$$

> [!tip]- Cross Entropy in Python
>```Python
>import numpy as np
>
>def cross_entropy(p, q):
>	return -sum([p[i] * np.log(q[i]) for i in range(len(p))])
>
>p = [0, 0, 0, 1]
>q = [0.45, 0.2, 0.02, 0.33]
>cross_entropy(p, q)
>```

> [!example]- <font color="orange">Esempio</font>
>Si consideri un task di classificazione per stabilire il topic di un articolo tra tecnologia, intrattenimento, finanza e politica. Supponiamo che per un articolo di politica (quindi con label $[0, 0, 0, 1]$) il modello restituisca $[0.45, 0.2, 0.02, 0.33]$.
>
>- Per le prime tre classi (dove $y = 0$):
>	1. $0 \cdot \log(0,45) + (1-0) \cdot \log(1-0,45)=0+0,79851=0,79851$
>	2. $0 \cdot \log(0,2) + (1-0) \cdot \log(1-0,2)=0+0,22314=0,22314$
>	3. $0 \cdot \log(0,02) + (1-0) \cdot \log(1-0,02)=0+0,01980=0,01980$
>- Per la quarta classe (dove $y = 1$):
>	4. $0 \cdot \log(0,33) + (1-1) \cdot \log(1-0,33)=-0,41504+0=\color{#61AFEF}-0,41504$
>
>Si ottiene la Cross Entropy sommando i valori ottenuti: $0,79851 + 0,22314 + 0,01980 - 0,41504 ≈ \color{#CC241D}0,6264$


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