---
aliases: 
tags:
  - corsi/informatica/programmazione_distribuita
paragrafo: Java Persistence API
cssclasses:
  - 
---
>Una **Persistence Unit** indica all'[[037 Entity Manager|Entity Manager]] il tipo di database da usare ed i *parametri per la connessione*, entrambi specificati nel file `persistence.xml`.

Le informazioni che è possibile inserire per ogni PU sono:
- *Nome* della Persistence Unit;
- *Classi* (*[[034 Entità JPA|Entità]]*) a cui si riferisce;
- *Tipo di database* (per scegliere il giusto driver [[026 JDBC|JDBC]], ovvero Derby);
- *La posizione* ([[002 URL|URL]]);
- Modalità per l'*autenticazione*.

Senza una Persistence Unit le entità possono essere manipolate esclusivamente come [[023 POJO|POJO]] *senza funzionalità di persistenza*.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<persistence xmlns="http://xmlns.jcp.org/xml/ns/persistence" 
			 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
			 xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/persistence 
			 http://xmlns.jcp.org/xml/ns/persistence/persistence_2_1.xsd" 
			 version="2.1">
   <persistence-unit name="chapter04PU" transaction-type="RESOURCE_LOCAL">
      <provider>org.eclipse.persistence.jpa.PersistenceProvider</provider>
      <class>org.agoncal.book.javaee7.chapter04.Book</class>
      <properties>
         <property name="javax.persistence.schema-generation-action" value="drop-and-create" />
         <property name="javax.persistence.schema-generation-target" value="database" />
         <property name="javax.persistence.jdbc.driver" value="org.apache.derby.jdbc.ClientDriver" />
         <property name="javax.persistence.jdbc.url" value="jdbc:derby://localhost:1527/chapter04DB;create=true" />
         <property name="javax.persistence.jdbc.user" value="USER_NAME" />
         <property name="javax.persistence.jdbc.password" value="PASSWORD" />
      </properties>
   </persistence-unit>
</persistence>
```

La Persistence Unit `chapter04PU` definisce una connessione JDBC per il database Derby `chapter04DB` che è in esecuzione sul `localhost` alla porta 1527. Si connette con un user (`USER_NAME`) ed una password (`PASSWORD`) ad un dato URL.

Il tag `<class>` indica al PU di gestire la classe `Book`.

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