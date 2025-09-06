---
aliases: 
tags:
  - corsi/informatica/programmazione_strutture_dati
paragrafo: Abstract Data Type
cssclasses:
  - 
---
Una **Tabella Hash** è una struttura dati usata per *mettere in corrispondenza* una data chiave con un dato valore.

Le chiavi ed i valori possono appartenere a diversi tipi di primitivi o pseudo-generici (come gli Item).

> [!example]+ <font color="orange">Esempio in JavaScript</font>
> È possibile associare l'età (intero) ad una persona utilizzando il nome (stringa).
> ```js
>età["francesco"] = 23
>età["antonio"] = 24
>
>età["francesco"] //Output: 24
>```

### INDIRIZZAMENTO DIRETTO
Se l'universo delle chiavi *è piccolo* e le chiavi sono *intere*, allora è sufficiente utilizzare una **Tabella ad Indirizzamento Diretto**.

Definiamo con $U$ l'universo di tutte le possibili chiavi da utilizzare, mentre con $K$ definiamo l'insieme delle chiavi effettivamente utilizzate.

Una Tabella ad Indirizzamento Diretto corrisponde al concetto di array. Infatti *ad ogni chiave possibile corrisponde una posizione*, o slot, nella tabella.

Una tabella restituisce il dato memorizzato nello slot con posizione indicata tramite la chiave. Questo avviene in tempo costante $O(1)$.

![[Pasted image 20220520160509.png]]

> [!warning] Universo Grande delle chiavi
> Se le chiavi *non sono intere* e/o l'universo delle possibili chiavi è *molto grande* non è possibile o conveniente utilizzare il metodo delle tabelle ad indirizzamento diretto.
>
> Quindi questa soluzione non è pratica quando $|K| << |U|$ (molto minore).
>
> Può non essere possibile a causa della limitatezza delle risorse di memoria.
> Oppure non può essere conveniente perché se il numero di chiavi effettivamente utilizzato è piccolo si hanno tabelle quasi vuote, sprecando spazio.

## TABELLE HASH
![[Funzione Hash]]


Con il metodo di indirizzamento diretto, un elemento con chiave $k$ viene memorizzato nella tabella in posizione $k$, ma questo, come abbiamo visto prima, può portare problemi.

Invece con il **Metodo Hash**, un elemento con chiave $k$ viene memorizzato nella tabella in posizione $h(k)$, dove $h()$ è chiamata *funzione hash*.

Lo scopo della funzione hash è quello di definire una *corrispondenza* tra l'universo $U$ delle chiavi e le posizioni di una tabella hash $T[0...m-1]$, con $m<<|U|$. 
È definita in questo modo:
$$h:U\to\{0,1,...,m-1\}$$
### FUNZIONE HASH
La funzione hash *calcola l'indice* della tabella hash in cui andare a mettere un elemento (**Entry**) in base ad una chiave (**Key**).

Questa dovrebbe utilizzare tutte le cifre della chiave per produrre un valore hash.

#### FUNZIONE PER GLI INTERI
Si usa molto spesso il *metodo di divisione*.
$$h(k)=k\cdot mod(m)$$
Ovvero, il valore hash è il resto della divisione di $k$ per $m$.

#### FUNZIONE PER LE STRINGHE
Per convertire una stringa in un numero naturale si considera la stringa come un numero in base $128$; esistono cioè 128 simboli diversi per ogni cifra di una stringa. 

