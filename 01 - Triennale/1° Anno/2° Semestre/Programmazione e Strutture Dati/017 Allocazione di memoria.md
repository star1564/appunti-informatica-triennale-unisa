---
aliases: 
tags:
  - corsi/informatica/programmazione_strutture_dati
paragrafo: Abstract Data Type
cssclasses:
  - 
---
Un array ha due dimensioni, che possono essere uguali o non:
- **Dimensione Fisica** (Cardinalità): è lo spazio totale in memoria allocato, misurato in caselle (1 byte = 8 bit).
- **Dimensione Logica** (Riempimento): è il numero di elementi che si trovano veramente nell'array.

![[Pasted image 20220322114307.png|500]]

Questo può comportare uno *spreco di memoria*. Per evitare questo, si possono allocare spazi di memoria in maniera **Dinamica** al run-time (all'inizio del programma).

Ciò permette di creare [[016 Introduzione agli ADT#STRUTTURE DATI CON GLI ADT|strutture dati]], stringhe e array, la cui dimensione varia (aumenta o diminuisce) durante l'esecuzione, in funzione delle necessità.

## FUNZIONI PER ALLOCARE DINAMICAMENTE
Per allocare dinamicamente la memoria, la libreria `<stdlib.h>` mette a disposizione 3 funzioni specifiche.

### MALLOC
Prototipo:
```C
void *malloc(size_t size);
```

`size_t` è un tipo di intero senza segno definito nella libreria del C.

La `malloc` alloca un blocco di memoria di `size` bytes *senza indicizzarlo*.

Viene usata per allocare qualsiasi tipo di strutture, array e stringhe e *restituisce il puntatore al blocco di memoria allocato*.

Tipica chiamata alla funzione per una struttura:
```C
puntatore = malloc(sizeof(struct nomeStruttura));
```

Tipica chiamata alla funzione per un array o stringa:
```C
int *a, dimArray = 5;
a = (int *) malloc(dimArray * sizeof(int)); //Array

char *b, dimStr = 5;
b = (char *) malloc(dimStr * sizeof(char)); //Stringa
```

Il casting come `(int *)` o `(char *)` normalmente non è obbligatorio, ma alcuni compilatori ne hanno bisogno.

### CALLOC
Prototipo:
```C
void *calloc(size_t numElements, size_t elementSize);
```

La `calloc` alloca un blocco di memoria di `numElements` $\times$ `elementSize` bytes e lo *inizializza a 0* (clear).

Questa funzione viene usata soprattutto per gli array e le stringhe e *restituisce il puntatore al blocco*.

Tipica chiamata alla funzione per un array o stringa:
```C
int *a, dimArray = 5;
a = (int *) calloc(dimArray, sizeof(int)); //Array (5 e.)

char *b, dimStr = 5;
b = (char *) calloc(dimStr, sizeof(char)); //Stringa (5 e.)
```

### REALLOC
Prototipo:
```C
void *realloc(void *pointer, size_t size);
```

La `realloc` cambia la dimensione del blocco di memoria precedentemente allocato puntato da `pointer`.

Questa funzione *restituisce il puntatore* ad una zona di memoria di dimensione `size`, che *contiene gli stessi dati* della vecchia regione indirizzata da `pointer` *troncata* alla fine nel caso della nuova dimensione (sia minore) di quella precedente.

Tipica chiamata alla funzione per un array o stringa:
```C
int *a, dimArray = 5;
a = malloc(dimArray * sizeof(int)); //Array(5)
//RIALLOCAZIONE ARRAY
int nuovaDimArray = 10;
a = realloc(a, nuovaDimArray); //Array(10)


char *b, dimStr = 5;
b = malloc(dimStr * sizeof(char)); //Stringa(5)
//RIALLOCAZIONE STRINGA
int nuovaDimStr = 10;
b = realloc(b, nuovaDimStr); //Stringa(10)
```

## FREE
La `free` serve per liberare uno spazio allocato, come una variabile, array, struttura, ecc.
Tipica chiamata: `free(tmp)`.

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
