---
aliases: 
tags:
  - corsi/informatica/programmazione_strutture_dati
paragrafo: Abstract Data Type
cssclasses:
  - 
---
Uno **Pseudo-Generico** (Falso Generico) è uno strumento che permette la definizione di un tipo placeholder, che viene esplicitato successivamente in fase di compilazione (o [[007 Makefile|linkaggio]]) secondo le necessità.

Questo permette di generalizzare gli algoritmi in modo che possano funzionare con i 3 tipi specificati in precedenza; interi, stringhe e [[018 Strutture|strutture]].

Procediamo in questo modo:
- Creare un tipo generico **Item** (interfaccia) che supporti input, output e confronto;
- Modificare le librerie in modo che operino sul tipo Item; 
- Realizzare Item in 3 file `.c` ed un file `.h` che supportano le 3 varianti intero, stringa e struttura (es.: Nome e Matricola Studente);
- Linkare ed eseguire separatamente (con make) le 3 varianti.

In generale, nei file in cui si intende usare i pseudo-generici, si usa come tipo di argomenti e funzioni `Item`.
Essenzialmente il tipo `Item` è un puntatore a `void`, questo può essere letto come intero, carattere o struttura al linkaggio.

File `item.h`:
Nell'unico file `.h` si esegue il `typedef` a `void`.
```C
typedef void* Item;

Item inputItem();
void outputItem(Item);
int cmpItem(Item,Item);
```

File `item-int.c` oppure `item-string.c`:
Richiede le librerie:
```C
#include <stdio.h>
#include <stdlib.h>
#include "item.h"

//Aggiungere le seguenti in per item-string.c
//#include <string.h> 
//#define NCHAR 100
```

Per **convertire in `Item` un oggetto** si esegue un casting automatico, passandogli l'indirizzo dell'oggetto.
```C
//Richiede item-int
int a = 10;
Item item = &a;
outputItem(item);

//Richiede item-string
char stringa[] = "a";
Item item = &stringa;
outputItem(item);

//Richiede item-char
char carattere = 'a';
Item item = &carattere;
outputitem(item);
```

Si possono anche convertire **array** di oggetti in item.
```C
for(i = 0; i < sizeof(a)/sizeof(int); i++){
	Item item = &a[i];
	outputitem(item);
}
```


#### INPUT ITEM
La `inputItem` ha il compito di ritornare un puntatore intero (che verrà scambiato per tipo `Item`) di un oggetto inserito dall'utente.
```C
//VERSIONE INTERI

//Ritorna il puntatore all'item inserito dall'utente.
Item inputItem(){
	int *p;
	p = malloc(sizeof(int));
	//Alloca spazio per la variabile
	scanf("%d", p); //Prende in input la variabile
	return p;
}

//VERSIONE STRINGHE

//Ritorna il puntatore all'item inserito dall'utente.
Item inputItem(){
	char *p;
	p = malloc(NCHAR * sizeof(char)); 
	//Alloca spazio per tutta la stringa
	scanf("%s", p); //Prende la stringa
	return p;
}
```

Un esempio dove usare l'`inputItem` è la [[1000.F Input Array|Input Array]], dove:
`a[i] = inputItem();`

#### OUTPUT ITEM
La `outputItem` deve stampare un `Item` passato per argomento alla funzione.
```C
//VERSIONE INTERI

//Stampa a video un item.
void outputItem(Item item){
	int *p;
	p = item;
	printf("%d ", *p);
}

//VERSIONE STRINGHE

//Stampa a video un item.
void outputItem(Item item){
	char *p;
	p = item;
	printf("%s ", p);
}
```

Un esempio dove usare l'`outputItem` è la [[1001.F Output Array|Output Array]], dove:
`outputItem(a[i]);`

#### COMPARE ITEM
La `cmpItem` fa un confronto tra due `Item` passati per argomenti. Ritorna: 
- Un intero $>0$ se `item1` $>$ `item2`;
- Un intero $<0$ se `item1` $<$ `item2`;
- $0$ se `item1` $=$ `item2`.
```C
//VERSIONE INTERI

//Confronta due item. Ritorna 0 se item1 = item2, un intero >0 se item1>item2, un intero <0 altrimenti.
int cmpItem(Item item1 ,Item item2){
	int *p1, *p2;
	p1 = item1;
	p2 = item2;
	return *p1 - *p2;
}

//VERSIONE STRINGHE

//Confronta due item. Ritorna 0 se item1 = item2, un intero >0 se item1>item2, un intero <0 altrimenti.
int cmpItem(Item item1 ,Item item2){
	char *p1, *p2;
	p1 = item1;
	p1 = item2;
	return strcmp(p1, p2);
}
```

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