È possibile stabilire una conversione fra ogni simbolo e i numeri naturali. La conversione viene poi fatta nel modo tradizionale, utilizzando la [[012 Caratteri non numerici#^d63d3e|tabella ASCII]], partendo con gli esponenti più grandi a sinistra per poi diminuire di uno andando verso destra.

> [!example]+ <font color="orange">Esempio</font>
> Convertire la stringa "pt".
> "pt"$=p\cdot 128^1 + t\cdot 128^0 = 112\cdot128+116\cdot1=14452$

#### CHIAVI MOLTO GRANDI
Spesso capita che le chiavi abbiano dimensione tale da non poter essere rappresentate come numeri interi per una data architettura. 

Un modo alternativo di procedere è di utilizzare una funzione hash modulare, trasformando un pezzo di chiave alla volta.

Per fare questo basta sfruttare le proprietà aritmetiche dell'operazione modulo e usare la regola di Horner per scrivere la conversione.

## COLLISIONI
Necessariamente la funzione hash *non può essere [[020 Applicazioni Iniettive|iniettiva]]*, ovvero due chiavi distinte possono produrre lo stesso valore hash.

Quando accade che $h(k_1)=h(k_2)$, dove $k_1\neq k_2$, si verifica una **Collisione**.

Per avere una tabella efficiente, bisogna:
- Minimizzare il numero di collisioni;
- Gestire le collisioni residue, quando avvengono.

### RISOLUZIONE DELLE COLLISIONI
Si hanno due strategie per risolvere le collisioni:
- Il metodo di Concatenazione;
- Il metodo di Indirizzamento Aperto.

#### METODO DI CONCATENAZIONE
L'idea è di mettere tutti gli elementi che collidono in una [[020 Liste nel C|lista concatenata]].
Quindi, alla posizione j-esima di una tabella si ha un puntatore alla testa della j-esima lista (oppure un puntatore a `NULL` se non ci sono elementi).
![[Pasted image 20220520175136.png]]

#### METODO DI INDIRIZZAMENTO APERTO
L'idea è quella di memorizzare tutti gli elementi nella stessa tabella ed in caso di collisione si memorizza l'elemento nella posizione *successiva*. Quindi si genera un nuovo valore hash fino a trovare una posizione vuota dove inserire l'elemento.

Si va ad estendere la funzione hash perché generi non solo un valore hash, ma una sequenza di scansione.
$$h:U\times\{0,1,...,m-1\}\to\{0,1,...,m-1\}$$
Cioè, data una chiave $k$, si parte dalla posizione 0 e si ottiene $h(k, 0)$, la seconda posizione da scansionare sarà $h(k,1)$, e così via $h(k,i)$.
Alla fine si ottiene una sequenza $$<h(k,0), h(k,1), ..., h(k,m-1)>$$

## IMPLEMENTAZIONE
### KEY
La **Key** servirà per decidere a quale indirizzo si inserirà un valore nella tabella hash.

![[78770683-25f9-4e95-b5ee-20be88231181.jpg]]

> [!tip]- key.h
> ```C
>typedef void *Key;
>
>//Funzioni...
>```

> [!tip]- key.c
>```C
>#include <stdlib.h>
>#include <string.h>
>#include "key.h"
>#define MAX_STRING 50
>
>//Funzioni...
>```


---
> [!note]- Controllo per gli uguali (equals)
> Per vedere se due chiavi stringhe sono uguali, si creano due [[001 Puntatori|puntatori]] `char` che puntano alle chiavi, poi si usa la `strcmp`.
>```C
>int equals(Key key1, Key key2){
>	char *p1, *p2;
>	p1 = key1;
>	p2 = key2;
>
>	if (strcmp(p1,p2) == 0)
>		return 1;
>	else
>		return 0;
>}
>```

> [!note]- Funzione Hash per una stringa (hashValue)
> La funzione hash per una stringa si calcola in questo modo.
>```C
>int hashValue(Key key, int i){
>	int h = 0, a = 128;
>	char *v=key;
>	for (; *v != '\0'; v++)
>		h = (h*a + *v) % i;
>	return h;
>}
>```

^96c2d2

> [!note]- Inserire una Key (inputKey)
> Serve per inserire e ritornare una key.
>```C
>Key inputKey(){
>	char *v;
>	v = malloc(sizeof(char)*MAX_STRING);
>	scanf("%s", v);
>	return v;
>}
>```

> [!note]- Stampare una Key (outputKey)
>```C
>void outputKey(Key key){
>	char *v;
>	v = key;
>	printf("%s", v);
>}
>```

---
### ENTRY
La **Entry** è l'insieme delle coppie (chiave, valore).

![[8f58a7fe-0f77-4f34-837a-f20a1d089a4a.jpg]]

> [!tip]- entry.h
> ```C
>#include "key.h"
>
>typedef struct entry *Entry;
>
>//Funzioni...
>```

> [!tip]- entry.c
> Serve una struttura per contenere la key e l'item.
>```C
>#include <stdlib.h>
>#include "entry.h"
>
>struct entry {
>	Key key;
>	Item value;
};
>
>//Funzioni...
>```

---
> [!note]- Creazione di una Entry (newEntry)
> Per unire una key ed un valore in una struttura entry si usa la [[017 Allocazione di memoria#MALLOC|malloc]].
>```C
>Entry newEntry(Key key, Item item){
>	Entry e = malloc(sizeof(struct entry));
>	e->key = key;
>	e->value = item;
>	return e;
>}
>```

> [!note]- Ritorna la Key di una Entry (getKey)
>```C
>Key getKey(Entry entry){
>	if(entry != NULL)
>		return entry->key;
>	else
>		return NULL;
>}
>```

> [!note]- Ritorna l'Item di una Entry (getValue)
>```C
>Key getValue(Entry entry){
>	if(entry != NULL)
>		return entry->value;
>	else
>		return NULL;
>}
>```

---
### HASH-TABLE
La **Hash-Table** è "l'array" che contiene tutte le entry.
![[c0483342-5299-4d1d-b613-335ab36e90c7.jpg]]


> [!tip]- hashtable.h
> ```C
>#include "key.h"
>#include "entry.h"
>
>typedef struct hashtable *HashTable;
>
>//Funzioni...
>```

> [!tip]- hashtable.c
> Serve una struttura per contenere la lista delle entries e la sua dimensione.
>```C
>#include <stdlib.h>
>#include "hashtable.h"
>#include "list.h"
>
>struct hashtable {
>	int size;
>	List *entries;
>};
>
>//Funzioni...
>```

---
> [!note]- Creazione di una Tabella Hash (newHashtable)
> Creare una tabella hash con una lista, a questa funzione gli viene data un `int size`, ovvero la dimensione massima della tabella hash. 
> Ad ogni nodo della lista `entries` si crea una nuova lista per gestire le collisioni.
>```C
>HashTable newHashtable(){
>	int i;        
>	HashTable t = malloc(sizeof(struct hashtable));
>	t->size=size;
>	t->entries=malloc(sizeof(List)*size);
>	
>	for(i=0; i<size; i++)
>		t->entries[i] = newList();
>	
>	return t;
>}
>```

> [!note]- Inserire una Entry in una Tabella Hash data una key (insertHash)
> Per inserire una entry in una tabella hash si deve prima calcolare il suo indice con la funzione [[026 Tabelle Hash#^96c2d2|hashValue]], poi si usa la `addHead` nella lista.
>```C
>int insertHash(Entry entry){
>	int index = hashValue(getKey(entry), t->size);
>	addHead(t->entries[index], entry);
>	return index;
>}
>```

> [!note]- Eliminare una Entry da una Tabella Hash data una key (deleteHash)
> Per eliminare una entry in una tabella hash si calcola prima l'indice con `hashValue` poi si usa la `removeListItem`:
>```C
>Entry deleteHash(Entry entry){
>	Key k = getKey(entry);
>	int index = hashValue(k, t->size);
>	Entry e = newEntry(k, NULL);
>	return removeListItem(t->entries[index], e);
>}
>```


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