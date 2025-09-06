---
aliases: 
  - Tempo Polinomiale
  - Tempo Esponenziale
tags:
  - corsi/informatica/elementi_teoria_computazione
paragrafo: Complessità di Tempo Polinomiale
cssclasses:
  - 
---
>I **polinomi** hanno la forma $$\color{#CC241D}n^k, k\geq 1$$
>Mentre gli **esponenziali** hanno la forma $$\color{#61AFEF}{2}^{n^k}, k\geq 1$$

^0e8d25



Le [[053 Relazioni di complessità tra modelli#^3386b3|differenze polinomiali nel tempo di esecuzione]] sono considerate "*piccole*", mentre quelle [[053 Relazioni di complessità tra modelli#^76c5ec|esponenziali]] sono considerate "*grandi*".

Per capirne il perché, notiamo la differenza drastica tra il tasso di crescita di polinomi ed esponenziali che si incontrano di solito, ad esempio $n^3$ e $2^n$.

![[Pasted image 20240529152846.png|500]]

Sia $n=1000$ , allora: 
- $n^3=1.000.000$, un numero grande ma gestibile
- $2^n$ sarà un numero altamente ingestibile

Quindi, gli algoritmi aventi **tempo polinomiale** $TIME(n^k), k\geq1$ sono abbastanza veloci per molti scopi, al contrario di quelli esponenziali (usati molto spesso in algoritmi di [[003 Crittografia#^9a60da|Brute Force]].

Tutti i *modelli computazionali deterministici* "ragionevoli" sono **polinomialmente equivalenti**. Ovvero, uno di essi può *simularne* un altro con aumento solo polinomiale del tempo di esecuzione.

> [!example]- <font color="orange">Esempio</font>
>Se una macchina di Turing multinastro risolve un problema in tempo $TIME(n)$, una macchina di Turing a nastro singolo può simularla in tempo $P(TIME(n))$, dove $P$ è un polinomio.



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