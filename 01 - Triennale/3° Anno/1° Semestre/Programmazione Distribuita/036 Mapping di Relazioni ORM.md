---
aliases: 
tags:
  - corsi/informatica/programmazione_distribuita
paragrafo: Java Persistence API
cssclasses:
  - 
---
## Table Reference
In [[033 Java Persistence API|JPA]] quando si ha una associazione fra una classe ed un altra, nel database si ottiene una **Table Reference** che può essere modellata attraverso una *foreign key (join column)* oppure una *join table*.

### Join Column
Nel caso della join column, in java la classe `Customer` possiede semplicemente un attributo `Address`, ma nel db relazionale si ha una foreign key nella tabella `CUSTOMER` che punta ad una riga della tabella `ADDRESS`.

![[Pasted image 20231114171902.png|700]]
### Join Table
Nel caso della join table, si crea una tabella a parte che mantiene le relazioni.

![[Pasted image 20231114171924.png|700]]

## Tipologie di Mapping Relazionale
Due classi possono avere tre tipi di **associazioni**:
- *Unidirezionale*, un oggetto può navigare verso un altro;
- *Bidirezionale*, un oggetto può navigare in entrambe le direzioni;
- *Con Molteplicità*, un oggetto della classe di partenza può riferire più oggetti della classe di destinazione.

![[Pasted image 20231114171637.png|700]]

La maggior parte delle entità hanno necessità di referenziare, o avere relazioni con altre [[034 Entità JPA|Entità]]. JPA permette di mappare associazioni cosicché una entità possa essere linkata ad un'altra in un modello relazionale.

La *cardinalità* fra due entità può essere:
- *one-to-one*, con `@OneToOne`;
- *one-to-many*, con `@OneToMany`;
- *many-to-one*, con `@ManyToOne`;
- *many-to-many*, con `@ManyToMany`;

Ogni annotazione può essere usata in modo unidirezionale o bidirezionale.

### Mapping Unidirezionale
In una relazione unidirezionale, una entità `Customer` ha un attributo di tipo `Address`. La relazione è unidirezionale perché si naviga da un lato verso l'altro. L'entità `Customer` rappresenta il proprietario ella relazione.

![[Pasted image 20231115103132.png|500]]

### Mapping Bidirezionale
In una relazione bidirezionale, non si sa chi è il proprietario della relazione, dato che si può navigare in entrambi i versi.

![[Pasted image 20231115103702.png|500]]

In questo caso si utilizzano le annotazioni di cardinalità, specificando tra parentesi `mappedBy`. 

> [!example]- <font color="orange">Esempio</font>
>Entrambe le entità hanno un collegamento verso l'altra, specificato con `@OneToOne`. L'elemento `mapped` indica che la join column è specificata all'altro lato della relazione. Infatti, nell'altro lato, l'entità `Customer` definisce la join column usando l'annotazione `@JoinColumn` e rinomina la foreign key in `address_fk`.
>
>![[Pasted image 20231115103921.png|600]]


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