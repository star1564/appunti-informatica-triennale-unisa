---
aliases: 
tags:
  - corsi/informatica/programmazione_strutture_dati
paragrafo: Abstract Data Type
cssclasses:
  - 
---
## DEFINIZIONE
Un **Albero Binario di Ricerca (BST)** è un [[023 Alberi Binari#ALBERI BINARI|albero binario]] che viene utilizzato principalmente per facilitare la ricerca e l'inserimento di valori numerici al suo interno.

È organizzato in questo modo:
- Si ha una radice con un valore;
- Il valore del sottoalbero sinistro è minore ($\color{#61AFEF}<$) di quello della radice;
- Il valore del sottoalbero destro è maggiore ($\color{#61AFEF}>$) di quello della radice;
- I sottoalberi sinistro e destro sono a loro volta degli *alberi binari di ricerca*.

<font color="orange">Esempio</font>:
![[Pasted image 20220506145853.png|650]]

---
### ALGORITMO DI INSERIMENTO NODI
Per inserire nodi (al contrario degli alberi binari) si utilizza una funzione ricorsiva.
Questa, parte dalla radice dell'albero e si comporta nel seguente modo:
- Controlla se l'albero è vuoto:
	- Se è vuoto alloca spazio per un nodo, inserisce l'item nell'etichetta ed inizializza i sottoalberi a `NULL`; continua.

- Si confronta l'item da inserire con l'etichetta del nodo corrente:
	- Se l'item da inserire è *minore* di quello del nodo, si richiama questa funzione sul sottoalbero sinistro;
	- Se l'item da inserire è *maggiore* di quello del nodo, si richiama questa funzione sul sottoalbero destro;
	- Se l'item è uguale a quello del nodo, la funzione termina.

<font color="orange">Esempio</font>:
![[b33c13c9-1363-45d0-9335-cc9c260b19cf.png]]

---
### ALGORITMO DI RICERCA
Questo algoritmo ricorsivo si comporta similmente alla [[1013.F Ricerca Binaria|ricerca binaria]].
Si parte dalla radice:
- Se l'albero è vuoto, ritorna `NULL`;

- Se l'elemento cercato coincide con la radice dell'albero restituisce l'item della radice;

- Se l'elemento cercato è minore della radice restituisce il risultato della ricerca dell'elemento nel sottoalbero *sinistro*;

- Se l'elemento cercato è maggiore della radice restituisce il risultato della ricerca dell'elemento nel sottoalbero *destro*.

<font color="orange">Esempio</font>: Ricerca del 7.
![[Immagine 2022-05-06 160236.png|400]]

---
### ALGORITMO DI ELIMINAZIONE
Si ricerca ricorsivamente il nodo da rimuovere. Se il nodo è stato trovato possiamo avere tre casi.

#### CASO 1
Il nodo da eliminare è una foglia.
Si può semplicemente eliminare il nodo dall'albero.

![[Pasted image 20220512143727.png]]

#### CASO 2
Il nodo da eliminare ha un solo figlio.
- Si copia il sottoalbero figlio al nodo da eliminare;
- Si elimina il figlio.

![[Pasted image 20220512144756.png]]

#### CASO 3
Il nodo da eliminare ha due figli.
- Si sostituisce l'elemento da eliminare con il massimo nel sottoalbero sinistro (alternativamente si cerca di sostituire con l'elemento minimo nel sottoalbero destro).
- Si elimina il nodo del minimo (o massimo).

![[2886b0f5-2faa-4914-aac8-19153769e706.jpg|600]]

---
### COMPLESSITÀ DELLE OPERAZIONI
Le operazioni sull'albero binario di ricerca hanno [[031 Notazioni Asintotiche|complessità]] $\color{#CC241D}O(h)$, dove $h$ è l'altezza dell'albero.

Nel *caso peggiore*, il nodo da ricercare (da inserire o da eliminare) si troverà a distanza $h$ dalla radice.

In un albero bilanciato con $n$ nodi, l'altezza dell'albero è $\color{#CC241D}log_2(n)$.

## IMPLEMENTAZIONE
![[2d89f27d-e742-4466-ba43-3bb0d213d21a.jpg]]

### FILE .H
```C
#include "item.h"
typedef struct node *BST;

//Funzioni...
```

### FILE .C
```C
struct node{
	Item value; //Etichetta del nodo
	struct node *left; //Puntatore sottoalbero sin.
	struct node *right; //Puntatore sottoalbero destro
};
```

Molte funzioni sono simili a quelle degli alberi binari normali.

#### CREARE UN ALBERO
Questa volta la funzione vene utilizzata.

```C
BST newBST(){
	return NULL;
}
```

#### VEDERE SE UN SOTTOALBERO È VUOTO
Questa funzione serve per vedere se un *sottoalbero* è vuoto. Per vedere se un intero albero è vuoto si deve mandare la radice.

```C
int isEmptyBST(BST bst){
	if (bst == NULL)
		return 1;
	return 0;
}
```

#### INSERIRE UN SOTTOALBERO
Per assemblare un albero si devono inserire dei sottoalberi.

```C
void insertBST(BST *bst, Item e){
	if(isEmptyBST(*bst)){ //Se raggiunge un nodo nullo
		*bst = malloc(sizeof(struct node));
		(*bst)->left = NULL;
		(*bst)->right = NULL;
		(*bst)->value = e;
	}

	int value;
	value = cmpItem(e,(*bst)->value);
	if (value < 0)
		insertBST(&((*bst)->left), e); 
		//Inserisci a sinistra
	else if(value > 0)
		insertBST(&((*bst)->right), e); 
		//Inserisci a destra
}
```

<font color="orange">Esempio</font> nel main:
```C
int arr[] = {20,10,30,5,15,25,40,3,6,35};
BST bst;
bst = newBST();
int i;
for(i = 0; i < sizeof(arr)/sizeof(int); i++)
	insertBST(&bst, &arr[i]);
```

#### OTTENERE L'ETICHETTA DELLA RADICE DI UN SOTTOALBERO
Si vede prima se il sottoalbero è vuoto (se è vero ritorna `NULL`) poi ritorna l'etichetta.
```C
Item getItem(BST bst){
	if (isEmptyBST(bst))
		return NULL;
	return bst->value;
}
```

#### OTTENERE IL SOTTOALBERO DESTRO O SINISTRO
Si vede prima se il sottoalbero è vuoto (se è vero ritorna `NULL`) poi ritorna il sottoalbero desiderato.
```C
BST getLeft(BST bst){
	if (isEmptyBST(bst))
		return NULL;
	return(bst->left); //Oppure return(bst->right)
}
```

#### OPERAZIONE DI RICERCA
```C
Item search(BST bst, Item e){
	if (isEmptyBST(bst))
		return NULL;

	int value;
	value = cmpItem(e,bst->value);
	if (value == 0)
		return bst->value;
	if (value < 0)
		return search(bst->left, e);
	else
		return search(bst->right, e);
}
```

#### OPERAZIONE DI RICERCA DEL MINIMO E DEL MASSIMO
Per ricercare il minimo (o il massimo) elemento si sfrutta la composizione dell'albero binario di ricerca.
Ovvero si va a cercare l'elemento che si trova più in basso a sinistra (o a destra per il massimo).

```C
Item min(BST bst){
	if(isEmptyBST(bst))
		return NULL;
	if (bst->left != NULL)
		return min(bst->left);
	else
		return bst->value;
}

Item max(BST bst){
	if(isEmptyBST(bst))
		return NULL;
	if (bst->right != NULL)
		return max(bst->right);
	else
		return bst->value;
}
```

#### ELIMINAZIONE DI UN NODO
Nella funzione vengono riportati tutti e tre i casi.
È consigliato vedere il [[024 Alberi Binari di Ricerca#ALGORITMO DI ELIMINAZIONE|paragrafo dedicato all'eliminazione]].

```C
Item deleteBST(BST *bt, Item i)
{
	if(isEmptyBST(*bt))
		return NULL;
	
	int a = cmpItem(i, (*bt)->value);
	
	if(a == 0){ //Se i = valore radice albero
		//Caso 1 - 2 (0 figli o un figlio a destra)
		if(isEmptyBST((*bt)->left)){ 
			BST temp = *bt;
			Item e = (*bt)->value;
			*bt = (*bt)->right;
			free(temp);
			return e;
		}
		//Caso 2 (un figlio a sinistra)
		else if(isEmptyBST((*bt)->right)){
			BST temp = *bt;
			Item e = (*bt)->value;
			*bt = (*bt)->left;
			free(temp);
			return e;
		}
		//Caso 3 (due figli)
		else {
			Item m = max(*bt);
			Item e = (*bt)->value;
			(*bt)->value = m;
			deleteBST(&((*bt)->left), m);
			return e;
		}
	}	
	else if(a > 0) //Se i > valore
		return deleteBST(&((*bt)->right), i);
	else if(a < 0) //Se i < valore
		return deleteBST(&((*bt)->left), i);
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