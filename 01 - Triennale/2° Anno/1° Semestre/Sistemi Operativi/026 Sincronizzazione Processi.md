---
aliases: 
tags:
  - corsi/informatica/sistemi_operativi
paragrafo: Sincronizzazione Processi
cssclasses:
  - 
---
## PRODUTTORE-CONSUMATORE
Supponiamo di avere un [[Processo (Job)|Processo]] **Produttore** che produce informazioni, ed un processo **Consumatore** che le ottiene.

I due processi hanno un [[Buffer]] (zona di memoria) in comune, nel quale possono scrivere e leggere.
Lo si definisce nel seguente modo.
```C
#define BUFFER_SIZE 10

typedef struct {
	...
} elemento;

elemento buffer[BUFFER_SIZE];

int in = 0; 
int out = 0; 
int contatore = 0;
```
Abbiamo a disposizione una struttura contenente un certo numero di informazioni.

Dichiariamo il *`buffer`* come un array di tanti dati di tipo *`elemento`*, dove il numero di elementi massimo *`BUFFER_SIZE`* è 10.
Si hanno anche delle variabili in comune tra i due processi: 
- *`in`*: è l'indice della successiva posizione *libera* nel `buffer`; 
- *`out`*: è l'indice della prima posizione *piena* nel `buffer`; 
- *`contatore`*: è il *numero totale* di elementi nel `buffer`.

### PROCESSO PRODUTTORE
Il *processo produttore* deve eseguire il seguente codice:
```C
elemento appena_prodotto;

while(1){
	while(contatore == BUFFER_SIZE)
		; // Se si è raggiunto il massimo di elementi, non fare nulla fino a quando contatore non diminuisce
	buffer[in] = appena_prodotto;
	in = (in + 1) % BUFFER_SIZE; // Se finisce lo spazio dell'array, ricomincia a mettere gli elementi dall'inizio
	contatore++;
}
```

Supponiamo che il produttore abbia appena creato un elemento (`appena_prodotto`) che deve essere scritto nel `buffer`.

> [!tip]- Flowchart
![[Immagine 2022-11-17 122506.png]]

1. Se il `buffer` è pieno, si "gira a vuoto" facendo aspettare il processo produttore;
2. Se invece c'è spazio allora si inserisce l'elemento nella prima posizione libera disponibile (indice `in`), calcolata in modo circolare attraverso il modulo ([[022 Queue nel C#^aa5709|come in questo caso]]);
3. Si incrementa anche il contatore del `buffer`.

### PROCESSO CONSUMATORE
Il *processo consumatore* deve eseguire il seguente codice:
```C
elemento da_consumare;

while(1){
	while(contatore == 0)
		; // Se il buffer è vuoto, non fare nulla fino a quando contatore non aumenta
	da_consumare = buffer[out];
	out = (out + 1) % BUFFER_SIZE; // Se si va al di sotto dell'indice 0 dell'array, ricomincia a prendere gli elementi dalla fine
	contatore--;
}
```

Il suo scopo è quello di prelevare gli elementi dall'array. Funziona quasi come per il processo produttore.

> [!tip]- Flowchart
![[Immagine 2022-11-17 125551.png]]

## PROBLEMA
> [!error] In questo caso si ha la [[Race Condition|RACE CONDITION]]
> Prese separatamente, le procedure del produttore e del consumatore sono corrette, ma possono "non funzionare" se eseguite in [[Concorrenza]].
> In particolare, le istruzioni `contatore++` e `contatore--` possono causare problemi se non sono [[Operazione atomica|atomiche]].
>
> Infatti se il produttore ed il consumatore tentano di accedere al buffer contemporaneamente, le istruzioni in linguaggio macchina possono risultare *interfogliate*.
> La sequenza effettiva di esecuzione dipende da come i processi produttore e consumatore vengono [[021 Scheduling di Processi e Code|schedulati]].

Quello che accade realmente quando si effettua l'incremento/decremento di un contatore è che:
1. Si va a prendere il valore del contatore e lo si mette in un [[058 Registri|Registro]];
2. Si incrementa/decrementa il valore del registro;
3. Si mette il valore del registro incrementato/decrementato nel contatore.

L'incremento e il decremento del contatore non sono operazioni atomiche, dunque se il processo produttore/consumatore perde la CPU in una posizione intermedia dell'operazione, questo può causare problemi.

Per questo ci sono [[027 Soluzioni alla Race Condition|varie soluzioni]].

___
[[000 Indice SO|↖ Ritorna all'indice ↖]]

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