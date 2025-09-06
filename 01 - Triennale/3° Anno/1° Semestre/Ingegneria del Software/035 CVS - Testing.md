---
aliases:
- Testing
- Componente di Testing
- Affidabilità (Testing)
- Affidabilità del Software (Testing)
- Fallimento, Failure (Testing)
- Stato Erroneo, Errore (Testing)
- Difetto, Fault, Bug (Testing)
tags:
  - corsi/informatica/ingegneria_software
paragrafo: Testing
cssclasses:
  - 
---
>La quinta fase del [[003 Ciclo di Vita del Software|CVS]], il **Testing**, verifica e convalida il codice scritto durante l'[[029 CVS - Implementazione|Implementazione]] e tutti gli altri documenti.

![[Pasted image 20240121105726.png|900]]

Il Testing è il processo che *cerca le differenze tra i comportamenti attesi* dalle specifiche *e quelli osservati* nel sistema implementato. Quindi ha lo scopo di mostrare che l'implementazione del sistema sia inconsistente con i vari modelli.

L'obbiettivo principale è quello di *progettare dei test* per provare il sistema e rilevare possibili problemi.

Questa fase dovrebbe essere realizzata da sviluppatori che non sono stati coinvolti nelle attività di costruzione del sistema, questo perché il testing cerca di *rompere il sistema*.

## Terminologie di Base

>Un **Test** può avere due esiti:
>- Ha *successo* se rileva uno o più malfunzionamenti del sistema.
>- *Fallisce* se non riesce a rilevare alcun malfunzionamento.

^f152ed

- **Componente di Testing**: una parte isolata del sistema da testare.
 ^ef6c59
- **Affidabilità**: è una misura del *successo* con cui il comportamento osservato di un sistema *è conforme alle specifiche* del suo comportamento.

- **Affidabilità del Software**: è la probabilità che un sistema software non causi guasti per un periodo di tempo specificato in condizioni specifiche.
 ^4fc61b
- **Fallimento** (*Failure*): è una qualsiasi *deviazione del comportamento osservato* rispetto a quello specificato.
 ^bb2926
- **Stato Erroneo** (*Errore*): il sistema si trova in uno stato tale che un'ulteriore elaborazione da parte del sistema *porterà a un guasto*, il che fa sì che il sistema si allontani dal comportamento previsto.
 ^733a0c
- **Difetto** (*Fault*, *Bug*): è la *causa meccanica o algoritmica* di uno stato erroneo. ^1cc62a

___
[[000 Indice IS|↖ Ritorna all'indice ↖]]

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
