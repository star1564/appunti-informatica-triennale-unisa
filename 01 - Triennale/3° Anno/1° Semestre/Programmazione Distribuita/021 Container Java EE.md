---
aliases: 
tags:
  - corsi/informatica/programmazione_distribuita
paragrafo: Java Enterprise Edition
cssclasses:
  - 
---
>Un **Enterprise Container** offre l'*implementazione* ai servizi specificati da [[020 Java Enterprise Edition|JEE]]. Ogni volta che si scrive una applicazione in JEE, la si fa eseguire su questo container.

Poiché lo sviluppatore non deve sviluppare questi servizi, può concentrarsi sulla [[Logica di business]].

Ogni container ha uno *specifico ruolo*, supporta un *set di API* ed offre *servizi* alle [[020 Java Enterprise Edition#Componenti JEE|componenti]], nascondendo tutti i dettagli tecnici complessi.


![[Pasted image 20231021114651.png|400]]

<center><i>Da notare come i vari container comunicano tra di loro</i></center>

Esistono 4 tipi differenti di container:
- *Applet Container*: gestisce l'esecuzione di [applets](https://www.oracle.com/java/technologies/applets.html);
- *Application Client Container*: gestisce l'esecuzione dei componenti client;
- *[[011 Web Dinamico#CONTAINER / APPLICATION SERVER|Web Container]]*: gestisce l'esecuzione delle pagine web e servlet per applicazioni Java EE;
- *[[041 Enterprise Java Beans|EJB]] Container*: gestisce l'esecuzione degli enterprise beans per applicazioni Java EE.

Un servizio importante offerto dai container è **[[033 Java Persistence API|Java Persistence API (JPA)]]**, uno standard API che effettua automaticamente le query sul database.

### Packaging
Un'applicazione Java EE viene impacchettata (*packaging*) in una o più unità standard per essere distribuita su qualsiasi sistema compatibile con la piattaforma Java EE. 

Ogni unità contiene:
- Uno o più componenti funzionali, come un enterprise bean, una pagina web, una servlet o un'applet;
- Un deployment descriptor che ne specifica il contenuto.

Una volta prodotta un'unità Java EE, l'applicazione è pronta per essere distribuita. Il deployment prevede in genere l'utilizzo dello strumento di deployment di una piattaforma per specificare le informazioni specifiche del luogo, come l'elenco degli utenti locali che possono accedere e il nome del database locale. Una volta distribuita su una piattaforma locale, l'applicazione è pronta per essere eseguita.


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