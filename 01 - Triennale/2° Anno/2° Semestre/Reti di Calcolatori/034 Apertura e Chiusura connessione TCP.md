---
aliases:
  - Three-Way Handshake
  - Four-Way Handshake
  - TCP Handshake
tags:
  - corsi/informatica/reti_calcolatori
paragrafo: Livello Trasporto
cssclasses:
  - 
---
## Apertura della connessione
>Il **Three-Way Handshake** è un fondamentale processo di *inizializzazione delle connessioni* [[030 TCP|TCP]] utilizzato nei protocolli di comunicazione su Internet. 

Questo procedimento coinvolge l'invio di *tre messaggi coordinati* tra il mittente e il destinatario per stabilire una connessione affidabile e bidirezionale.

I tre messaggi sono: 
- **SYN** (SYNchronize); 
- **SYN-ACK** (SYNchronize-ACKnowledgement);
- **ACK** (ACKnowledgement).

Durante il Three-Way Handshake: 
1. Il mittente invia un pacchetto di richiesta di connessione (SYN) al destinatario; 
2. Il destinatario risponde con un pacchetto di conferma della richiesta (SYN-ACK);
3. Infine, il mittente conferma la ricezione del pacchetto di conferma (ACK). 

![[Pasted image 20230909122215.png|600]]

Questa procedura garantisce che entrambi i nodi siano pronti a scambiare dati in modo *sincronizzato*, creando una base stabile per la trasmissione di informazioni attraverso la rete.

> [!question]- Perché non si utilizza un sistema più semplice?
> La sua implementazione non è principalmente finalizzata alla sicurezza, ma piuttosto a garantire l'affidabilità e l'integrità delle comunicazioni. Ecco perché viene preferito rispetto a un approccio più semplice.
>
>Infatti, il Three Way Handshake previene il rischio di connessioni accidentali o non autorizzate. Se un dispositivo riceve un pacchetto SYN senza ricevere una risposta SYN-ACK, non si considera connesso. Ciò impedisce l'instaurazione di connessioni indesiderate.


## Chiusura della connessione
>Il **Four-Way Handshake** è un processo di *interruzione della connessione* dopo un Three-Way Handshake iniziale.

Questo processo assicura che entrambi i nodi abbiano avuto l'opportunità di *consegnare tutti i dati pendenti* prima di chiudere definitivamente la connessione, contribuendo a evitare perdita di dati o problemi durante la chiusura.

I messaggi utilizzati sono: 
- **FIN** (FINish); 
- **ACK** (ACKnowledgement).

Durante il Four-Way Handshake: 
1. Il mittente (client) invia un pacchetto di richiesta di chiusura della connessione (FIN) al destinatario (server); 
2. Il server risponde con un pacchetto di conferma (ACK) per indicare che ha ricevuto la richiesta di chiusura. Nel frattempo, *SOLO* il server *può continuare a inviare dati* se necessario;
3. Successivamente, quando il server è pronto a chiudere la sua parte della connessione, invia un pacchetto di richiesta di chiusura (FIN) al client;
4. Il client risponde con un pacchetto di conferma (ACK).

Dopo il completamento di questo processo, la connessione TCP è chiusa in modo sicuro e ordinato, e entrambi i nodi hanno terminato la comunicazione.

![[Pasted image 20230909123330.png|450]]

> [!example] <font color="orange">Esempio</font>
> L'invio di [[Cookie]] da parte di un server a un client alla chiusura della connessione.


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
- [Mozilla](https://developer.mozilla.org/en-US/docs/Glossary/TCP_handshake)