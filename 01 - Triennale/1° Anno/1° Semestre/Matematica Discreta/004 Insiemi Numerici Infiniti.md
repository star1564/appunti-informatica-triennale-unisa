---
aliases:
  - Insiemi Numerici Infiniti
tags:
  - corsi/matematica/matematica_discreta
paragrafo: Teoria degli Insiemi
cssclasses:
  - 
---
# %%Definizione%%
> Gli **Insiemi Numerici Infiniti** sono [[001 Insieme|Insiemi]] di numeri che contengono una quantità infinita di elementi. 

Non importa quanto grande sia un numero che appartiene a un insieme numerico infinito, c'è sempre un numero ancora più grande all'interno dell'insieme.

Ci sono vari tipi di insiemi numerici infiniti, dove *ognuno ha i suoi tipi di numeri*:

| Nome                                 | Simbolo                       | Descrizione                                                                                     | Esempio                                                                |
| ------------------------------------ | ----------------------------- | ----------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------- |
| **Numeri Naturali compreso lo zero** | $\color{#61AFEF}\mathbb{N_0}$ | Include tutti i numeri interi a partire dallo zero                                              | $\{0, 1, 2, 3, 4, \dots\}$                                             |
| **Numeri Naturali**                  | $\color{#61AFEF}\mathbb{N}$   | Include tutti i numeri interi positivi                                                          | $\{1, 2, 3, 4, \dots\}$                                                |
| **Numeri Interi**                    | $\color{#61AFEF}\mathbb{Z}$   | Include tutti i numeri interi positivi e negativi insieme allo zero                             | $\{\dots,-2,-1,0,1, 2,\dots\}$                                         |
| **Numeri Razionali**                 | $\color{#61AFEF}\mathbb{Q}$   | Include tutti i numeri che possono essere scritti come frazioni (rapporto di due numeri interi) | $\{\dots, -\frac{3}{4},\frac{1}{2}, \frac{7}{1} \}$                    |
| **Numeri Irrazionali**               | $\color{#61AFEF}\mathbb{I}$   | Include tutti i numeri che possono essere scritti come radici                                   | $\left\{ \sqrt{2}, \sqrt[3]{5}, \sqrt[5]{\frac{3}{2}}, \dots \right\}$ |
| **Numeri Reali** [^1]                | $\color{#61AFEF}\mathbb{R}$   | Include tutti i numeri razionali e irrazionali, ovvero $\mathbb{R}=\mathbb{Q}\cup \mathbb{I}$   | $\{\dots, -\frac{3}{4},1, \sqrt{2} \}$                                 |
| **Numeri Complessi** [^2]            | $\color{#61AFEF}\mathbb{C}$   | Include tutti i numeri reali e immaginari                                                       | $\{real, immaginary \}$                                                |

^6c50e3

[^1]: [[003 Insieme dei numeri Reali|Insieme dei numeri Reali (ANALISI)]]
[^2]: [[022 Numeri Complessi|Numeri Complessi (ANALISI)]]

$$\mathbb{N}\subset\mathbb{N_0}\subset\mathbb{Z}\subset\mathbb{Q}\subset\mathbb{R}\subset \mathbb{C}$$

![[Pasted image 20240925153125.png]]

# %%FinePagina%%
___
[[000 Indice MD|↖ Ritorna all'indice ↖]]

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

- [YouMath](https://www.youmath.it/lezioni/algebra-elementare/lezioni-di-algebra-e-aritmetica-per-scuole-medie/2244-cosa-sono-i-numeri-irrazionali.html)