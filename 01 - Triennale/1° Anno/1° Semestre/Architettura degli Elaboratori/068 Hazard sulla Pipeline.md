---
aliases: 
tags:
  - corsi/informatica/architettura_degli_elaboratori
paragrafo: MIPS - Processore
cssclasses:
  - 
---
In una pipeline si possono verificare situazioni in cui *l'istruzione successiva non può essere eseguita nel ciclo di clock successivo*. Tali situazioni sono dette **Hazard** e possono essere di tre tipi:
- *Hazard strutturali*, quando stadi diversi della pipeline necessitano delle stesse risorse hardware nello stesso ciclo di clock;
- *Hazard sui dati*, quando un'istruzione dipende dal risultato di un istruzione precedente che non è stata ancora completata;
- *Hazard sul controllo*, quando è necessario prendere una decisione sulla prossima istruzione da eseguire prima che la condizione sia valutata.

## HAZARD STRUTTURALI

### CAUSE

Nel caso del MIPS può accadere se: 
1. La *memoria istruzioni* e la *memoria dati* non sono separate; 
2. Il *banco dei registri* è usato nello stesso ciclo di clock per un *accesso in lettura* da un'istruzione e uno *in scrittura* da un'altra istruzione.

### SOLUZIONI
Supponendo che la memoria istruzioni e la memoria dati *siano separate*. 
Per risolvere il secondo problema la **scrittura** nel banco dei registri avviene nella prima metà del ciclo di clock; la **lettura** nella seconda.

![[Pasted image 20211213105518.png]]

## HAZARD SUI DATI

### CAUSE

> [!example]+ <font color="orange">Esempio</font>
>```
>add $2, $1, $3
>sub $12, $2, $5
>```
>Uno degli operandi sorgente di `sub` (**$2**) è prodotto da una `add`, che è ancora nella pipeline.
>![[Pasted image 20211213105752.png|500]]

### SOLUZIONI
Esistono due soluzioni per gestire gli hazard sui dati:
- *Soluzioni di tipo hardware*:
	- [[069 Propagazione|Propagazione]];
	- Inserimento di stalli (o bolle) nella pipeline.
- *Soluzioni di tipo software*:
	- Inserimento di istruzioni `nop` (no operation);
	- Riordinamento delle istruzioni.


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