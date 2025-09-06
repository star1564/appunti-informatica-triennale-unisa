---
aliases: 
tags:
  - corsi/informatica/programmazione_strutture_dati
paragrafo: Algoritmi di Ordinamento
cssclasses:
  - 
---
Per ordinare un array con il **Quick Sort** si usa la strategia del *Divide et Impera*, che si basa sulla [[028 Ricorsione (Programmazione)|Ricorsione]].

Si sceglie un elemento come **Pivot** e creano *Partizioni* attorno ad esso.

Ci sono diverse posizioni da cui si possono prendere i pivot:
- Si prende sempre il primo elemento come pivot;
- Si prende sempre l'ultimo elemento come pivot;
- Si prende sempre l'elemento mediano come pivot;
- Si prende sempre un elemento casuale come pivot;
- Si considerano il primo elemento, l'ultimo e quello mediano, per poi prendere quello intermedio tra i tre come pivot.

Svilupperemo la prima implementazione.

Il processo principale dell'algoritmo è la funzione `partition`.
Il suo obbiettivo, dato un array con l'indice di inizio e di fine, 
è quello di: 
- Scegliere un elemento come *pivot*; 
- Mettere il *pivot* nella posizione corretta;
- Mettere a sinistra gli elementi $\color{#61AFEF}\leq$ *pivot*; 
- Mettere a destra gli elementi $\color{#61AFEF}\geq$ *pivot*.

In generale, invece:
- **Divide**: partiziona l'array `A[inizio ... fine]` in due sottoarray SX e DX rispetto ad un pivot `x`. Il pivot si troverà in posizione `q`, data dalla funzione `partition`.
- **Impera**:
	- Base della ricorsione: Se l'array ha 1 elemento, è ordinato;
	- Quicksort sul sottoarray SX `A[inizio ... q]`;
	- Quicksort sul sottoarray DX `A[q+1 ... fine]`.


<font color="orange">Esempio di `partition`</font>, con pivot preso dall'elemento mediano:

![[8988c5b2-8a41-45a3-b0d0-7e2ddea1ca28.jpg]]

Spostiamo i puntatori, facendo crescere le due regioni, fino a quando è possibile.
Quando entrambi i puntatori non possono avanzare *scambiamo gli elementi*.

![[Pasted image 20220520103112.png]]

A questo punto i due puntatori possono riprendere ad avanzare.
La procedura termina quando i due puntatori si incrociano.

![[Pasted image 20220520103158.png]]

Adesso il $17$ è nella *posizione corretta*.

Le prossime chiamate della quicksort saranno:
- `qSort(arr, inizio zona blu, fine zona blu)`;
- `qSort(arr, inizio zona gialla, fine zona gialla)`.

## IMPLEMENTAZIONE
Si usano 3 funzioni per rappresentare il quicksort. Si usa anche la [[1002.F Swap (int)|swap]].

```C
//Prende il primo elemento come pivot e lo mette alla posizione corretta nell'array ordinato, posiziona gli elementi più grandi a destra e quelli più piccoli a sinistra del pivot.

int partition(int *arr, int start, int end) {
    int pivot = arr[start]; //Pivot è l'inizio
    int i = start - 1, j = end + 1;

    while (1) {
//do: Esegue prima il corpo e poi controlla la condizione, se necessario ripete il corpo e ricontrolla.
        
        do { //Puntatore rosso
            j--;
        } while (arr[j] > pivot);
        
        do { //Puntatore blu
            i++;
        } while (arr[i] < pivot);

        if (i >= j) //Se i e j si incrociano
            return j;
            
        swap(arr + j, arr + i);
    }
}

//Funzione usata per la ricorsione.
void qSort(int *arr, int start, int end) {
    //Caso Base: start >= end
    
    if (start < end) {
        int q = partition(arr, start, end);
        qSort(arr, start, q);
        qSort(arr, q + 1, end);
    }
}

//Funzione chamata dal cliente.
void quickSort(int *arr, int size) {
    qSort(arr, 0, size - 1);
}
```

## ANALISI [[030 Complessità Computazionale|COMPLESSITÀ]]
Trattandosi di un algoritmo di ordinamento basato sui confronti, analizziamo il numero di confronti in funzione del numero $n$ di oggetti da ordinare.

Tutti i confronti sono eseguiti nella funzione `partition`.

Quando `partition` è eseguita su un sottoarray di lunghezza $m$, vengono eseguiti $m$ confronti (ogni elemento è confrontato una volta con il pivot).

Se l'array da ordinare viene partizionato in due array di dimensione $r$ e $n-r$ abbiamo che il numero di confronti $T(n)$ soddisfa alla ricorrenza:
$$T(n)=\begin{cases} \textcolor{#CC241D}0, \quad se\ n=1 \\ \textcolor{#61AFEF}{T(r)+T(n-r)+n}, \quad se\ n>1 \end{cases}$$

Questa ricorrenza non è del tipo che si può ricondurre ai casi presentati. 

L'efficienza è legata al bilanciamento delle partizioni.
Mentre il bilanciamento è legato alla scelta del pivot.

Studieremo il caso peggiore e il caso migliore, vedendo anche il caso medio.
A ogni passo `partition` ritorna:
- caso *peggiore*, un array da $1$ elemento e l'altro da $n-1$;
- caso *migliore*, due array da $n/2$ elementi;
- caso *medio*, due array di dimensioni diverse.

### CASO PEGGIORE
Quando il pivot è il *minimo o il massimo* dell'array.
Oppure quando l'array è già ordinato.

Equazione di ricorrenza:
$T(n) = T(\textcolor{orange}{n-1}) + n,\quad n ≥ 2$
$T(1) = 1$

Abbiamo il [[033 Valutazione della complessità nella Ricorsione|Caso 2.a]]. Quindi:
$\color{#CC241D}T(n) = \Theta(n^2)$

### CASO MIGLIORE
Quando ad ogni passo l'array viene partizionato in due regioni uguali.

Equazione di ricorrenza:
$T(n) = \textcolor{orange}2T(\textcolor{orange}{n/2}) + n,\quad n ≥ 2$        Caso 2.b
$T(1) = 1$

Abbiamo il Caso 2.b. Quindi:
$\color{#CC241D}T(n) = \Theta(n\ log(n))$

![[Pasted image 20220520110837.png|600]]

### CASO MEDIO
Studiamo il caso in cui si alternano una partizione cattiva ed una buona.

Nei livelli dispari il problema viene spezzato in $[1,n-1]$, nei livelli pari in $[n/2, n/2]$.

![[Pasted image 20220520110721.png]]

Per scendere di un livello nell'albero il costo è $\Theta(n)$.
L'altezza dell'albero è circa $2log(n)$ quindi il costo complessivo è $\color{#CC241D}\Theta(n\ log(n))$.

In generale, se le partizioni sono spesso molto sbilanciate, il costo può risultare molto alto.

Se le partizioni non sono "troppo sbilanciate" si ha un costo $O(n\ log(n))$ cioè asintoticamente uguale al caso ottimo.

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