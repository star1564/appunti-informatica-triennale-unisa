---
aliases: 
  - Problema decidibile
  - Problema semi-decidibile
tags:
  - corsi/informatica/elementi_teoria_computazione
paragrafo: Problemi di decisione
cssclasses:
  - 
---
Mentre l'[[033 Istanze di un Problema di Decisione#Insieme delle istanze|insieme delle istanze di un problema di decisione]] si divide in due sottoinsiemi, l'insieme delle stringhe su $\Sigma$ si divide in tre sottoinsiemi:
1. L'insieme delle stringhe $w$ che codificano istanze con *`Vero`*
2. L'insieme delle stringhe $w$ che codificano istanze con *`Falso`*
3. L'insieme delle stringhe $w$ che *non sono codifiche di istanze*

>Il linguaggio $L$ **associato** ad un [[001 Logica Proposizionale#^proposizione|Problema di Decisione]] $P$ è il *[[001 Linguaggio|linguaggio]] delle [[036 Codifiche|codifiche]] delle istanze che hanno come risposta `Vero`*.

- Se *esiste* una Macchina di Turing che *[[024 Macchine Decisionali#Linguaggio Deciso|decide]]* $L$, il problema viene detto **decidibile**.  ^3c83c4
- Se *NON esiste* una Macchina di Turing che decide $L$, il problema viene detto **[[042 Linguaggi Indecidibili#Definizione Problema Indecidibile|indecidibile]]**.
- Se esiste una Macchina di Turing che *[[023 Linguaggio Turing-Riconoscibile|riconosce]]* $L$, il problema viene detto **semi-decidibile**. 

In questo modo esprimiamo un problema computazionale come un problema di riconoscimento di un linguaggio. La macchina *corrisponde ad un algoritmo per il problema*.

> [!example]- <font color="orange">Esempio</font>
>Il linguaggio associato al problema $CONNESSO$ (verificare se un grafo è connesso) è il seguente $$A=\{\langle G \rangle\ |\ G \text{ è un grafo connesso} \}$$
>Dove $\langle G \rangle$ denota una codifica di $G$ mediante una stringa su un alfabeto $\Sigma$.
>
>Risolvere $CONNESSO$ *equivale a trovare una Macchina di Turing che decide $A$*.


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