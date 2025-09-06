---
aliases: 
tags:
  - corsi/informatica/programmazione_distribuita
paragrafo: Web Services
cssclasses:
  - 
---
>**SOAP** (*Simple Object Access Protocol*) è il protocollo standard di **message-encoding** per le applicazioni di [[053 Web Service|Servizi Web]]. Fornisce il *meccanismo di comunicazione per collegare i Servizi Web*, scambiando dati XML formattati attraverso un protocollo di rete, comunemente [[003 HTTP|HTTP]].

Mentre [[054 WSDL|WSDL]] descrive una interfaccia astratta del servizio web, SOAP *implementa lo scambio dei messaggi [[034 XML|XML]]* tra il consumer ed il provider. 
Infatti, invece di usare HTTP per inviare una [[006 Basi di HTML|pagina web]] ad una richiesta da un browser, *SOAP invia e riceve un messaggio XML* attraverso HTTP.

![[Pasted image 20231124094741.png|600]]

Un messaggio può essere di richiesta o di risposta ed è composto da un `Envelope`, `Header` e `Body`.

| Elemento  | Descrizione                                                                                                           |
|-----------|-----------------------------------------------------------------------------------------------------------------------|
| `Envelope`  | Definisce il messaggio e il namespace utilizzato nel documento. Questo è un elemento radice obbligatorio.            |
| `Header`    | Contiene eventuali attributi opzionali del messaggio o infrastrutture specifiche dell'applicazione, come informazioni sulla sicurezza o instradamento di rete. |
| `Body`      | Contiene il messaggio scambiato tra le applicazioni.                                                                 |
| `Fault`     | Fornisce informazioni sugli errori che si verificano durante l'elaborazione del messaggio. Questo elemento è opzionale.|


> [!tip]- Messaggio SOAP request
>```xml
><soapenv:Envelope 
>	xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" 
>	xmlns:web="http://www.example.com/SimpleService">
>    <soapenv:Header/>
>    <soapenv:Body>
>        <web:SimpleOperation>
>            <web:input>Il tuo parametro di input</web:input>
>        </web:SimpleOperation>
>    </soapenv:Body>
></soapenv:Envelope>
>```

> [!tip]- Messaggio SOAP response
>```xml
><soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:web="http://www.example.com/SimpleService">
>    <soapenv:Header/>
>    <soapenv:Body>
>        <web:SimpleOperationResponse>
>            <web:output>Il tuo risultato</web:output>
>        </web:SimpleOperationResponse>
>    </soapenv:Body>
></soapenv:Envelope>
>
>```



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