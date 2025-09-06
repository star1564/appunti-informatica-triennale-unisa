---
aliases: 
tags:
  - corsi/informatica/programmazione_distribuita
paragrafo: Enterprise Java Bean
cssclasses:
  - 
---
Il client di un [[041 Enterprise Java Beans|EJB]] può essere di diversi tipi, ma NON può usare l'operatore `new` per crearlo. Ha bisogno di un riferimento, che può essere iniettato attraverso l'annotazione `@EJB` (o `@Inject`) oppure attraverso un lookup [[025 Risorse e JNDI|JNDI]].

> [!tip]- GlassFish e gli EJB
> Il monitoraggio degli EJB può essere effettuato tramite la console di amministrazione di GlassFish. GlassFish consente poi di personalizzare diversi parametri del pool EJB. È possibile impostare la dimensione del pool (numero iniziale, minimo e massimo di bean nel pool), il numero di bean da rimuovere quando scade il timeout di inattività del pool e il numero di millisecondi per il timeout del pool. Queste opzioni sono specifiche dell'implementazione e potrebbero non essere disponibili su altri contenitori EJB.


> [!success] [[4 GlassFish Help - EJB|Impostare e creare EJB su Glassfish]]
### EJB no-interface
Se il bean è del tipo [[041 Enterprise Java Beans#^2aad01|no-interface]], allora il client deve solo ottenere un riferimento alla classe bean stessa:

```Java
@Stateless
public class ItemEJB {...}

// Client che prende la referenza all'EJB
@EJB ItemEJB item;
```

### EJB con Interfacce Locali e Remote
Se invece ci sono diverse interfacce, bisogna specificare quale si vuole usare:

```Java
@Stateless
public class ItemEJB implements ItemLocal, ItemRemote {...}

// Client che prende la referenza all'EJB o le sue interfacce
@EJB ItemEJB item; // Standard
@EJB ItemLocal itemLocal;
@EJB ItemRemote itemRemote;
```


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