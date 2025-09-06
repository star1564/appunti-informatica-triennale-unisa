---
aliases:
  - Web
cssclasses:
  - 
tags:
  - corsi/informatica/tecnologie_software_web
paragrafo: HTTP
---
>Il **World Wide Web** (WWW, *Web*) mette insieme le idee di [[Ipertesto]] e [[001 Introduzione a RC|Rete Internet]] in modo efficiente.

In pratica, il World Wide Web è un *ipertesto distribuito sulla rete*.

I documenti (chiamati **pagine**) costituiti da più risorse (immagini, video, testo, ecc.), risiedono su [[Server]] geograficamente distribuiti (World Wide) e costituiscono una ragnatela virtuale (*Web*).

> [!warning] Il Web non è Internet
> - Il Web è uno dei modi per diffondere informazioni con Internet.
> - Internet è utilizzato anche da [[037 SMTP|e-mail]], instant messaging e [[035 Livello di Applicazione#^3c1bc2|FTP]].
>
> Quindi l'internet è una *infrastruttura*, mentre il Web *funziona su quella infrastruttura*.
> 
> ![[Pasted image 20230301172059.png|400]]

>Il Web è composto da tre elementi:
>- [[002 URL|URL]];
>- [[003 HTTP|HTTP]];
>- [[006 Basi di HTML|HTML]].

### UTILIZZO DI UNA PAGINA WEB
Il Web utilizza il [[Modello Server-Client]].

1. Essenzialmente, per creare una pagina web, si creano file scritti in HTML e li si mettono sui server.
2. Il file HTML indica ai browser che si connettono al server cosa mostrare sullo schermo.
3. Una volta messi i file sul server, tutti i browser possono recepire la pagina web sull'internet.

Quando un client fa una richiesta contenente l'[[002 URL|URL]] della risorsa da ottenere, la risposta del server contiene: 
- la risorsa ricercata oppure un messaggio di errore; 
- un codice di stato (se la ricerca ha avuto successo o no);
- tipo di contenuto.

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