---
aliases: 
tags:
  - corsi/informatica/progettazione_algoritmi
paragrafo: Alberi e Grafi
cssclasses:
  - 
---
>L'**Heap** è una struttura dati di tipo [[023 Alberi Binari|albero binario]] perfettamente bilanciato, tranne l'ultimo livello (anche se è riempito da sinistra a destra). Ci sono due tipi principali di heap:
>- L'*heap minimo*: il valore di ogni nodo è **maggiore o uguale** al valore dei suoi genitori.
>- L'*heap massimo*: il valore di ogni nodo è **minore o uguale** al valore dei suoi genitori.

Osserveremo principalmente l'heap minimo.

![[Pasted image 20230607164307.png|600]]

L'heap è spesso utilizzato per implementare altre strutture di dati, come [[017 Insiemi Dinamici#^37af8c|code di priorità o code con priorità]], in cui gli elementi con il valore più alto (nell'heap massimo) o più basso (nell'heap minimo) sono estratti in base alle loro priorità.

Gli heap possono essere memorizzati utilizzando in un array, livello per livello da sinistra a destra.

> [!example]- <font color="orange">Esempio</font>
> ![[Pasted image 20230607164638.png]]

Il nodo radice conterrà il *valore minimo dell'intero heap*, mentre ogni sottoalbero avrà il suo minimo nel nodo radice di quel sottoalbero.
Mentre l'altezza di uno heap con $n$ nodi è $\Theta(\log n)$.

## Operazioni
La complessità delle operazioni con l'heap è la seguente:

| *Operazione*                 | **Lista a Puntatori** | **Array Ordinato** | **Heap**          |
| ---------------------------- | --------------------- | ------------------ | ----------------- |
| `INSERT(S, x)`               | $\Theta(1)$           | $\Theta(n)$        | $\Theta(\log n)$  |
| `DELETE(S, x)`               | $\Theta(1)$           | $\Theta(n)$        | $\Theta(\log n)$  |
| `MINIMUM(S)`                 | $\Theta(n)$           | $\Theta(1)$        | $\Theta(1)$       |
| `EXTRACT-MIN(S)`             | $\Theta(n)$           | $\Theta(n)$        | $\Theta(\log n)$  |
| `DIMINUISCI-CHIAVE(S, x, k)` | $\Theta(1)$           | $\Theta(n)$        | $\Theta(\log n)$ |
| `AUMENTA-CHIAVE(S, x, k)`    | $\Theta(1)$           | $\Theta(n)$        | $\Theta(\log n)$ |


### INSERT

> [!summary] Algoritmo
> 1. Inserisci $x$ in una nuova foglia, più a sinistra possibile nell'ultimo livello dell'albero;
> 2. Iterativamente, scambia di posizione elementi con il loro padre, fin quando la proprietà base dello heap ($\forall v,key(v)\geq key(parent(v))$) non sia stata ristabilita.

> [!example]- <font color="orange">Esempio</font>
> Inserimento di $4$: ![[Pasted image 20230607165619.png]]

La funzione prende in input il vettore $A$ in cui è memorizzato lo heap, ed il valore $k = key[x]$ della chiave del nuovo elemento da inserire. Il campo `heap_size` contiene il numero di elementi attualmente nello heap. 
Ricordiamo che per ogni nodo memorizzato in $A[i]$, il suo padre nello heap è memorizzato in $A[Parent(i)]$, dove $Parent(i) = \lfloor i/2 \rfloor$.

```C
HeapInsert(A, k){
	heap_size = heap_size + 1;
	A[heap_size] = k;
	i = heap_size;
	
	while (i > 1 && A[Parent(i)] > A[i]){
		Scambia(A[i], A[Parent(i)]);
		i = Parent(i);
	}
}
```

Complessità $\Theta(\log n)$.


### EXTRACT-MIN

> [!summary] Algoritmo
> 1. Restituisci l'elemento che si trova nella radice dello heap;
> 2. Sostituisci l'elemento che si trova nella radice con quello che si trova nella foglia più a destra del livello e più in basso dello heap;
> 3. Ripristina la proprietà principale dello heap (se è stata violata) scambiando iterativamente un nodo con il figlio che ha il valore chiave minore.

> [!example]- <font color="orange">Esempio</font>
> Estrai il minimo ($2$): 
> ![[Pasted image 20230607171015.png]]
> ![[Pasted image 20230607171046.png]]

```C
HeapExtractMin(A){
	result = A[1];
	A[1] = A[heap_size];
	heap_size = heap_size - 1;
	MinHeapify(A, 1);
	return result;
}

MinHeapify(A, i){
	l = Left(i);
	r = Right(i);
	
	if(l <= heap_size && A[l] < A[i]){
		smallest = l;
	} else {
		smallest = i;
	}
	
	if(r <= heap_size && A[r] < A[smallest]){
		smallest = r;
	}
	
	if(smallest != i){
		Scambia(A[i], A[smallest]);
		MinHeapify(A, smallest);
	}
}
```

È molto utile ricordare che la procedura `MinHeapify(A, i)` è in generale utilizzabile ogni qualvolta si voglia ripristinare in uno heap la proprietà di base dello heap a causa di un cambiamento del *solo* valore $key(i)$.

Complessità $\Theta(\log n)$.

### MINIMUM
```C
Minimum(A){
	return A[1];
}
```

### AUMENTA-CHIAVE

```C
AumentaChiave(A, i, k){
	A[i] = k;
	MinHeapify(A, i);
}
```

### DIMINUISCI-CHIAVE

> [!summary] Algoritmo
> 1. Imposta il valore;
> 2. Iterativamente, scambia di posizione elementi con il loro padre, fin quando la proprietà base dello heap non sia stata ristabilita.

```C
DiminuisciChiave(A, i, k){
	A[i] = k;
	while (i > 1 && A[Parent(i)] > A[i]){
		Scambia(A[i, A[Parent(i)]);
		i = Parent(i);
	}
}
```

# 

___
[[000 Indice PA|↖ Ritorna all'indice ↖]]

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