---
aliases: 
tags:
  - corsi/informatica/programmazione_strutture_dati
paragrafo: Complessità Computazionale
cssclasses:
  - 
---
Negli algoritmi ricorsivi la soluzione di un problema si ottiene applicando lo stesso algoritmo ad uno o più sottoproblemi.

La complessità viene espressa nella forma di una relazione di ricorrenza.

Per la valutazione della complessità valutiamo:
- **Il lavoro di combinazione** (preparazione delle chiamate ricorsive e combinazione dei risultati ottenuti), che può essere costante, lineare, ecc.

- **La forma dell'equazione di ricorrenza**, che può essere con o senza partizione dei dati.

- Il numero di termini ricorsivi *da attivare* (chiamate ricorsive nella funzione).

Ma soprattutto si specificano il [[028 Ricorsione di Funzioni|passo base ed il passo ricorsivo]] della funzione.

Si utilizza la seguente tabella:
![[Pasted image 20220503111751.png|1100]]

<font color="orange">Esempio</font>: Fibonacci.
```C
int fib(int n){
	if (n < 2)
		return n;
	else
		return fib(n-1) + fib(n-2);
}
```
Passo Base: $T(0) = T(1) = c$.
Passo Ricorsivo: $T(n) = T(n-1) + T(n-2) + b$.
Dove $b=0$ e $c$ significa che si è raggiunto il passo base.

Rientra nel caso 1.a: Lavoro di combinazione costante, senza partizione dei dati.

Dato che sono presenti 2 chiamate ricorsive attive (cioè che vengono eseguite) $T(n)$ è *Esponenziale con n*.

<font color="orange">Esempio</font>: Fattoriale.
```C
int fact(int n){
	if (n == 0)
		return 1;
	else
		return fact(n-1) * n;
}
```
Passo Base: $T(0) = T(1) = c$.
Passo Ricorsivo: $T(n) = T(n-1) + b$.

Rientra nel caso 1.a: Lavoro di combinazione costante, senza partizione dei dati.

È presente una sola chiamata ricorsiva attiva, quindi $T(n)$ è *Lineare con n*.

<font color="orange">Esempio</font>: Ricerca Binaria Ricorsiva.

Passo Base: $T(1) = c$.
Passo Ricorsivo: $T(n) = T(n/2) + b$.
Dove $b=0$ e $c$ significa che si è raggiunto il passo base.

Si ha $T(n/2)$ perché si va a dimezzare la dimensione dell'array ad ogni chiamata ricorsiva.

Rientra nel caso 1.b: Lavoro di combinazione costante, con partizione dei dati.

È presente una sola chiamata ricorsiva attiva ($a=1$), quindi $T(n)$ è *Logaritmico con n*.

___
[[000 Indice PSD|↖ Ritorna all'indice ↖]]

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