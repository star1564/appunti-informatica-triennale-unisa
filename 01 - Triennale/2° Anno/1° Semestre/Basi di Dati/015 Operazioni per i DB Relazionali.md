---
aliases: 
tags:
  - corsi/informatica/basi_di_dati
paragrafo: Modello Relazionale
cssclasses:
  - 
---
È possibile *combinare* queste operazioni per ottenere dati specifici.

### SELECT
L'operazione **SELECT** (ossia di selezione) è usata per *selezionare* un [[002 Sottoinsiemi|Sottoinsieme]] di [[010 Modello Relazionale#^Righe|righe]] di una relazione che soddisfano una **condizione di selezione**.

Si può considerare tale operazione come un *filtro* che trattiene solo le tuple che soddisfano una condizione qualificante.

Questa operazione si indica con $\color{#CC241D}\sigma$ (sigma minuscolo), mentre la condizione di selezione si inserisce al suo pedice. Quindi abbiamo la seguente sintassi:
$$\color{#61AFEF}\sigma_{<condizione\ selezione>}(\text{<}Relazione\text{>})$$

> [!example]- <font color="orange">Esempio</font>
>Vogliamo selezionare le tuple di `IMPIEGATO` il cui dipartimento è 4 *oppure* quelle il cui stipendio supera i 30.000 dollari.
>![[Pasted image 20221213143223.png]]

La condizione di selezione è una [[025 Funzioni Booleane (AdE)|espressione booleana]] formata in questo modo:
$$\text<nome\ attributo\text>\ \text<op\ confronto\text>\ \text<valore\ costante\text>$$
oppure
$$\text<nome\ attributo\text>\ \text<op\ confronto\text>\ \text<nome\ attributo\text>$$
Gli operatori di confronto sono *unari* e possono essere gli operatori standard {$=, <, >, \neq, \leq, \geq$}, oppure degli operatori booleani {[[002 AND (MMI)|AND]], [[003 OR (MMI)|OR]], [[004 NOT (MMI)|NOT]]}.

> [!example]- <font color="orange">Esempio</font>
>Vogliamo selezionare le tuple di `IMPIEGATO` il cui dipartimento è 4 *e* quelle il cui stipendio supera i 30.000 dollari.
>![[Pasted image 20221213144309.png]]
>Ovviamente si può ricreare l'esempio di prima (oppure) inserendo OR invece di AND.

La condizione di selezione è valutata per ogni tuple individualmente: se è vara, la tuple viene inserita nella relazione risultante.

---
### PROJECT
L'operazione **PROJECT** (di proiezione), seleziona determinate [[010 Modello Relazionale#^Colonne|colonne]] della tabella e scarta le altre.
Se si è interessati solo a determinati attributi di una relazione, si può usare l'operazione PROJECT per *proiettare* la relazione solo su di loro.

Il formato generale dell'operazione PROJECT è:
$$\color{#61AFEF}\pi_{\text<lista\ attributi\text>}(\text<Relazione\text>)$$
All'interno della lista attributi si inseriscono i nomi delle colonne da ottenere.

> [!example]- <font color="orange">Esempio</font>
>Vogliamo selezionare le seguenti colonne di `IMPIEGATO`: `Nome e Cognome`, `Numero Dipartimento`.
>![[Pasted image 20221213145642.png]]

___
[[000 Indice BD|↖ Ritorna all'indice ↖]]

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