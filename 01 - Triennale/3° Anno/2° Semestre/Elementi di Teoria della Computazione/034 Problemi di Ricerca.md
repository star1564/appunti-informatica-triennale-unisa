---
aliases: 
tags:
  - corsi/informatica/elementi_teoria_computazione
paragrafo: Problemi di decisione
cssclasses:
  - 
---
>I **Problemi di Ricerca** sono quelli per i quali *si cerca una specifica soluzione*, se questa esiste.

Possiamo usare un *[[001 Introduzione a PA#^Algoritmo|algoritmo]] corrispondente* a un [[001 Logica Proposizionale#^proposizione|Problema di Decisione]] come *sottoprogramma* per un problema di ricerca, se tale algoritmo *esiste*. Se non esiste, né il problema di ricerca né quello di decisione sono algoritmicamente risolvibili.

> [!example]+ <font color="orange">Esempio</font>
>Data una formula booleana $\phi$, vogliamo fornire, se esiste, un assegnamento di valori di verità che soddisfa $\phi$. Disponiamo di un algoritmo `Al` che può risolvere questo problema.
>
>- Si usa `Al` per stabilire se $\phi$ è soddisfacibile
>- Se $\phi$ *non è soddisfacibile* l'assegnamento non esiste
>- Se $\phi$ *è soddisfacibile*:
>	1. Si assegna a una delle variabili $x$ il valore `1`
>	2. Si esegue `Al` sulla nuova formula ottenuta
>	3. Se tale formula non è soddisfacibile, si assegna ad $x$ il valore `0`. La formula ottenuta sarà ora soddisfacibile, perché se $\phi$ è soddisfacibile, uno dei due valori è quello giusto
>	4. Chiamiamo $\phi'$ la formula ottenuta da $\phi$ assegnando in essa ad $x$ il "giusto" valore. Se in $\phi'$ vi sono ancora variabili, riapplichiamo la procedura dal passo 1 a $\phi'$





___
[[000 Indice ETC|↖ Ritorna all'indice ↖]]

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