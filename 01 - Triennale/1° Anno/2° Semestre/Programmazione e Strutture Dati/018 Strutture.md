---
aliases: 
tags:
  - corsi/informatica/programmazione_strutture_dati
paragrafo: Abstract Data Type
cssclasses:
  - 
---
Attraverso le **Strutture** si possono implementare gli ADT.
Le strutture sono un tipo di dati composito che includono *un elenco di variabili* fisicamente raggruppate in un unico blocco di memoria, per una migliore leggibilità dei programmi.

## STRUTTURE VISIBILI
Le strutture possono essere implementate nel `main.c` oppure in un file `.h` (queste vengono chiamate **Strutture Visibili**).
### CREARE UNA STRUTTURA
Una struttura si può creare attraverso `struct`.
<font color="orange">Esempio</font>:
```C
struct Point {
	float x;
	float y;
};
```
### USARE UNA STRUTTURA
Una struttura può essere dichiarata come un qualsiasi altro tipo di dati, specificando per <font color="orange">esempio</font>:
```C
struct Point p1;
```

Per accedere agli elementi di una struttura si usa l'operatore punto (`.`).
<font color="orange">Esempio</font>:
```C
#include <stdio.h>

struct Point {
	float x;
	float y;
};

int main(void){
	struct Point p1 = {3.0, 2.0}; //x = 3.0, y = 2.0

	p1.x = 20.0; //Accesso e modifica della variabile x
	
	printf("x = %d, y = %d\n", p1.x, p1.y);
	//OUTPUT: x = 20.0, y = 2.0

	return 0;
}
```

## STRUTTURE NON VISIBILI
### PROBLEMI DELLE STRUTTURE VISIBILI
L'implementazione della struttura visibile si esegue nell'Header File (oppure nel main), quindi è visibile al modulo client, che potrebbe accedere direttamente ai campi della struttura senza usare gli operatori dell'ADT.

Per evitare ciò si nasconde l'implementazione della struttura, inserendo un *puntatore* nel file `.h` alla rappresentazione della struttura in un file `.c` a parte. Questa viene chiamata **Struttura non Visibile**.

### CREARE UNA STRUTTURA NASCOSTA
Utilizzeremo la [[004 Programmazione Modulare|Programmazione Modulare]] con tre files:
- `main.c` (programma cliente);
- `punto.h` (dichiarazioni delle funzioni e puntatore alla struttura);
- `punto.c` (implementazione struttura e funzioni).

Le strutture si possono dichiarare e creare allo stesso tempo attraverso la funzione [[017 Allocazione di memoria#MALLOC|malloc]].
Questa *alloca uno spazio nella memoria di dimensione specificata* della struttura. Si usa quando si vuole implementare una funzione per creare una struttura.

### ACCESSO AGLI ELEMENTI DI UNA STRUTTURA NASCOSTA
Si possono accedere agli elementi della struttura nascosta attraverso la freccia (`->`).

Quando si dichiara una struttura, si specifica il tipo scrivendo la prima lettera in maiuscolo.

### ESEMPIO STRUTTURA NASCOSTA
Vogliamo creare una struttura con due punti di un grafico e delle funzioni per interagire con l'ADT. La struttura deve essere nascosta. Tabella ADT:

![[Pasted image 20220317113522.png|900]]

File `punto.h`:
```C
typedef struct point *Point; //Puntatore alla struttura

Point createPoint(float x, float y);
float ascissa(Point p);
float ordinata(Point p);
float distanza(Point p1, Point p2);
```

File `punto.c`:
```C
#include <math.h>
#include <stdlib.h>
#include "punto.h"

//Struttura
struct point {
	float x;
	float y;
};

//Crea e ritorna una nuova struttura con gli elementi specificati.
Point createPoint(float x, float y) {
	Point p = malloc(sizeof(struct point));
	p->x = x;
	p->y = y;
	return p;
}

//Ritorna l'x della struttura.
float ascissa(Point p){
	return p->x;
}

//Ritorna l'y della struttura.
float ordinata(Point p) {
	return p->y;
}

//Calcola e ritorna la distanza tra due punti.
float distanza(Point p1, Point p2) {
	float dx, dy, d;
	dx = p1->x - p2->x; //ascissa(p1) - ascissa(p2);
	dy = p1->y - p2->y; //ordinata(p1) - ordinata(p2);
	d = sqrt((dx*dx) + (dy*dy));
	return d;
}
```

File `main.c`:
```C
#include <stdio.h>
#include "punto.h" 

int main(void){
	Point p1, p2; //Dichiarazione nuovi punti
 
	p1 = createPoint(3.0, 3.0); //Creazione punto 1
	p2 = createPoint(5.0, 5.0); //Creazione punto 2
	
	printf("Distanza: %.1f\n", distanza(p1, p2));
	return 0;
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
<center><font color="rainbow"><i>"This is the Construct. It's our loading program. We can load anything... <br>From clothing to equipment, weapons, training simulations; anything we need."</i></font></center>

Altri collegamenti: 