---
aliases: 
tags:
  - corsi/informatica/architettura_degli_elaboratori
paragrafo: Rappresentazione Numeri - Con Virgola
cssclasses:
  - 
---
> [!note] Con virgola

Per poter rappresentare numeri reali molto piccoli o molto grandi in maniera compatta, si utilizza la notazione scientifica in base 10.

È un modo conciso di esprimere i numeri, utilizzando *potenze intere* (positive o negative) di 10.

## NOTAZIONE SCIENTIFICA NORMALIZZATA

Una notazione scientifica si dice **normalizzata** se a sinistra della virgola non ci sono zeri e c'è una sola cifra.

> [!example]- <font color="orange">Esempio</font>
**1**,0<sub>10</sub> $\times$ 10<sup>-9</sup> è normalizzata
**0**,1<sub>10</sub> $\times$ 10<sup>-8</sup> NON è normalizzata

Anche i *numeri binari* possono essere espressi in notazione scientifica normalizzata. La loro forma in notazione scientifica normalizzata è:
**1,xxx<sub>2</sub> $\times$ 2<sup>yyy</sup>**


### SPOSTARE LA VIRGOLA
Spostare la virgola in un numero comporta l'aumento o la diminuzione dell'esponente del 10.

Se si sposta a *sinistra* di n cifre decimali, si *incrementa* l'esponente del 10 di n.
Se si sposta a **destra** di n cifre decimali, si **decrementa** l'esponente del 10 di n.

Funziona anche per i numeri *in binario*.

> [!example]- <font color="orange">Esempio</font>
*12345*,**67890**<sub>10</sub> $\times$ 10<sup>0</sup> $\quad\quad\quad\quad\quad\quad\quad$ *12345*,**67890**<sub>10</sub> $\times$ 10<sup>0</sup>
*1234*,**567890**<sub>10</sub> $\times$ 10<sup>1</sup> $\quad\quad\quad\quad\quad\quad\quad$ *123456*,**7890**<sub>10</sub> $\times$ 10<sup>-1</sup>
*123*,**4567890**<sub>10</sub> $\times$ 10<sup>2</sup> $\quad\quad\quad\quad\quad\quad\quad$ *1234567*,**890**<sub>10</sub> $\times$ 10<sup>-2</sup>
*12*,**34567890**<sub>10</sub> $\times$ 10<sup>3</sup> $\quad\quad\quad\quad\quad\quad\quad$ *12345678*,**90**<sub>10</sub> $\times$ 10<sup>-3</sup>
*1*,**234567890**<sub>10</sub> $\times$ 10<sup>4</sup> (←Normalizzato)$\quad$ *123456789*,**0**<sub>10</sub> $\times$ 10<sup>-4</sup>

___
[[000 Indice AdE|↖ Ritorna all'indice ↖]]

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