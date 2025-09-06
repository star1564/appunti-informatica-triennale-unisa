---
aliases: SMTP
tags:
  - corsi/informatica/reti_calcolatori
paragrafo: Livello Applicazione
cssclasses:
  - 
---
>**SMTP** (Simple Mail Transfer Protocol) si tratta di un protocollo di comunicazione utilizzato per *inviare e recapitare messaggi di posta elettronica* attraverso Internet. 

Nell'SMTP ci sono due componenti principali, oltre il protocollo stesso:
- **Agente Utente** (mail reader), un programma per leggere e gestire la posta;
- **Mail Server**, contiene una *casella di posta per ogni utente* con i messaggi in arrivo ed una *coda di messaggi da trasmettere*.

Per inviare un messaggio (che corrisponde al trasporto di un file) in modo affidabile, *SMTP utilizza [[030 TCP|TCP]]*, il quale [[034 Apertura e Chiusura connessione TCP#Apertura della connessione|effettua l'handshaking per aprire la connessione]], trasferisce il messaggio e [[034 Apertura e Chiusura connessione TCP#Chiusura della connessione|chiude la connessione]].

> [!info] Indirizzi email
> Gli indirizzi sono risolti da un [[036 DNS|DNS]] che individua il server a cui inviare il messaggio.

![[Pasted image 20230920194542.png|550]]

> [!example] <font color="orange">Esempio Procedimento SMTP</font>
>1. Bob usa il suo agente utente per comporre il messaggio da inviare ad Alice;
>2. L'agente utente di Bob invia un messaggio al server di posta di Bob, questo viene posto nella coda dei messaggi del server;
>3. Il server di posta di Bob apre una connessione TCP con il server di posta di Alice ed invia il messaggio;
>4. Il server di posta di Alice pone il messaggio nella casella di posta di Alice;
>5. Alice invoca il suo agente utente per leggere il messaggio.

## PROTOCOLLI DI ACCESSO ALLA POSTA
I protocolli di accesso alla posta sono essenziali per consentire agli agenti utenti di *recuperare le email da un server di posta*. Ci sono due principali protocolli di questo tipo, ognuno di questi gestiscono e conservano le mail in modo diverso.

### POP3
**POP3** (*Post Office* Protocol 3) è progettato principalmente per *scaricare le email (RIMUOVENDOLE) dal server* di posta elettronica e *conservarle localmente sul dispositivo* del client. Da qui l'analogia di un ufficio postale.

### IMAP
**IMAP** (Internet Message Access Protocol) è progettato per *sincronizzare le email tra il server e i vari dispositivi* di accesso. Questo significa che le email *rimangono sul server* e sono accessibili da qualsiasi dispositivo connesso.

Le azioni effettuate (invio, cancellazione, archiviazione, ...) sul client vengono riflesse sul server.

## MIME NELL'SMTP
In SMTP, [[003 HTTP#MIME|MIME]] è utilizzato per consentire l'invio di messaggi di posta elettronica *contenenti contenuti multimediali o dati binari*.

Prima dell'introduzione di MIME, i messaggi di posta elettronica potevano contenere solo testo [[012 Caratteri non numerici|ASCII]]. Attraverso MIME, è possibile includere allegati come immagini, file audio, documenti Word, PDF e altro ancora nei messaggi di posta elettronica.

Come in HTTP, MIME definisce un *set di intestazioni specifiche* che vengono aggiunte ai messaggi di posta elettronica per indicare il tipo di dati contenuto nel corpo del messaggio e per gestire gli allegati. Ad esempio, un'intestazione `Content-Type` indica il tipo di contenuto, mentre un'intestazione `Content-Disposition` può specificare *come l'allegato deve essere visualizzato o trattato* dal programma di posta elettronica del destinatario.

___
[[000 Indice RC|↖ Ritorna all'indice ↖]]

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