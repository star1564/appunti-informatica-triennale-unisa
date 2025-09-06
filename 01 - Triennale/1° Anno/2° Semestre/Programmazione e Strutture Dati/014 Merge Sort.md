---
aliases: 
tags:
  - corsi/informatica/programmazione_strutture_dati
paragrafo: Algoritmi di Ordinamento
cssclasses:
  - 
---
Per ordinare un array con il **Merge Sort** (e per risolvere piccoli problemi computazionali) si usa la strategia del *Divide et Impera* (Dividi e Conquista), che si basa sulla [[028 Ricorsione (Programmazione)|Ricorsione]].

Nel Divide Et Impera si hanno tre fasi:
- **Divide**, si procede alla *suddivisione dei problemi* in problemi di dimensione minore;
- **Impera**, i problemi vengono *risolti in modo ricorsivo*. Quando i sottoproblemi arrivano ad avere una dimensione sufficientemente piccola, essi vengono risolti direttamente tramite il *caso base*;
- **Combina**, si ricombina l'output ottenuto dalle precedenti chiamate ricorsive al fine di ottenere il risultato finale.

## ALGORITMO
### MERGE SORT
Si ha un vettore da ordinare.

- *Divide*: Questo vettore si divide in due sottovettori SX e DX rispetto al centro del vettore.
![[Pasted image 20220513105722.png|300]]

- *Impera*: Si esegue il merge sort sul sottovettore SX e poi su quello DX, con la condizione di terminazione (caso base) che la dimensione dell'array è 1 (quando p = r) oppure 0 (quando p > r).

- *Combina*: Si usa il merge per fondere i due sottovettori ordinati in un vettore ordinato e si estrae ripetutamente il minimo dei due sottovettori e lo si pone nella sequenza in uscita.

<font color="orange">Esempio</font>:
![[Pasted image 20220513105804.png|500]]
Si usa il merge, **partendo dal basso**, per ottenere gli array ordinati.

![[Pasted image 20220513105827.png|500]]

### MERGE
![[Pasted image 20220513105936.png]]

Scorriamo i due vettori `a1` (con dimensione `n1`) e `a2` (con dimensione `n2`) utilizzando due indici `i`, `j`, rispettivamente:
- Confrontiamo `a1[i]` e `a2[j]` fino a quando `i` $<$ `n1` $\land$ `j` $<$ `n2`:
	- Se `a1[i]` $\leq$ `a2[j]`: inseriamo `a1[i]` in `a` ed incrementiamo `i`;
	- Altrimenti: inseriamo `a2[j]` in `a` ed incrementiamo `j`.
- Riversiamo tutti gli elementi restanti di `a1` oppure `a2` all'interno di `a`.

<font color="orange">Esempio</font>:
I due array di partenza sono già ordinati.
![[Pasted image 20220513110612.png|500]]

## IMPLEMENTAZIONE
```C
void merge(int *a1, int *a2, int n1, int n2, int *a){
    int i = 0, j = 0, k = 0;
    int b[n1+n2];

	//Si inseriscono gli elementi nell'array finale
    for(; i < n1 && j < n2; k++){
        if(a1[i] <= a2[j]){
            b[k] = a1[i];
            i++;
        } 
        else {
            b[k] = a2[j];
            j++;
        }
    }

	//Si riversano tutti gli elementi rimasti
    for(; i < n1; i++, k++)
        b[k] = a1[i];
    
    for(; j < n2; j++, k++)
        b[k] = a2[j];

	//Si inseriscono gli elementi di b in a
    for(k = 0; k < n1+n2; k++)
        a[k] = b[k];
}

void mergeSort(int *arr, int n){
    if(n>1) { //Caso base n <= 1
        mergeSort(arr, n/2); //Sottoarray SX
        mergeSort(arr+n/2, n-n/2); //Sottoarray DX
        merge(arr, arr+n/2, n/2, n-n/2, arr); //Merge
    }       
}
```

## ANALISI [[030 Complessità Computazionale|COMPLESSITÀ]]
Equazione alle ricorrenze:
$T(x)=2T(n/2)+\Theta(n)\quad n\geq 2$
$T(1)=1$

Abbiamo il [[033 Valutazione della complessità nella Ricorsione|Caso 2.b]]. Quindi:
$\color{#CC241D}T(n) = \Theta(n\ log(n))$

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