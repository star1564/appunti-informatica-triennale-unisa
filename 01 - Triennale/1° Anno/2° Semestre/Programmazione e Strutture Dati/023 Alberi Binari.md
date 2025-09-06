---
aliases: 
tags:
  - corsi/informatica/programmazione_strutture_dati
paragrafo: Abstract Data Type
cssclasses:
  - 
---
![[012 Grafi#Definizione]]

> [!note]- Definizione vecchia di PSD
>Il *Grafo* è una struttura dati alla quale si possono ricondurre strutture più semplici: [[020 Liste nel C|Liste]] ed Alberi.
>Un grafo orientato $G$ è una coppia $<N, A>$ dove $N$ è un insieme finito non vuoto di *Nodi*, mentre $A\textcolor{#CC241D}\subseteq N\times N$ è un insieme finito di coppie ordinate di nodi, detti *Archi*.
>
><font color="orange">Esempio</font>:
>$N=\{U1,U2,U3\}$
>$A=\{(U1, U1), (U1,U2),(U2,U1),(U1,U3)\}$
>Si può rappresentare graficamente il grafo in questo modo.
>
>Da notare le frecce sugli archi, queste indicano le relazioni tra i nodi che si possono trovare nell'insieme $A$.

---
![[013 Alberi#Albero Radicato]]

## ALBERI BINARI
Gli **Alberi binari** sono dei particolari alberi dove *ogni nodo può avere al massimo due figli*, detti il **Sottoalbero Sinistro** ed il **Sottoalbero Destro**.

Quindi un albero binario è una terna $(s, r, d)$ composta dal nodo radice $(r)$, ed il sottoalbero binario sinistro e destro $(s, d)$.

![[Pasted image 20220506113635.png|550]]

Possiamo notare se un nodo è una foglia vedendo se i suoi sottoalberi sinistro e destro sono entrambi `NULL`.
![[Pasted image 20220506160055.png|500]]


### ALGORITMI DI VISITA DI UN ALBERO BINARIO
La visita di un albero consiste nel seguire una rotta di viaggio che consenta di esaminare ogni nodo dell'albero esattamente una volta. Queste visite possono essere ricorsive oppure iterative.

- **Visita in Pre-Ordine** (pre-order): si applica ad un albero non vuoto e richiede dapprima l'analisi della radice dell'albero, poi la visita effettuata con lo stesso metodo dei due sottoalberi, prima il sinistro poi il destro;
![[Pasted image 20220506115312.png|400]]
 ^d9a8a0
- **Visita in Post-Ordine** (post-order): si applica ad un albero binario non vuoto e richiede dapprima la visita, effettuata con lo stesso modo, dei sottoalberi, prima il sinistro e poi il destro, in seguito si procede con l'analisi della radice dell'albero.
![[Pasted image 20220506115657.png|400]]
 ^bdc2a9
- **Visita Simmetrica**: richiede prima la visita del sottoalbero sinistro (effettuata sempre con lo stesso metodo), poi l'analisi della radice ed infine la visita del sottoalbero destro.
![[Pasted image 20220506115945.png|400]] ^236869

<font color="orange">Esempio</font>: Effettuare i tre tipi di visite sul seguente albero.
![[54e41e32-44a7-4258-9f37-5cf6e367ab14.jpg]]

## IMPLEMENTAZIONE ALBERI BINARI
![[02559baa-f1f3-4678-8b68-b0a65e2f0230.jpg]]

### FILE .H
```C
#include "item.h"
typedef struct node *BTree; //Puntatore alla struttura

//Funzioni...
```

### FILE .C
```C
#include "item.h"
#include "btree.h"

struct node{
	Item value; //Etichetta del nodo
	struct node *left; //Puntatore al sottoalbero sinistro
	struct node *right; //Puntatore al sottoalbero destro
};
```

#### CREARE ED ASSEMBLARE UN ALBERO
Per creare un nuovo albero si fa solamente ritornare un `NULL`.
Questa funzione è essenzialmente inutile. 
```C
BTree newTree(){
	return NULL;
}
```

Invece per assemblare un albero, si crea con la malloc un nodo e gli si assegnano i due sottoalberi e l'item.
```C
BTree buildTree(BTree left, BTree right, Item item){
	BTree bt = malloc(sizeof(struct node));
	bt->value = item;
	bt->left = left;
	bt->right = right;
	return bt;
}
```

Per assemblare un albero nel main bisogna cominciare creando prima le foglie, dove i sottoalberi sono tutte e due dei `NULL`. Per poi andare verso la radice.

<font color="orange">Esempio</font>:
```C
BTree radice, sinistro, destro;
sinistro = buildTree(NULL, NULL, "s"); //Sottoalbero sin.
destro = buildTree(NULL, NULL, "d"); //Sottoalbero destro
radice = buildTree(sinistro, destro, "r"); //Radice
```

#### VEDERE SE UN SOTTOALBERO È VUOTO
Questa funzione serve per vedere se un *sottoalbero* è vuoto. Per vedere se un intero albero è vuoto si deve mandare la radice.

```C
int isEmptyTree(BTree bt){
	if (bt == NULL)
		return 1;
	return 0;
}
```

#### OTTENERE L'ETICHETTA DELLA RADICE DI UN SOTTOALBERO
Si vede prima se il sottoalbero è vuoto (se è vero ritorna `NULL`) poi ritorna l'etichetta.
```C
Item getBTreeItem(BTree bt){
	if (isEmptyTree(bt))
		return NULL;
	return bt->value;
}
```

#### OTTENERE IL SOTTOALBERO DESTRO O SINISTRO
Si vede prima se il sottoalbero è vuoto (se è vero ritorna `NULL`) poi ritorna il sottoalbero desiderato.
```C
BTree getLeft(BTree bt){
	if (isEmptyTree(bt))
		return NULL;
	return(bt->left); //Oppure return(bt->right)
}
```

### IMPLEMENTAZIONE DELLE VISITE (RICORSIVE)
Per facilità di comprensione degli algoritmi, osservare le frecce gialle nelle immagini del paragrafo dedicato agli algoritmi di visita (collegamenti qui in basso).

#### [[023 Alberi Binari#^d9a8a0|PRE-ORDINE]]
```C
void preOrder(BTree bt){
	if(!isEmptyTree(bt)){ //Passo Base, l'albero è vuoto
		outputItem(bt->value); //Stampa la radice
		preOrder(bt->left); //Visita a sinistra
		preOrder(bt->right); //Visita a destra
	}
}
```

#### [[023 Alberi Binari#^bdc2a9|POST-ORDINE]]
```C
void postOrder(BTree bt){
	if(!isEmptyTree(bt)){ //Passo Base, l'albero è vuoto
		postOrder(bt->left); //Visita a sinistra
		postOrder(bt->right); //Visita a destra
		outputItem(bt->value); //Stampa la radice
	}
}
```

#### [[023 Alberi Binari#^236869|SIMMETRICA]]
```C
void inOrder(BTree bt){
	if(!isEmptyTree(bt)){ //Passo Base, l'albero è vuoto
		inOrder(bt->left); //Visita a sinistra
		outputItem(bt->value); //Stampa la radice
		inOrder(bt->right); //Visita a destra
	}
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