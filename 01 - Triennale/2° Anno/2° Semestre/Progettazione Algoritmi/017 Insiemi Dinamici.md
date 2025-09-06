---
aliases: 
tags:
  - corsi/informatica/progettazione_algoritmi
paragrafo: Alberi e Grafi
cssclasses:
  - 
---
>Gli **Insiemi Dinamici** sono una struttura dati che consente di memorizzare un insieme di elementi che *possono variare nel tempo*. A differenza degli insiemi statici, dove l'insieme di elementi è fissato durante l'inizializzazione, gli insiemi dinamici consentono l'*aggiunta, la modifica e la rimozione di elementi* durante l'esecuzione del programma.

Normalmente, ciascun elemento è rappresentato da un **oggetto**, i cui **campi** possono essere manipolati ed esaminati se abbiamo un *puntatore all'oggetto*.
Questi sono i campi dell'oggetto:
- *Chiave*: per la identificazione dell'oggetto;
	- I valori delle chiavi sono presi da un *insieme ordinato* (come la sequenza $1,2,\dots,n$).
- *Dati Satelliti*: i dati veri e propri che possono essere memorizzati in diversi altri campi dell'oggetto.

Le **operazioni** su insiemi dinamici sono raggruppati in due categorie (con delle operazioni di esempio):
- Operazioni di *Interrogazione*, che ritornano informazioni circa l'insieme:
	- *`MINIMUM(S)`*/*`MAXIMUM(S)`*: ritorna l'elemento $S$ con la chiave di minimo/massimo valore;
- Operazioni di *Modifica*, che cambiano l'insieme:
	 - *`INSERT(S, x)`*: inserisce un elemento $x$ nell'insieme $S$;
	 - *`DELETE(S, x)`*: elimina un elemento $x$ dall'insieme $S$;
	 - *`EXTRACT-MIN(S)`*/*`EXTRACT-MAX(S)`*: ritorna l'elemento di $S$ con la chiave di minimo/massimo valore e lo cancella da $S$;
	 - *`DIMINUISCI-CHIAVE(S, x, k)`*: decrementa il valore della chiave dell'elemento puntato da $x$ ad un nuovo valore $k$;
	 - *`AUMENTA-CHIAVE(S, x, k)`*: aumenta il valore della chiave dell'elemento puntato da $x$ ad un nuovo valore $k$.

Le strutture dati che supportano le operazioni con il `MINIMUM`/`MIN` sono dette **(min)code a priorità**, mentre quelle che supportano operazioni con il `MAXIMUM`/`MAX` sono dette **(max)code a priorità**. ^37af8c

---
### Implementazioni
Le seguenti implementazioni saranno efficaci per le *(min)code a priorità*, ma potranno essere usate con alcune modifiche anche per le (max)code a priorità. ^a9ced4

Ci sono principalmente due tipi di implementazioni: una attraverso [[020 Liste nel C#DEFINIZIONE|liste a puntatori]], l'altra attraverso un array ordinato.

Di seguito le complessità per la esecuzione di ciascuna operazione:

| *Operazione*                 | **Lista a Puntatori** | **Array Ordinato** |
| ---------------------------- | --------------------- | ------------------ |
| `INSERT(S, x)`               | $\Theta(1)$           | $\Theta(n)$        |
| `DELETE(S, x)`               | $\Theta(1)$           | $\Theta(n)$        |
| `MINIMUM(S)`                 | $\Theta(n)$           | $\Theta(1)$        |
| `EXTRACT-MIN(S)`             | $\Theta(n)$           | $\Theta(n)$        |
| `DIMINUISCI-CHIAVE(S, x, k)` | $\Theta(1)$           | $\Theta(n)$        |
| `AUMENTA-CHIAVE(S, x, k)`    | $\Theta(1)$           | $\Theta(n)$        |

La complessità lineare di alcune operazioni non è accettabile dal punto di vista pratico. Pertanto introduciamo una struttura dati più efficiente, lo [[018 Heap|Heap]].

___
[[000 Indice PA|↖ Ritorna all'indice ↖]]

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