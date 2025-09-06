---
aliases: 
tags:
  - corsi/informatica/basi_di_dati
paragrafo: Modello Relazionale
cssclasses:
  - 
---
Una relazione è un insieme di tuple, quindi possiamo applicare le classiche *operazioni insiemistiche*. Il risultato della combinazione di due relazioni per mezzo di un operazione su insieme è una nuova relazione.

Per poter applicare un'operazione insiemistica a due relazioni, queste devono avere la stessa struttura, ovvero essere **Compatibili all'Unione**. Ciò significa che le due relazioni hanno lo *stesso numero di attributi* e che *ogni coppia di attributi corrispondenti ha lo stesso dominio*.

> [!example]- <font color="orange">Esempio di tutte le operazioni</font>
![[Pasted image 20221213155341.png]]

---
### UNIONE
Dati due insiemi $S$ ed $R$, il risultato di questa operazione è la relazione che include tutte le tuple che sono in $R$ o in $S$, oppure in $R$ ed $S$. Le tuple duplicate sono eliminate.
$$R\cup S$$

> [!example]- <font color="orange">Esempio specifico unione</font>
>Trovare il SSN di tutti gli impiegati che lavorano nel dipartimento 4 oppure che supervisionano un impiegato che lavora nel dipartimento 4.
>![[Pasted image 20221213154935.png]]

### INTERSEZIONE
Dati due insiemi $S$ ed $R$, il risultato di questa operazione è la relazione che include tutte le tuple che sono in sia in $R$ che in $S$.
$$R\cap S$$

### DIFFERENZA
Dati due insiemi $S$ ed $R$, il risultato di questa operazione è la relazione che include tutte le tuple che sono in in $R$ ma non in $S$.
$$R - S$$

### PRODOTTO CARTESIANO
> [!example]- <font color="orange">Esempio</font>
> ![[Pasted image 20221213155619.png]]

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