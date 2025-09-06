---
aliases: 
tags:
  - corsi/informatica/tecnologie_software_web
paragrafo: HTTP
cssclasses:
  - 
---
Un *Messaggio* è l'unità base di comunicazione [[003 HTTP|HTTP]], è definita come una specifica sequenza di byte concettualmente atomica. Può essere di richiesta o di risposta. ^fd7da5

## RICHIESTA
In un messaggio di richiesta viene passato il *nome del metodo HTTP*, che specifica il tipo di richiesta.
I metodi più usati sono:
- **GET**: quando il client richiede qualche risorsa dal server; ^86d27c
	- Per *ricevere* risorse attraverso le risposte.
- **POST**: quando il client richiede una risorsa attraverso un messaggio; ^9fc0de
	- Per far sapere al server delle informazioni da *memorizzare* (immettere dati su un sito o applicazione).

> [!info]- Mandare informazioni con GET e POST
> Le piccolissime informazioni che si possono inviare con GET le si possono inserire nell'URL attraverso le query.
> ![[Immagine 2023-03-02 162429.jpg|600]]
> 
> Invece nel POST, i dati sono inseriti nel corpo del messaggio.
> ![[Immagine 2023-03-02 162420.jpg|600]]

- *PUT*: chiede la memorizzazione sul server di una risorsa all'URL specificato (quindi si crea una nuova risorsa sul server);
- *DELETE*: richiede la cancellazione della risorsa riferita dall'URL specificato.

## RISPOSTA
Un messaggio può essere di **Risposta** da parte del [[Server]].
Una risposta ha:
- un **Header**, che comunica al browser: ^5613f3
	- il protocollo utilizzato;
	- l'esito della richiesta;
	- il tipo di contenuto del corpo.
- un **Corpo (Body)**, che contiene le informazioni (come un [[006 Basi di HTML|HTML]]) da far mostrare al browser. ^02e64f

![[Immagine 2023-03-02 163452.png.jpg|700]]

> [!info]+ Codici di stato
> Lo *status code* è un numero di tre cifre, di cui la prima indica la classe della risposta e le altre due la risposta specifica.
> - $\textcolor{#939fbf}{1xx}$, <font color="#939fbf">Informational</font>. 
> 	- Una risposta temporanea alla richiesta, durante il suo svolgimento (sconsigliata).
> - $\textcolor{#00C575}{2xx}$, <font color="#00C575">Successful</font>.
> 	- Il server ha ricevuto, capito e accettato la richiesta.
> - $\textcolor{orange}{3xx}$, <font color="orange">Redirection</font>.
> 	- Il server ha ricevuto e capito la richiesta, ma sono necessarie altre azioni da parte del client per portare a termine la richiesta.
> - $\textcolor{#FF6611}{4xx}$, <font color="#FF6611">Client Error</font>.
> 	- La richiesta del client non può essere soddisfatta per un errore da parte del client (errore sintattico o richiesta non autorizzata).
> - $\textcolor{#CC241D}{5xx}$, <font color="#CC241D">Server Error</font>.
> 	- Il server non è in grado di soddisfare la richiesta per un SUO problema.

^6cccca

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

<center><font color="rainbow"><i>404. Those who don't exist.</i></font></center>

Altri collegamenti: 

