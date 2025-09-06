---
aliases: 
tags:
  - corsi/informatica/programmazione_strutture_dati
paragrafo: Algoritmi di Ordinamento
cssclasses:
  - 
---
Scambia ripetutamente gli elementi adiacenti se si trovano nell'ordine sbagliato.

Si effettuano $n-1$ visite dell'array, alla vista i-esima, si confrontano elementi adiacenti dal primo al ($n-i$)-esimo elemento. Elementi adiacenti che non risultano ordinati vengono scambiati.

Ad ogni passo l'elemento più grande viene portato nella sua posizione finale.

Infatti, dopo il passo i-esimo, gli elementi tra le posizioni `n-i` ed `n-1` risultano ordinati e nelle loro posizioni finali.

È possibile creare anche una versione adattiva dell'algoritmo. Vede dopo una iterata se sono stati effettuati scambi. Se non sono stati effettuati scambi allora l'array è già ordinato.

<font color="orange">Esempio</font>:
- Si confronta 10 con 20, dato che è già ordinato, si lascia così e si passa avanti.
- Si confronta 20 con 7, dato che 7 < 20, si scambiano di posto.
- Si confronta 20 con 18, dato che 18 < 20, si scambiano di posto.
- Si confronta 20 con 6, dato che 6 < 20, si scambiano di posto.
- Si confronta 20 con 4, dato che 4 < 20, si scambiano di posto.
- Troviamo come ultimo elemento dell'array il numero più grande, 20.
![[Pasted image 20220309141628.png|300]]

- Si ritorna al primo elemento.
- Si confronta 10 con 7, dato che 7 < 10, si scambiano di posto.
- Si confronta 10 con 18, dato che è già ordinato, si lascia così e si passa avanti.
- Si confronta 18 con 6, dato che 6 < 18, si scambiano di posto.
- Si confronta 18 con 4, dato che 4 < 18, si scambiano di posto.
- Si confronta 18 con 20, dato che è già ordinato, si lascia così e si conclude.
- Troviamo come penultimo elemento dell'array il penultimo numero più grande, 18.

L'operazione si ripete per tutti gli elementi in rosso.
![[Pasted image 20220309141824.png|300]]

## PROGETTAZIONE E CODICE
### NECESSARI
Avremo bisogno di una funzione:
- [[1002.F Swap (int)|Swap (int)]].

### BUBBLE SORT
#### PROGETTAZIONE (ADATTIVA)
- Si pone `ordinato` = 0.
- `for (i = 1; i < n && !ordinato; i++)`
	1. Si pone `ordinato` = 1.
	2. `for (j = 0; j < n-i; j++)`
		1. Se l'elemento corrente è maggiore del prossimo elemento, scambia.
		2. Si pone `ordinato` = 0.

#### ANALISI
1. La funzione prenderà in input: `a[]`, `n`.
	- [[003 Fasi dello Sviluppo del Programma#^bda15e|Precondizione]]: `n` $> 0$.
	- [[003 Fasi dello Sviluppo del Programma#^23536b|Postcondizione]]: Gli elementi di `a[]` devono essere ordinati in modo crescente con l'algoritmo del Bubble Sort.

[[003 Fasi dello Sviluppo del Programma#DIZIONARIO DEI DATI|Dizionario dei dati]].

| Ident.   | Tipo   | Desc.                                          |
| -------- | ------ | ---------------------------------------------- |
| a        | array  | Array da ordinare                              |
| n        | intero | Dimensione dell'array a                        |
| i        | intero | Indice della posizione presa in considerazione |
| j        | intero | Indice usato per i confronti                   |
| ordinato | intero | Valore che indica se l'array è già ordinato    | 

##### VERSIONE SEMPLICE
```C
//Scambia ripetutamente gli elementi adiacenti se si trovano nell'ordine sbagliato.

void bubbleSort(int a[], int n){
	int i, j;
	for (i = 1; i < n; i++)
		for (j = 0; j < n-i; j++) //Elementi non ordinati
			if (a[j]>a[j+1])
				swap(&a[j], &a[j+1]);
}
```

##### VERSIONE ADATTIVA
```C
//Scambia ripetutamente gli elementi adiacenti se si trovano nell'ordine sbagliato.  Versione Adattiva.

void adaptiveBubbleSort(int a[], int n){
	int i, j, ordinato = 0;
	for (i = 1; i < n && !ordinato; i++){
		ordinato = 1;
		for (j = 0; j < n-i; j++) //Elementi non ordinati
			if (a[j]>a[j+1]){
				ordinato = 0;
				swap(&a[j], &a[j+1]);
			}
	}
}
```

Tipica chiamata alla funzione: `bubbleSort(a, n)`, `adaptiveBubbleSort(a, n)`.

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