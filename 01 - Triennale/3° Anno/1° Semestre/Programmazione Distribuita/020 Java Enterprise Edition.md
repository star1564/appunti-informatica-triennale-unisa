---
aliases:
  - JEE
  - Java EE
  - Componenti Java EE
tags:
  - corsi/informatica/programmazione_distribuita
paragrafo: Java Enterprise Edition
cssclasses:
  - 
---
Le imprese hanno il bisogno di applicazioni *[[001 Sistema Distribuito|distribuite]]*, *transazionali*, *scalabili* e *portabili* che sfruttino la *velocità*, la *sicurezza* e l'*affidabilità* della tecnologia lato server. Tutto questo evitando perdite di denaro.

>**Java Enterprise Edition (JEE)** è una estensione di Java Standard Edition (SE), usata per facilitare lo sviluppo di *applicazioni enterprise distribuite*, accorciando i tempi di sviluppo, riducendone la complessità e migliorandone le prestazioni. Per questo fornisce agli sviluppatori un potente *insieme di [[006 API a Chiamate di Sistema#^356da0|API]]*.

Java EE *è basato su delle specifiche standard* che sono sviluppate attraverso il Java Community Process (JCP), ovvero il processo di approvazione per le specifiche dalla comunità Java.

### Architettura Multitier
Il modello Java EE definisce un'*architettura multitier* (in questo caso [[020 Stili di Architettura#Three-Tier|Three-Tier]]), separando il lavoro in logica implementata dallo sviluppatore ([[Logica di business]] e [[Logica di presentazione]]) e servizi di sistema forniti dalla piattaforma, semplificando lo sviluppo di servizi multitier complessi.

Per gestire queste applicazioni in modo efficiente: 
- le funzioni inerenti al *client* vengono svolte nel livello superiore, attraverso browser o applicazioni java con [[Interfaccia GUI]] all'interno del client; 
- le funzioni *aziendali* vengono svolte nel livello intermedio, eseguito su hardware server dedicato;
- i database o servizi legati al [[009 Client-side e Server-side|back-end]] vengono eseguiti sul livello inferiore.

![[Pasted image 20231021112429.png|380]]

### Componenti JEE
Le applicazioni Java EE sono costituite da *unità software funzionali autonome*, note come **Componenti**. Questi componenti sono integrati nelle applicazioni Java EE insieme alle classi e ai file correlati e comunicano con altri componenti. Le specifiche di Java EE definiscono vari tipi di componenti:
- *Client applicativi*, che vengono eseguiti sul lato client;
- *Componenti Web*, tra cui Java [[013 Servlet|Servlets]], Java Server Faces e [[016 JSP|Java Server Pages]], che vengono eseguiti sul server e gestiscono attività relative al Web;
- *Componenti Business*, sono componenti che comprendono la logica di business che vengono eseguiti sul server (come gli [[041 Enterprise Java Beans|Enterprise Java Beans]]);
- *Componenti di Persistenza*, sono componenti che si occupano di interagire con il database (come le [[034 Entità JPA|Entità]]).

I componenti Java EE sono scritti nel linguaggio di programmazione Java e compilati come programmi Java standard. Tuttavia, si differenziano dalle classi Java standard per il fatto che vengono assemblati in un'applicazione Java EE, verificati per la conformità alle specifiche Java EE e distribuiti in produzione, dove vengono eseguiti e gestiti dal server Java EE.

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