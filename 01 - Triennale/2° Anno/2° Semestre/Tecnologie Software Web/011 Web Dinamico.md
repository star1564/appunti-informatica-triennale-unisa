---
aliases:
  - Web Container
  - Servlet Container
  - Web Server
tags:
  - corsi/informatica/tecnologie_software_web
paragrafo: Web Server
cssclasses:
  - 
---
>Il **Web Dinamico** è un tipo di sito web che utilizza la *programmazione server-side* per generare contenuti personalizzati in tempo reale in base alle richieste degli utenti.

In un sito web dinamico, il contenuto *può essere generato a partire da dati memorizzati* in un [[001 Introduzione a Basi di Dati#^f0d93b|database]] o da altre fonti di informazioni esterne. 
Ciò significa che il contenuto può essere personalizzato per l'utente in modo dinamico, in base alle sue preferenze o al suo comportamento sul sito web, *separando* gli aspetti di **contenuto** da quelli di **presentazione**.

I siti web dinamici possono offrire altre funzionalità avanzate come la *ricerca interna*, la *registrazione degli utenti* e l'*e-commerce*. 

Inoltre, i siti web dinamici possono essere gestiti con maggiore facilità rispetto ai siti web statici, poiché il contenuto viene creato e gestito attraverso un'interfaccia amministrativa basata su web.

![[Pasted image 20230330084039.png|700]]

Tuttavia, i siti web dinamici richiedono un server web in grado di elaborare codice dinamico e possono richiedere competenze tecniche avanzate per la loro creazione e gestione. Inoltre, i siti web dinamici possono richiedere una maggiore attenzione alla *sicurezza*, poiché il codice dinamico può essere vulnerabile ad attacchi informatici.

---
### CONSERVARE IL CONTENUTO DI UN SITO WEB DINAMICO
Si potrebbe utilizzare un [[010 Modello Relazionale|database relazionale]] per memorizzare le informazioni relative ad ogni pagina da costruire. Pertanto si creano diversi *programmi* che generano dinamicamente il sito a partire dai dati nel database.

![[Pasted image 20230325114507.png|800]]

<center>Da notare che questo database NON è ottimale</center>

Quando si trova il contenuto da mostrare, si fa ritornare il campo `Testo`, ovvero il contenuto in formato [[006 Basi di HTML|HTML]], facendolo mostrare nella pagina completa.

In questo modo, è anche possibile *cambiare agevolmente il layout* di tutte le pagine utilizzando un unico **template di pagina**, dove i vari programmi vanno a sostituire le parti variabili.
![[Pasted image 20230325120135.png|600]]


---
### CONTAINER / APPLICATION SERVER

> [!failure] Ogni volta che viene invocato un programma si crea un processo che viene distrutto alla fine dell'elaborazione, creando problemi di prestazione.

La soluzione migliore al problema di prestazione è quella di creare un **Contenitore** in cui far "vivere" le funzioni server-side.

Un contenitore web è costituito da un'istanza del software di runtime e da *tutte le dipendenze necessarie per eseguire l'applicazione web*, come:
- librerie; 
- framework; 
- codice;
- file di configurazione. 

Il contenitore web fornisce un'[[018 Interfacce|interfaccia]] di programmazione unificata per l'applicazione, consentendo di eseguire, testare e distribuire l'applicazione web in modo consistente e riproducibile su diversi ambienti. Stabilisce anche il ciclo di vita di una funzione server-side.

Si ha così una soluzione *modulare* in cui le funzionalità ripetitive vengono portate a fattor comune.

Un ambiente di questo tipo prende il nome di **application server**.

![[Pasted image 20230325121109.png|600]]


Quando un server web riceve una richiesta [[003 HTTP|HTTP]] da un client, il server manda la richiesta al container (come [[012 Apache Tomcat|Apache Tomcat]]) nel quale è dispiegato un [[013 Servlet|Servlet]].

![[Pasted image 20230325154540.png|800]]

---
### TIPI DI INTERAZIONE CON L'APPLICATION SERVER
L'interazione tra un [[Client]] ed un [[Server]] su un application server può essere di due tipi:
- **Stateless**: è un tipo di applicazione che *non tiene traccia dello stato delle sessioni degli utenti*. 
	- Ogni richiesta inviata dal client al server viene trattata come una richiesta separata e indipendente, senza alcun riferimento al contesto delle richieste precedenti. 
	- Ogni richiesta contiene tutte le informazioni necessarie per essere elaborata dal server e produrre una risposta appropriata.
	- È attuabile solo se le risposte del server producono sempre lo stesso tipo di risultato.
	- Più facili da scalare e gestire.
- **Stateful**: è un tipo di applicazione che *mantiene lo stato delle sessioni degli utenti* tra le richieste. 
	- Tiene traccia di tutte le attività precedenti dell'utente e utilizza queste informazioni per gestire le richieste successive. 
	- Consente all'applicazione di fornire un'esperienza utente più personalizzata e interattiva.
	- Maggiore complessità di gestione.




___
[[000 Indice TSW|↖ Ritorna all'indice ↖]]

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