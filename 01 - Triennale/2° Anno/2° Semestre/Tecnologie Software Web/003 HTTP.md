---
aliases:
  - HTTP
  - Stateless
  - MIME
cssclasses:
  - 
tags:
  - corsi/informatica/tecnologie_software_web
paragrafo: HTTP
---

>L' **HTTP** (*HyperText Transfer Protocol*) è un [[Protocollo]] usato come principale sistema per la trasmissione d'informazioni (pagine o elementi di pagina) dal [[Server]] al [[Client]] attraverso il [[001 Introduzione a TSW|Web]].

Gestisce sia le richieste ([[002 URL|URL]]) inviate al server che le [[004 Richiesta e Risposta HTTP|risposte]] inviate al client (pagine) ed utilizza il [[030 TCP|TCP]] per far comunicare il client al server e viceversa.

## MIME
**MIME** (Multipurpose Internet Mail Extensions), è uno standard utilizzato per *consentire la trasmissione di dati* non solo in formato testuale, ma anche in formato *multimediale*, come immagini, audio, video e altri tipi di dati binari.

In HTTP, MIME è utilizzato per consentire ai server Web di inviare risorse diverse dal semplice testo [[006 Basi di HTML|HTML]] ai browser dei client.

Quando un server Web invia una risorsa a un client tramite HTTP, solitamente include un'*intestazione `Content-Type`* che specifica il tipo di dati contenuto nella risorsa. Ad esempio, il server può specificare che il contenuto è un documento HTML, un'immagine JPEG, un file audio MP3, ecc.

I MIME types sono rappresentati da una stringa del tipo `tipo/sottotipo` (esempio: `text/html` per HTML o `image/jpeg` per JPEG). Queste informazioni sono utilizzate dal browser per interpretare correttamente il contenuto ricevuto e renderlo in modo appropriato.

## VERSIONI HTTP
Esistono varie versioni del protocollo HTTP (la versione corrente è HTTP/3).

### HTTP/1.0
Il protocollo HTTP/1.0 ha le seguenti proprietà:
- **Stateless**: né il server né il client mantengono, a livello di protocollo, informazioni relative ai [[004 Richiesta e Risposta HTTP#^fd7da5|messaggi]] precedentemente scambiati. ^18998a
- *One-Shot*: il server può inviare solo una risorsa alla volta, chiudendo la connessione all'invio.

Questo tipo di connessioni vengono dette *non persistenti*.

> [!example]+ <font color="orange">Esempio</font>
> Ipotizziamo di volere richiedere una pagina composta da un file [[006 Basi di HTML|HTML]] e 10 immagini JPEG.
> ![[Pasted image 20230302153851.png]]

### HTTP/1.1
In questa versione, *la stessa connessione HTTP può essere utilizzata* per una serie di richieste e una serie corrispondente di risposte.
Questo tipo di connessioni vengono dette *persistenti*.

Praticamente, il server lascia aperta la connessione TCP dopo aver spedito la risposta e può quindi ricevere le richieste successive sulla stessa connessione.
La connessione viene chiusa dal server quando viene specificato dal messaggio oppure quando non è usata da un certo tempo (*timeout*).

#### PIPELINING
Per migliorare ulteriormente le prestazioni si può usare la tecnica del **[[067 Pipeline#^806fdc|pipelining]]**, che consiste nell'invio di *molteplici richieste da parte del client prima di terminare la ricezione delle risposte.*

![[Pasted image 20230302155529.png]]

### HTTP/2
In questa versione (altamente compatibile con quella precedente) viene *ridotta la latenza* per migliorare la velocità di caricamento delle pagine nei browser web.

Questo viene raggiunto con:
- La compressione dei dati appartenenti ai messaggi;
- Le tecnologie **push** dal lato server:
	- Tecnologia che permette al server di fornire le risorse di una pagina ancora prima che il client ne faccia richiesta.
- La pipeline delle richieste-risposte;
- Tempi di attesa molto minori nell'invio di una risorsa;
- Renderizzazione da parte del browser di ogni risorsa della pagina man mano che questa si rende disponibile.



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