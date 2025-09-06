---
aliases: 
tags:
  - corsi/informatica/ingegneria_software
paragrafo: Implementazione
cssclasses:
  - 
---
L'ottimizzazione del [[028 Object Design Document|Modello di Object Design]] permette di migliorare la *performance* del sistema.

Ci sono quattro attività.

## Ottimizzare cammini di accesso
### Attraversamento Ripetuto di Associazioni
Per identificare i percorsi di accesso inefficienti, è necessario *identificare le operazioni che vengono invocate spesso* ed esaminare, con l'aiuto di un [[UML 2 - Diagramma di Sequenza|diagramma di sequenza]], il sottoinsieme di queste operazioni che richiede l'attraversamento di più associazioni. 

```java
// Inefficiente
metodo().metodo2().metodo3()...
```

Le operazioni frequenti non dovrebbero richiedere molti attraversamenti, ma dovrebbero avere una *connessione diretta* tra l'oggetto interrogante e l'oggetto interrogato.
Se questa connessione diretta manca, è necessario aggiungere un'associazione tra questi due oggetti. 

### Associazioni con molteplicità a molti
Per le associazioni con molteplicità [[UML 5 - Diagramma di Classi#Associazione|a molti]], si dovrebbe cercare di ridurre il tempo di ricerca *riducendo le "molte" a "una"*. 

Se non è possibile ridurre la molteplicità dell'associazione, si dovrebbe considerare la possibilità di ordinare o indicizzare gli oggetti sul lato "molti" per ridurre il tempo di accesso.

### Attributi con posizionamento errato
Durante la fase di [[013 Analisi dei Requisiti|analisi]], potrebbero essere identificati delle classi che non hanno molta utilità (ad esempio se hanno solamente setter e getter).

In questo caso si dovrebbe *unire gli attributi e metodi nelle classi chiamanti*.

## Trasformare oggetti in attributi
Dopo che il modello è stato ristrutturato e ottimizzato diverse volte, alcune delle sue classi potrebbero avere *pochi attributi o metodi*. 
Queste classi, quando sono associate solo a un'altra classe, possono essere **collassate in un attributo** all'interno della classe associata, riducendo così la complessità del modello.

Questo avviene tramite il [[030 Concetti di Mapping#Refactoring|Refactoring]].

![[Pasted image 20240125113215.png]]

## Ritardare le operazioni costose
Di solito, alcuni oggetti richiedono computazioni costose, per esempio il caricamento di tutti i pixel di un'immagine. Durante il caricamento dell'immagine è possibile mostrare una immagine di caricamento temporanea fino a quando il calcolo non è concluso.

Questo è possibile attraverso il [[025 Design Pattern#Proxy Pattern|Proxy Design Pattern]].

![[Pasted image 20240125113712.png|670]]

## Memorizzare il risultato delle operazioni costose
Se alcuni metodi vengono chiamati più volte, ma il risultato cambia in modo poco frequente, è possibile ridurre il numero di computazioni *conservando questo risultato* in uno spazio di memoria opportuno, ad esempio un attributo privato.


___
[[000 Indice IS|↖ Ritorna all'indice ↖]]

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