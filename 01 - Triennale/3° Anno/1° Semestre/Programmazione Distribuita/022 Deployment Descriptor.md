---
aliases: 
tags:
  - corsi/informatica/programmazione_distribuita
paragrafo: Java Enterprise Edition
cssclasses:
  - 
---
Un [[021 Container Java EE|enterprise container]] fa grande uso del *pre-processore* delle [[Annotazioni]] per specificare come raggiungere un obbiettivo (Declarative Programming).

>Le **Annotazioni in JEE** sono uno degli unici strumenti che uno sviluppatore ha per comunicare con l'application container, sono utilizzate per specificare come si deve comportare alla compilazione. Riducono la quantità di codice da scrivere.

> [!info] [[Alcune delle Annotazioni in CDI]]

## Deployment Descriptor
> È possibile inviare *maggiori informazioni* all'Enterprise container attraverso un **Deployment Descriptor** (un [[034 XML|XML]]) associato all'applicativo.

```XML
<?xmlversion="1.0"?>
<ejb-jar xmlns="http://xmlns.jcp.org/xml/ns/javaee"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee
	http://xmlns.jcp.org/xml/ns/javaee/ejb-jar_3_2.xsd"
	version="3.2">
	<enterprise-beans>
		<session>
			<ejb-name>ItemEJB</ejb-name>
			<remote>org.agoncal.book.javaee7.ItemRemote</remote>
			<local>org.agoncal.book.javaee7.ItemLocal</local>
			<local-bean/>
			<ejb-class>org.agoncal.book.javaee7.ItemEJB</ejb-class>
			<session-type>Stateless</session-type>
			<transaction-type>Container</transaction-type>
		</session>
	</enterprise-beans>
</ejb-jar>
```

La maggior parte dei deployment descriptor sono opzionali e si possono usare le annotazioni per ridurre il codice scritto, ma *per includere alcune cose sono obbligatori*.

Il **vantaggio** dei deployment descriptor è che possono essere *modificati senza richiedere modifiche al codice sorgente* e ricompilazioni.

Se esistono entrambi i meccanismi, l'*XML ha precedenza* sulle annotazioni.

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