---
aliases: 
tags:
  - corsi/informatica/programmazione_strutture_dati
paragrafo: Introduzione da Programmazione 1
cssclasses:
  - 
---
> [!success] [Video Consigliato](https://www.youtube.com/watch?v=2ybLD6_2gKM)

Un **Puntatore** è una variabile che contiene l'indirizzo di un'altra variabile.

I puntatori sono "type bound" cioè ad ogni puntatore è associato il tipo a cui il puntatore si riferisce.

Nella dichiarazione di un puntatore bisogna specificare un asterisco `*` prima del nome della variabile pointer.

```C
int *pun_int; //Puntatore a intero
char *pun_char; //Puntatore a carattere
float *pun_flt; //Puntatore a float

int i = 50;
pun_int = &i; //Assegnazione del puntatore a una variabile
```

Il C fornisce una coppia di operatori che sono specificamente pensati per l'utilizzo con i puntatori.

### OPERATORE INDIRIZZO
Ogni byte possiede un *indirizzo* univoco che lo distingue dagli altri presenti in [[030 Memoria Centrale|Memoria Principale]] (suddivisa in byte), dove ogni $n$ byte è una variabile.

Per trovare l'indirizzo di una variabile si usa l'**Operatore Indirizzo (`&`)**.

Se `x` è una variabile, allora `&x` è il suo indirizzo di memoria.

```C
int i, *p;
p = &i;

/*--------*/

int j;
int *q = &j;
```

Questa istruzione, assegnando l'indirizzo di `i` alla variabile `p`, fa sì che `p` punti ad `i`.

![[Pasted image 20220322122744.png|300]]

### OPERATORE ASTERISCO
Una volta che una variabile puntatore punta a un oggetto, possiamo usare l'**Operatore Asterisco (`*`)** per accedere al contenuto dell'oggetto stesso.
Fino a quando un puntatore punta ad una variabile, il primo diventa un *alias* per l'ultima.

```C
int i, *p;

p = &i; //p punta ad i

i = 1; //p adesso punta ad i che ha il valore 1

printf("%d\n", i); //Stampa 1
printf("%d\n", *p); //Stampa 1

*p = 2; //Modifica della variabile con il puntatore

printf("%d\n", i); //Stampa 2
printf("%d\n", *p); //Stampa 2
```

## PASSARE GLI INDIRIZZI DELLE VARIABILI ALLE FUNZIONI
I dati vengono passati **per argomenti**, cioè si passano alla funzione delle *copie* di variabili. Per esempio se abbiamo:

```C
void swap(int a, int b){
 int temp;
 temp = a;
 a = b;
 b = temp;
}

int main(void){
 int a = 2, b = 3;
 swap(a,b);
 printf("a=%d, b=%d", a, b);
 return 0;
}
```

Avremo che le variabili rimarranno le stesse: `a=2, b=3`.

Per non mandare delle copie alle funzioni, si devono mandare gli **indirizzi delle variabili** (con `&`).
In oltre le funzioni, per poter *ricevere* dei puntatori, hanno come argomento gli operatori `*` vicino ai nomi.

```C
void swap(int *a, int *b){
 int temp;
 temp = *a;
 *a = *b;
 *b = temp;
}

int main(void){
 int a = 2, b = 3;
 swap(&a,&b);
 printf("a=%d, b=%d", a, b);
 return 0;
}
```

Adesso avremo che: `a=3, b=2`.

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