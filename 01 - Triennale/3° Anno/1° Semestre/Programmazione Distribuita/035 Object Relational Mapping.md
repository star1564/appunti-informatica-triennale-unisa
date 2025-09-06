---
aliases:
  - Object-Relational Mapping
  - Object Relational Mapping
  - ORM
  - Configuration by Exception
tags:
  - corsi/informatica/programmazione_distribuita
paragrafo: Java Persistence API
cssclasses:
  - 
---
La maggior parte dei dati che una applicazione manipola vengono conservati, ottenuti ed analizzati all'interno di un [[001 Introduzione a Basi di Dati|database]], assicurando la *persistenza dei dati*. Però in OOP, quando il [[014 Garbage Collector|Garbage Collector]] decide di eliminare un elemento dalla memoria, è perduto per sempre.

Ci sono grosse differenze tra i linguaggi object oriented ed il modo in cui funzionano i database. 

>Per mettere insieme i due mondi esiste l'**Object-Relational Mapping** (*ORM*). Il principio dell'ORM è quello di *delegare a strumenti o framework* esterni (nel nostro caso, [[033 Java Persistence API|JPA]]) il compito di creare una *corrispondenza tra oggetti e tabelle*. 
>
>Quindi è un meccanismo che permette di mappare oggetti in dati memorizzati in un database.

Il mondo delle classi, degli oggetti e degli attributi può quindi essere *mappato* su database relazionali costituiti da tabelle contenenti righe e colonne. 

![[Pasted image 20231115095516.png]]

La mappatura offre una visione orientata agli oggetti agli sviluppatori, che possono utilizzare in modo trasparente le [[034 Entità JPA|Entità]] invece delle tabelle.

### Metadati
JPA mappa gli oggetti in un database attraverso i *[[Metadato|metadati]]*.

Ad ogni entità sono associati metadati che descrivono il mapping. I metadati consentono al persistence provider di riconoscere un'entità e di interpretare la mappatura.

### Configuration by Exception
Java EE 5 ha introdotto l'idea della **Configuration by Exception** (o "Convention Over Configuration"), una importante tecnica in cui  il [[021 Container Java EE|container]] o il provider *applica le regole predefinite*, a meno che non venga fornita una *configurazione personalizzata* (l'eccezione alla regola). 

Ciò consente di scrivere il minimo di codice per far funzionare l'applicazione, affidandosi alle impostazioni predefinite del contenitore e del provider. Se non si vuole che il provider applichi le regole predefinite, è possibile personalizzare la mappatura in base alle proprie esigenze, utilizzando i metadati.

Attraverso questa tecnica, un POJO può diventare una Entità specificando una configurazione personalizzata, attraverso [[Alcune delle Annotazioni in CDI|annotazioni]] oppure [[022 Deployment Descriptor|Deployment Descriptor XML]].

> [!example]- <font color="orange">Esempio</font>
>Si vuole cambiare il nome della tabella di una entità. Quindi si inserisce sopra la classe l'annotazione
>```Java
>@Table(name = "nomeTabella")
>```

### Mapping di Entità
Questo è il comportamento di default in JPA riguardo il mapping:
- Il nome dell'[[034 Entità JPA|Entità]] in maiuscolo diventa il nome della tabella.
- Il nome degli attributi diventano i nomi delle colonne.
- Vengono mappati i tipi primitivi di Java ai tipi di dati relazionali.

Le informazioni sul database sono fornite dal file `persistence.xml`.

Questo mapping di default segue il principio del configuration by exception. Senza annotazioni, il Book entity verrebbe trattato come POJO e pertanto non come classe persistente.

![[Pasted image 20231114130106.png|350]]

___
[[000 Indice PD|↖ Ritorna all'indice ↖]]

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