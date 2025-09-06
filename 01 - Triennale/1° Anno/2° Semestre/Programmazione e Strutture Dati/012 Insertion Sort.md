---
aliases: 
tags:
  - corsi/informatica/programmazione_strutture_dati
paragrafo: Algoritmi di Ordinamento
cssclasses:
  - 
---
L'algoritmo effettua una visita totale: ad ogni passo gli elementi che precedono l'elemento corrente sono ordinati.

1. Il primo elemento si salta;
2. Si prende in considerazione l'elemento della casella corrente (`val`);
3. Si compara `val` con l'elemento della casella precedente;
4. Se l'elemento della casella precedente è maggiore di `val`, copia gli elementi più grandi di una posizione verso destra per fare spazio.
	- Altrimenti continua fino a quando `val` è maggiore dell'elemento della cella precedente. 
5. Posiziona `val`.

<font color="orange">Esempio</font>: $i=3$
![[Pasted image 20220309134431.png]]

*18* si confronta con l'ultimo elemento ordinato, 30. 
*18* è minore di 30, allora 30 avanza di una posizione.
![[Pasted image 20220309134541.png]]

*18* si confronta con il penultimo elemento ordinato, 20.
*18* è minore di 20, allora il 20 avanza di una posizione.
![[Pasted image 20220309134716.png]]

*18* si confronta con il primo elemento ordinato, 10. 
*18* non è minore di 10, inseriamo *18* alla posizione dopo il 10.
![[Pasted image 20220309134819.png]]

## PROGETTAZIONE E CODICE
### INSERTION SORT
#### PROGETTAZIONE
- `for (current = 1; current < n; current++)`
	1. Prendi il valore della cella corrente (`current`) e inseriscilo in `val`.
	2. `previous` è il numero della cella prima di `current`.
	- `while (previous >= 0 && val < a[previous])`
		1. Copia il valore di `previous` nella cella a destra di `previous`.
		2. `previous--`.
	3.  La cella a destra di `previous` avrà il valore di `val`.

#### ANALISI
1. La funzione prenderà in input: `a[]`, `n`.
	- [[003 Fasi dello Sviluppo del Programma#^bda15e|Precondizione]]: `n` $> 0$.
	- [[003 Fasi dello Sviluppo del Programma#^23536b|Postcondizione]]: Gli elementi di `a[]` devono essere ordinati in modo crescente con l'algoritmo del Insertion Sort.

[[003 Fasi dello Sviluppo del Programma#DIZIONARIO DEI DATI|Dizionario dei dati]].

| Ident.   | Tipo   | Desc.                                      |
| -------- | ------ | ------------------------------------------ |
| a        | array  | Array da ordinare                          |
| n        | intero | Dimensione dell'array a                    |
| current  | intero | Indice corrente dell'array                 |
| previous | intero | Indice cella precedente di quella corrente |
| val      | intero | Valore preso in considerazione             | 

```C
//Per ogni elemento dell'array, a partire dal primo, vede se questo è minore dei precedenti.

void insertionSort(int a[], int n){
	int current, val, previous;
	for (current = 1; current < n; current++){
		val = a[current];
		previous = current - 1;
			while (previous >= 0 && val < a[previous]){
				a[previous + 1] = a[previous];
				previous--;
			}
		a[previous + 1] = val;
	}
}
```

Tipica chiamata alla funzione: `insertionSort(a, n)`.

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