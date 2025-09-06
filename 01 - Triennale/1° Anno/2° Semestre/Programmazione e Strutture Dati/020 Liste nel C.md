---
aliases: 
tags:
  - corsi/informatica/programmazione_strutture_dati
paragrafo: Abstract Data Type
cssclasses:
  - 
---
## DEFINIZIONE
Le **Liste** sono delle *sequenze di elementi di un determinato tipo*, in cui è possibile aggiungere o rimuovere elementi. È possibile specificare la posizione relativa nella quale l'elemento va aggiunto o tolto.

Ogni elemento di una lista concatenata è un record, contenente un puntatore che serve da collegamento per il record successivo.

### RAPPRESENTAZIONE DI UNA LISTA
Una lista concatenata è rappresentata da un puntatore al primo nodo della lista. Il primo nodo è chiamato <font color=#00b050>Head</font>.
Se l'elenco della lista è vuoto, il valore di Head è `NULL`.

Ciascun nodo in un elenco è costituito da almeno due parti:
- Dato da conservare (<font color=#00b050>Data</font>);
- Puntatore al nodo successivo (<font color=#00b050>Next</font>).

![[bd119c11-e38c-453a-b109-6937e730a8a2.png]]

### DIFFERENZE CON GLI ARRAY
Gli array hanno una dimensione fissa (dichiarata in compilazione), si può accedere direttamente ad ogni elemento con un indice.

Le liste hanno dimensioni variabili, si può accedere direttamente solo al *primo elemento*.

#### ACCESSO AGLI ELEMENTI
- Lista Concatenata: per accedere all'i-esimo elemento occorre scorrere la lista dal primo elemento (tempo massimo proporzionale ad $n$).
- *Array*: ogni elemento di una array è accessibile direttamente usando il suo indice (tempo costante).

#### INSERIMENTO E CANCELLAZIONE
- *Lista Concatenata*: dato un elemento, è possibile eliminarlo o aggiungerne un altro subito dopo.
- Array: occorre effettuare delle [[1007.F Cancella Pos Array|operazioni di shift]].

## IMPLEMENTAZIONE LISTA CONCATENATA
Nel C le liste si possono rappresentare usando le [[018 Strutture|strutture]] (`struct`).
Utilizzeremo come tipo nella lista lo [[019 Pseudo-Generici|pseudo-generico Item]].

> [!tip]- list.h
> ```C
>typedef struct list *List; //Puntatore alla struttura list
>
>//Funzioni...
>```

> [!tip]- list.c
> ```C
> //Struttura che rappresenta l'intera lista.
> struct list {
> 	int size; //Dimensione della lista
> 	struct node *head; //Puntatore all'Head della lista
> };
>
> //Struttura auto-referenziale che rappresenta i nodi
> struct node {
> 	Item value; //Dato del nodo
> 	struct node *next;
> 	//Puntatore al prossimo nodo della lista
> };
>
> //Funzioni...
> ```

### IMPLEMENTAZIONE CON ARRAY
Le liste si possono implementare anche grazie all'utilizzo degli array.


> [!tip]- list-array.h
> ```C
>typedef struct list *List; //Puntatore alla struttura list
>
>//Funzioni...
>```

> [!tip]- list-array.c
> ```C
>//Struttura che rappresenta l'intera lista.
>struct list {
>	Item array[MAX_LIST] //Array lista
>	int size; //Dimensione della lista
>};
>
> //Funzioni...
> ```

## FUNZIONI LISTA
![[75b5a065-5d58-4f03-9e42-dc32fc8e22fc.png]]

### CREAZIONE DI UNA LISTA (newList)
Serve per riservare spazio ed inizializzare una nuova lista.
```C
//Crea e ritorna una nuova lista (vuota).

List newList(){
	List list = malloc(sizeof(struct list));
	//Alloca spazio per una nuova lista
	
	list->size = 0;
	list->head = NULL;
	
	return list;
}
```

Tipica chiamata:
```C
List l = newList();
```

#### VISITA ED ITERAZIONE SUI NODI DI UNA LISTA (printList)
Per iterare su una lista (per esempio si vuole *Stampare una Lista*) si usa un ciclo `for`.
```C
//Stampa tutta la lista.

void printList(List list){
	struct node *p; //Puntatore nodo per iterare
	for (p = list->head; p != NULL; p = p->next)
		outputItem(p->value); //Stampa il valore
}
```

Tipica chiamata: `printlist(list)`.

#### CREAZIONE ED ALLOCAZIONE DI UN NUOVO NODO
Per allocare uno spazio per un nuovo nodo si usa la [[017 Allocazione di memoria#MALLOC|malloc]].
```C
struct node *new_node = malloc(sizeof(struct node));

/*---Oppure---*/

struct node *new_node;
new_node = malloc(sizeof(struct node));
```

### VEDERE SE UNA LISTA È VUOTA (isEmptyList)
Per vedere se una lista è vuota si controlla la sua dimensione.

```C
//Ritorna 1 se una lista è vuota, 0 altrimenti.

int isEmptyList(List list){
	if (list->size)
		return 0; //Se size != 0 non è vuota
	return 1;
}
```

Tipica chiamata: `isEmptyList(list)`.

### INSERIMENTO IN TESTA (addHead)
Il modo più semplice per inserire un nuovo elemento in una lista concatenata è quello di inserire l'elemento in un nuovo nodo da aggiungere in testa alla lista. Procedimento:
1. Si alloca il nuovo nodo E;
2. Si inserisce l'item nel nodo E;
3. Si aggiunge il collegamento E->next al nodo A;
4. Si sposta il collegamento Head della lista al nodo E;
5. Si aumenta di 1 la dimensione della lista.

![[b855540b-d311-4b24-a843-cf073aa2fd0c.png|600]]

```C
//Aggiunge un nuovo nodo e lo fa diventare l'Head della lista.

void addHead(List list, Item value){
	struct node *new_node = malloc(sizeof(struct node)); 
	//Creazione ed allocazione di un nuovo nodo

	new_node->value = value;
	new_node->next = list->head;
	list->head = new_node; 
	//new_node diventa l'Head della lista

	(list->size)++;
}
```

Tipica chiamata: `addHead(list, item)`.

### ELIMINAZIONE IN TESTA (removeHead)
Il modo più semplice per eliminare un elemento in una lista concatenata è quello di eliminare l'Head. Ritorna l'item eliminato. Procedimento:
1. Si vede se la lista è vuota;
	- Se lo è, ritorna `NULL`. 
2. Si crea un puntatore temporaneo di tipo node `tmp`, copia del nodo Head;
3. Si aggiorna Head, facendolo puntare a tmp->next;
4. Si copia l'item tmp->value;
5. Si elimina il nodo puntato da `tmp`, con la [[017 Allocazione di memoria#FREE|free]];
6. Si riduce di 1 la dimensione della lista;
7. Si ritorna l'item del nodo eliminato.

![[c33c7394-0cb0-441a-87a1-891628235f5d.png]]

```C
//Rimuove e ritorna l'head della lista.

Item removeHead(List list){
	if (isEmpty(list)) 
		return NULL;

	struct node *tmp = list->head; //Puntatore al nodo Head
	list->head = tmp->next;
	Item item = tmp->value; //Item da ritornare

	free(tmp);
	(list->size)--;
	return item;
}
```

Tipica chiamata: `removeHead(list)`.

### OPERAZIONI DI RITORNO DI DATI
#### RITORNO DEL VALORE DELL'HEAD (getHead)
```C
//Ritorna il valore dell'Head della lista.

Item getHead(List list){
	if (isEmpty(list))
		return NULL;
	return list->head->value;
}
```

#### RITORNO DELLA DIMENSIONE DELLA LISTA (sizeList)
```C
//Ritorna la dimensione della lista.

int sizeList(List list){
	return list->size;
}
```

### RICERCA DI UN ITEM IN UNA LISTA (searchList)
Per ricercare un item in una lista si utilizza il ciclo `for` per iterare fino a quando troviamo un nodo con il valore uguale all'item da cercare.
La funzione ritorna l'item trovato; questo per via di certe implementazioni.
Per ricevere la posizione si manda l'indirizzo della variabile `pos`.

```C
//Ricerca un item in una lista. Modifica la variabile pos. Restituisce l'item trovato, se non viene trovato questo è NULL.

Item searchList(List l, Item item, int *pos){
	struct node *p;
	*pos = 0;
	
	for(p = l->head; p != NULL; p = p->next, (*pos)++)
		if (compareItem(item, p->value) == 0)
			return p->value; 
	
	*pos = -1;
	return NULL; 
}
```

Tipica chiamata: `searchList(list, item, &pos)`.

### ELIMINAZIONE DI UN ITEM (removeItem)
Viene ritornato l'item eliminato.
Per eliminare un nodo con un certo item nella lista, si procede in questo modo:
1. Si vede se la lista è vuota:
	- Se la lista è vuota ritorna `NULL`.
2. Si itera con i due puntatori nodi `prev` e `p` (che rappresenta il nodo corrente) fino a quando si trova il nodo con l'item da eliminare:
	- Se l'item si trova in testa alla lista si usa e si fa ritornare la [[020 Liste nel C#ELIMINAZIONE IN TESTA removeHead|removeHead]].
	- Altrimenti si usa l'algoritmo per eliminare un nodo (riducendo di 1 la dimensione della lista).
	- Se l'item non si trova nella lista, ritorna `NULL`.

```C
//Rimuove un elemento con un certo item da una lista. Restituisce l'elemento eliminato.

Item removeItem(List list, Item item) {
	if (isEmpty(list))
		return NULL;

	struct node *p, *prev;
	Item it;
	
	for(p=list->head; p!=NULL; prev=p, p=p->next)
		if(compareItem(p->value, item) == 0){
			if (p == list->head)
				return removeHead(list);
			else {
				prev->next = p->next;
				it = p->value;
				free(p);
				(list->size)--;
				return it;
			}
		}
	
	return NULL;
}
```

Tipica chiamata: `removeItem(list, item)`.

### ELIMINAZIONE DI UN QUALSIASI NODO (removePos)
Viene ritornato l'item eliminato.
Per eliminare un qualsiasi nodo nella lista, si procede in questo modo:
1. Si vede se la lista è vuota:
	- Se la lista è vuota ritorna `NULL`.
2. Si vede se la posizione da eliminare si trova nella lista:
	- Se non lo è, ritorna `NULL`.
3. Trovare il nodo precedente al nodo che deve essere eliminato:
	- Se il nodo da eliminare è l'Head si usa e si fa ritornare la [[020 Liste nel C#ELIMINAZIONE IN TESTA removeHead|removeHead]].
4. Cambia il `next` del nodo precedente;
5. Si libera il nodo da eliminare;
6. Si riduce di 1 la dimensione della lista.

![[9021e6a0-b7b8-440b-925b-eb04f06cd51d.png]]

```C
//Rimuove una posizione specifica della lista. Restituisce l'elemento eliminato.

Item removePos(List list, int pos) {
	if (isEmpty(list))
		return NULL;
	
	if (pos < 0 || pos > list->size)
		return NULL;

	struct node *p, *prev;
	Item it;
	int cont = 0;

	for(p=list->head; p!=NULL; prev=p, p=p->next, cont++)
		if(pos == cont){
			if (p == list->head)
				return removeHead(list);
			else {
				prev->next = p->next;
				it = p->value;
				free(p);
				list->size--;
				return it;
			}
		}
	
	return NULL;
}
```

Tipica chiamata: `removePos(list, pos)`.

### INSERIMENTO DI UN NODO IN UNA QUALSIASI POSIZIONE (insertItem)
Ritorna 1 se l'item è stato inserito, 0 altrimenti.
Per aggiungere un nodo non in testa alla lista, si procede in questo modo:
1. Si vede se la posizione in cui inserire si trova nella lista:
	- Se non lo è, ritorna 0.
2. Si vede se la posizione in cui inserire è l'Head della lista:
	- Se lo è, si usa la [[020 Liste nel C#INSERIMENTO IN TESTA addHead|addHead]] e si ritorna 1.
3. Trovare il nodo precedente al nodo rappresentato da `pos`;
4. Si crea e si alloca spazio ad un nuovo nodo E;
5. Inserisci l'item del nuovo nodo;
6. Cambia il next del nodo precedente per agganciarlo ad E;
7. Cambia il next di E per agganciarlo al prossimo nodo C;
8. Si aumenta di 1 la dimensione della lista.
9. Si ritorna 1.

![[084ecda4-1520-4eab-a3f2-266308da4292.png]]

```C
//Inserisce un item ad una posizione specifica. Restituisce 0 se la posizione non è valida.

int insertItem(List list, Item item, int pos) {
	if (pos < 0 || pos > list->size)
		return 0;
	
	if (pos == 0) {
		addHead(list, item);
		return 1;
	}
	
	struct node *p, *new;
	int cont = 0;

	for (p = list->head; cont < pos-1; p = p->next, cont++)
		;
	
	new = malloc(sizeof(struct node));
	new->value = item;
	p->next = new;
	new->next = p->next;
	list->size++;
	return 1; 
}
```

Tipica chiamata: `insertItem(list, item, pos)`.

### INSERIRE IN CODA (insertTail)
Per inserire un elemento in coda, si usa la [[020 Liste nel C#INSERIMENTO DI UN NODO IN UNA QUALSIASI POSIZIONE insertItem|insertItem]] a cui si passa come posizione la dimensione della lista.

```C
//Inserisce un elemento alla fine della lista.

int insertTail(List list, Item item){
	return insertItem(list, item, list->size);
}
```

Tipica chiamata: `insertTail(list, item)`.

### REVERSE DI UNA LISTA (reverseList)
Per riscrivere una lista al contrario, invece di spostare fisicamente i nodi, si modificano i collegamenti facendoli ruotare:
1. Impostano i nodi puntatore: `curr` all'Head della lista, `prev` e `next` a `NULL`;
2. Cicla con un `while` fino a quando `curr` è `NULL`;
3. Si imposta il nodo `next` al prossimo nodo di `curr`;
4. Scambia i collegamenti facendoli ruotare;
5. Sposta `prev` e `curr` avanti di un nodo;
6. Dopo aver finito il ciclo, imposta come nuovo Head della lista `prev`.

![[RGIF2.mp4]]

```C
//Riscrive la lista al contrario.

void reverseList(List list){
	struct node *curr = list->head;
	struct node *prev = NULL; 
	struct node *next = NULL;
	
	while (curr != NULL){
		next = curr->next; //Memorizza il prossimo nodo
		curr->next = prev; //Qui avviene lo scambio
		prev = curr; //Sposta prev e curr avanti di un nodo
		curr = next;
	}

	list->head = prev; 
	//Imposta l'Head della lista all'ultimo elemento
}
```

Tipica chiamata: `reverseList(list)`.

### CLONAZIONE DI UNA LISTA (cloneList)
Ritorna una lista clonata.
Per clonare una lista si procede in questo modo:
1. Si crea una nuova lista con [[020 Liste nel C#CREAZIONE DI UNA LISTA newList|newList]];
2. Si itera con un ciclo `for`:
	- Ad ogni iterata si usa la [[020 Liste nel C#INSERIRE IN CODA insertTail|insertTail]] per inserire in coda l'item del nodo corrente.
3. Ritorna la nuova lista.

```C
//Clona una lista e la fa ritornare.

List cloneList(List list) {
	List clone = newList();
	struct node *p;
	
	for(p=list->head; p!=NULL; p=p->next)
		insertTail(clone, p->value);
	
	return clone;
}
```

Tipica chiamata:
```C
List cloned_list = cloneList(list);
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