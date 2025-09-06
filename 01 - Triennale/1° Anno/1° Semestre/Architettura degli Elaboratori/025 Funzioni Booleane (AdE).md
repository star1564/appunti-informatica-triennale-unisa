---
aliases:
  - Leggi di De Morgan (AdE)
  - Porte Logiche
  - Operatori Booleani
  - Letterale
  - Clausola
cssclasses:
  - 
tags:
  - corsi/informatica/architettura_degli_elaboratori
paragrafo: Componenti Logici
---

>Una *funzione booleana* a $n$ variabili è una funzione: $$f: \{0, 1\}^n \to \{0, 1\}$$
>Ovvero che *ogni $n$ variazione di bit*, composti da 0 e 1, *ha almeno una corrispondenza* nell'insieme {0, 1}.

Queste funzioni sono realizzate da semplici dispositivi fisici elettronici detti **porte logiche**, costituiti da circuiti elettronici e transistor.

La combinazione di queste daranno funzioni più complesse.


## ESPRESSIONI BOOLEANE

![[Formule Booleane|Espressioni Booleane]]

### PORTE LOGICHE (Operatori Booleani)
Gli **Operatori Booleani** più importanti sono:
- [[026 AND (AdE)|AND]]
- [[027 OR (AdE)|OR]]
- [[028 NOT (AdE)|NOT]]
- [[029 XOR (AdE)|XOR]]
- [[030 NAND|NAND (AND Negato)]]
- [[031 NOR|NOR (OR Negato)]]

### Letterale
Un **Letterale** è *una variabile booleana vera o negata* all'interno di una espressione booleana. 

> [!example]+ <font color="orange">Esempio</font>
>$\phi=(\overline{x}\land y)\lor (x\land \overline{z})$ è un'espressione booleana con 4 letterali e 3 variabili $x,y,z$.

### Clausola
Una **Clausola** è un OR di letterali.

> [!example]+ <font color="orange">Esempio</font>
>$$(\overline{x}\lor x\lor y\lor z)$$

### Forme
- [[035 Forme SOP|Forme SOP]]
- [[036 Forme POS|Forme POS]]



## PROPRIETÀ DELLE ESPRESSIONI BOOLEANE

Le due operazioni binarie *AND* e *OR* godono della proprietà:
- *Commutativa*;
$x\times y = y\times x,\quad x + y = y + x$
- *Associativa*;
$(x\times y)\times z = x\times (y\times z)$
- *Idempotenza*;
$x\times x = x,\quad x + x = x$ ^e30afa
- *Assorbimento*;
$x\times (x + y) = x,\quad x + (x\times y) = x$
$x\times (\overline{x} + y) = x\times y,\quad x + (\overline{x}\times y) = x + y$
- *Distributiva*.
$x\times (y + z) = x\times y + x\times z$ ^7cebf1

L'operazione unaria *NOT*, gode della proprietà:
- *Involuzione*;
$\overline{\overline{x}}=x$
- *Complemento*;
$x\times \overline{x} = 0,\quad x + \overline{x} = 1$ ^fbfeb7

### Leggi di De Morgan per l'Algebra Booleana

![[Leggi De Morgan]]

- $\overline{x\times y} = \overline{x}+\overline{y},\quad \overline{x + y} = \overline{x}\times\overline{y}$

Dove:
- $\times \iff \land \iff AND$; 
- $+ \iff \lor \iff OR$.


## PRINCIPIO DI DUALITÀ
Data una espressione booleana valida, se ne ottiene un'altra valida (duale della precedente) scambiando *0* con *1* e *AND* con *OR*.

> [!example]- <font color="orange">Esempio</font>
$(x+1)\cdot(y+0)\iff (x\cdot 0)+(y\cdot 1)$

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