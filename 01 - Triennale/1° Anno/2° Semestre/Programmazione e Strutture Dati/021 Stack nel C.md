---
aliases: 
tags:
  - corsi/informatica/programmazione_strutture_dati
paragrafo: Abstract Data Type
cssclasses:
  - 
---
## DEFINIZIONE
Uno **stack** (pila) è una sequenza di elementi di un determinato tipo, in cui è possibile aggiungere o togliere elementi *esclusivamente da un unico lato* (top dello stack).
Non è possibile accedere ad un elemento diverso dal primo se non dopo aver eliminato tutti gli elementi che lo precedono.

È una lista gestita con la modalità **LIFO** (Last-In-First-Out) cioè l'ultimo elemento inserito nella sequenza sarà il primo ad essere eliminato. ^c22c74

![[d8b60aa1-f791-4610-a439-c7b2851d9886.png]]

## POSSIBILI IMPLEMENTAZIONI
### IMPLEMENTAZIONE CON ARRAY
Lo stack (array) è implementato come un puntatore ad una [[018 Strutture|struttura]] che contiene due elementi:
- Un array di `MAX_STACK` elementi;
- Un intero che indica la posizione del top dello stack + 1.
Quando lo stack si riempie (ovvero quando `top == MAX_STACK`), non è possibile inserire altri elementi.

![[a6a7db6e-73d2-4f51-81a4-de368949af3b.png|600]]

```C
//Nel file .h
typedef struct stack *Stack;

/*------------*/

//Nel file .c (ARRAY)
#define MAX_STACK 50

struct stack{ //Struttura che rappresenta lo stack
	Item elements[MAX_STACK]; //Array dello stack 
	int top; //Indice del top+1
};
```

### IMPLEMENTAZIONE CON LISTA CONCATENATA
Lo stack ([[020 Liste nel C|lista concatenata]]) è implementato come un puntatore ad una struttura che contiene una lista che lo rappresenta.

![[45feb7e8-110a-4d42-b9a6-3a7250484cc0.png]]

```C
//Nel file .h
typedef struct stack *Stack;

/*------------*/

//Nel file .c (LISTA)
struct stack{ //Struttura che rappresenta lo stack
	List elements; //Lista dello stack
};
```

## LISTA DELLE FUNZIONI (CON LISTE)
![[c314fe4c-8b0a-4c7f-919d-2ba5be100323.jpg]]

### CREAZIONE DI UNO STACK (newStack)
Serve per riservare spazio ed inizializzare un nuovo stack.
Ha bisogno della [[020 Liste nel C#CREAZIONE DI UNA LISTA newList|newList]].

```C
//Crea e ritorna un nuovo stack (vuoto).

Stack newStack(){
	Stack stack = malloc(sizeof(struct stack));
	stack->elements = newList();
	return stack;
}
```

Tipica chiamata:
```C
Stack s = newStack();
```

### VEDERE SE UNO STACK È VUOTO (isEmptyStack)
Per vedere se uno stack è vuoto si controlla la dimensione della sua lista.
Ha bisogno della [[020 Liste nel C#VEDERE SE UNA LISTA È VUOTA isEmptyList|isEmptyList]].

```C
//Ritorna 1 se uno stack è vuoto, 0 altrimenti.

int isEmptyStack(Stack stack){
	return isEmptyList(stack->elements);
}
```

Tipica chiamata: `isEmptyStack(stack)`.

### INSERIRE UN ELEMENTO NELLO STACK (push)
Per inserire un elemento nello stack si usa la [[020 Liste nel C#INSERIMENTO IN TESTA addHead|addHead]].

```C
//Inserisce un elemento nello stack.

void push(Stack stack, Item item){
	addHead(stack->elements, item);
}
```

Tipica chiamata: `push(stack, item)`.

### RIMUOVERE UN ELEMENTO NELLO STACK (pop)
Per rimuovere un elemento da uno stack si usa la [[020 Liste nel C#ELIMINAZIONE IN TESTA removeHead|removeHead]].
Controlla se lo stack è vuoto, se non lo è rimuove e ritorna il top.

```C
//Rimuove e ritorna un elemento dallo stack.

Item pop(Stack stack){
	if (isEmptyStack(stack))
		printf("Impossibile: la struttura è vuota.\n");
	else
		return removeHead(stack->elements);
}
```

Tipica chiamata: `pop(stack)`.

### RITORNO DEL VALORE DEL TOP (top)
Si controlla se lo stack è vuoto e si ritorna il top dello stack.
Ha bisogno della [[020 Liste nel C#RITORNO DEL VALORE DELL'HEAD getHead|getHead]].

```C
//Ritorna l'item del top dello stack.

Item top(Stack stack){
	if (isEmptyStack(stack))
		printf("Impossibile: la struttura è vuota.\n");
	else
		return getHead(stack->elements);
}
```

Tipica chiamata: `top(stack)`.

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