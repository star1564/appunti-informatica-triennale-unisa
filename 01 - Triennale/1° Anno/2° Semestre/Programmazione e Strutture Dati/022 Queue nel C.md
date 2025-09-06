---
aliases:
  - Coda nel C
  - Code nel C
tags:
  - corsi/informatica/programmazione_strutture_dati
paragrafo: Abstract Data Type
cssclasses:
  - 
---
## DEFINIZIONE
Una **Queue** (coda) è una sequenza di elementi di un determinato tipo, in cui gli elementi *si aggiungono da un lato* (tail) *e si tolgono dall'altro lato* (head). 
Non è possibile accedere ad un elemento diverso da head, se non dopo aver eliminato tutti gli elementi che lo precedono.

La sequenza viene gestita con la modalità **FIFO** (First-in-first-out): il primo elemento nella sequenza sarà il primo ad essere eliminato. ^a60071

![[Pasted image 20220408141057.png]]

## POSSIBILI IMPLEMENTAZIONI
### IMPLEMENTAZIONE CON ARRAY
La queue (coda) è implementata come un puntatore ad una [[018 Strutture|struttura]] che contiene tre elementi:
- Un array di `MAX_QUEUE` elementi;
- Un intero che indica la posizione head della coda;
- Un intero che indica la posizione tail della coda.

Quando la coda si riempie, non è possibile eseguire l'operazione enqueue.

Si ha un problema con questa implementazione: quando si eliminano gli elementi head, head scorre a destra. Gli spazi vuoti si trovano quindi a sinistra di head ed a destra di tail.

Per risolverlo si può gestire l'array in modo circolare:
In ogni istante, gli elementi della coda si trovano nel segmento head, head+1, …, tail-1 ma non necessariamente `head <= tail`.

Infatti, dopo aver inserito in posizione `MAX_QUEUE-1` (ultima posizione dell'array), se c'è ancora spazio in coda, si inseriscono ulteriori elementi a partire dalla posizione 0 (prima posizione dell'array); in questo modo si riesce a garantire che ad ogni istante la coda abbia capacità massima di N-1 elementi.

Se `tail+1 == head` non si possono inserire più elementi.

![[b356c9dd-6ee1-460a-8ca1-b4ad914f3dca.jpg]] ^aa5709

```C
//Nel file .h
typedef struct queue *Queue;

/*------------*/

//Nel file .c (ARRAY)
#define MAX_QUEUE 50

struct queue{ //Struttura che rappresenta la queue
	Item elements[MAX_QUEUE]; //Array della queue
	int head; //Indice dell'head
	int tail; //Indice del tail
};
```

### IMPLEMENTAZIONE CON LISTA CONCATENATA
La queue ([[020 Liste nel C|lista concatenata]]) è implementata come un puntatore ad una struttura che contiene una lista che lo rappresenta.

![[bee0b0a2-caa0-4834-b8e4-3c8c765c7cca.png]]

Per motivi di efficienza, conviene avere accesso sia al primo elemento sia all'ultimo. Occorre quindi modificare il tipo lista come un puntatore ad una struct che contiene:
-   Un intero `size` che indica il numero di elementi della coda;
-   Un puntatore `head` ad uno struct nodo;
-   Un puntatore `tail` ad uno struct nodo.

```C
//Nel file .h
typedef struct queue *Queue;

/*------------*/

//Nel file .c (LISTA)
struct queue{ //Struttura che rappresenta la queue
	List elements; //Lista della queue
};
```

## LISTA DELLE FUNZIONI (CON LISTE)
![[cc2deb7e-4084-4674-bc3c-169c7766294a.jpg]]

### CREAZIONE DI UNA QUEUE (newQueue)
Serve per riservare spazio ed inizializzare una nuova queue.
Ha bisogno della [[020 Liste nel C#CREAZIONE DI UNA LISTA newList|newList]].

```C
//Crea e ritorna una nuova queue (vuota).

Queue newQueue(){
	Queue queue = malloc(sizeof(struct queue));
	queue->elements = newList();
	return queue;
}
```

Tipica chiamata:
```C
Queue q = newQueue();
```

### VEDERE SE UNA QUEUE È VUOTA (isEmptyQueue)
Per vedere se una queue è vuota si controlla la dimensione della sua lista.
Ha bisogno della [[020 Liste nel C#VEDERE SE UNA LISTA È VUOTA isEmptyList|isEmptyList]].

```C
//Ritorna 1 se una queue è vuota, 0 altrimenti.

int isEmptyQueue(Queue queue){
	return isEmptyList(queue->elements);
}
```

Tipica chiamata: `isEmptyQueue(queue)`.

### INSERIRE UN ELEMENTO NELLA QUEUE (enqueue)
Per inserire un elemento nella queue si usa la [[020 Liste nel C#INSERIRE IN CODA insertTail|insertTail]].

```C
//Inserisce un elemento nella queue.

int enqueue(Queue queue, Item item){
	return insertTail(queue->elements, item);
}
```

Tipica chiamata: `enqueue(queue, item)`.

### RIMUOVERE UN ELEMENTO DALLA QUEUE (dequeue)
Per rimuovere un elemento da una queue si usa la [[020 Liste nel C#ELIMINAZIONE IN TESTA removeHead|removeHead]].
Controlla se la queue è vuota, se non lo è rimuove e ritorna l'elemento eliminato.

```C
//Rimuove e ritorna un elemento dallo queue.

Item dequeue(Queue queue){
	if (isEmptyQueue(queue))
		printf("Impossibile: la struttura è vuota.\n");
	else
		return removeHead(queue->elements);
}
```

Tipica chiamata: `dequeue(queue)`.

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