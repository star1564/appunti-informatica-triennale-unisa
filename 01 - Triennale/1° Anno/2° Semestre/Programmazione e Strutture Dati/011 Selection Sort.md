---
aliases: 
tags:
  - corsi/informatica/programmazione_strutture_dati
paragrafo: Algoritmi di Ordinamento
cssclasses:
  - 
---
Ordina un array trovando ripetutamente l'elemento minimo della parte non ordinata dell'array e spostandolo all'inizio.

Vengono visitati tutti gli elementi dell'array in sequenza.

Per ogni posizione visitata, individua l'elemento che dovrebbe occupare quella posizione.

In questo modo, se $i$ è la posizione corrente $(0 \leq i < n)$, tutti gli elementi nelle posizioni comprese tra $0$ ed $i-1$ rispettano l'ordinamento, rientrando nella parte ordinata.

L'elemento che deve occupare la posizione $i$ sarà il *minimo* tra quelli nelle posizioni comprese tra $i$ ed $n-1$, ovvero la parte non ordinata.

<font color="orange">Esempio</font>: $i=3$
![[Pasted image 20220303112043.png]]

## PROGETTAZIONE E CODICE
### NECESSARI
Avremo bisogno di due funzioni:
- [[1002.F Swap (int)|Swap (int)]];
- [[1003.F Min Pos Array|Min Pos Array]].

### SELECTION SORT
#### PROGETTAZIONE
- `for (i = 0; i < n-1; i++)`
	1. Individua la posizione dell'elemento minimo compreso tra le posizioni `i` e `n-1` dell'array `a`
	`minPosArray(int a[],int n)`

	2. Scambia gli elementi di `a` con posizione `a[posizione_elemento_minimo]` e `a[i]`
	`swap(int *a, int *b)`

#### ANALISI
1. La funzione prenderà in input: `a[]`, `n`.
	- [[003 Fasi dello Sviluppo del Programma#^bda15e|Precondizione]]: `n` $> 0$.
	- [[003 Fasi dello Sviluppo del Programma#^23536b|Postcondizione]]: Gli elementi di `a[]` devono essere ordinati in modo crescente con l'algoritmo del Selection Sort.

[[003 Fasi dello Sviluppo del Programma#DIZIONARIO DEI DATI|Dizionario dei dati]].

| Ident. | Tipo   | Desc.                                |
| ------ | ------ | ------------------------------------ |
| a      | array  | Array da ordinare                    |
| n      | intero | Dimensione dell'array a              |
| min    | intero | Risultato della funzione minPosArray |
| i      | intero | Posizione corrente nel ciclo for     | 

Bisogna fare attenzione quando si cerca la posizione con l'elemento più piccolo. Infatti la funzione `minPosArray` ritorna la posizione partendo dal sottoarray, quindi **bisogna riallineare il risultato** all'array originale aggiungendo `+ i`.

```C
//Ordina un array trovando ripetutamente l'elemento minimo della parte non ordinata dell'array e spostandolo all'inizio

void selectionSort(int a[], int n){
	int i, min;
	for (i = 0; i < n-1; i++){
		min = minPosArray(a+i, n-i) + i;
		swap(&a[i], &a[min]);
	}
}
```

Tipica chiamata alla funzione: `selectionSort(a, n)`.

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