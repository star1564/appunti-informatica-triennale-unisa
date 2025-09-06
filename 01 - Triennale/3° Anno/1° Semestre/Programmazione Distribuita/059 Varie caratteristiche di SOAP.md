---
aliases:
  - Eccezioni SOAP
  - Ciclo di Vita SOAP
  - Contesto SOAP
tags:
  - corsi/informatica/programmazione_distribuita
paragrafo: Web Services
cssclasses:
  - 
---
## Eccezioni
Con i [[055 SOAP|servizi Web SOAP]] il meccanismo delle [[023 Gestione delle Eccezioni|eccezioni di Java]] *non può funzionare*, poiché il consumer e il servizio possono non essere scritti nello stesso linguaggio e sono separati da una rete. 

L'idea è quindi quella di utilizzare il **fault SOAP** nel messaggio SOAP. Il runtime JAX-WS *converte automaticamente le eccezioni Java in messaggi di fault (errore) SOAP* che vengono restituiti al client. Questa funzione consente di risparmiare molto tempo ed energia, eliminando la necessità di scrivere il codice che mappa le eccezioni del servizio in errori SOAP.

Si specifica una fault attraverso l'annotazione `@WebFault(name = "exceptionName")` che va inserita sulla classe dell'eccezione. Oppure, se l'eccezione è specificata all'interno di Java verrà automaticamente scritta la fault.

```xml
<soap:Envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/">
	<soap:Body>
		<soap:Fault>
			<faultcode>soap:Server</faultcode>
			<faultstring>java.lang.NullPointerException</faultstring>
		</soap:Fault>
	</soap:Body>
</soap:Envelope>
```

> [!example]- <font color="orange">Esempio</font>
>Eccezione:
>```Java
>@WebFault(name = "CardValidationFault")
>public class CardValidatorException extends Exception {
>	public CardValidatorRTException() {
>		super();
>	}
>	
>	public CardValidatorRTException(String message) {
>		super(message);
>	}
>}
>```
>
>WebService:
>```Java
>@WebService
>public class CardValidator throws CardValidatorException {
>	public boolean validate(CreditCard creditCard) {
>		Character lastDigit = creditCard.getNumber().charAt( 
>		creditCard.getNumber().length() - 1);
>		
>		if (Integer.parseInt(lastDigit.toString()) % 2 == 0) {
>			return true;
>		} else {
>			throw new CardValidatorException("The credit card number is invalid");
>		}
>	}
>}
>```
>
>Se accade l'eccezione `CardValidatorException` il messaggio SOAP di risposta sarà:
>```xml
><soap:Envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/">
>	<soap:Body>
>		<soap:Fault>
>			<faultcode>soap:Server</faultcode>
>			<faultstring>org.agoncal.book.javaee7.chapter14.CardValidatorException</faultstring>
>			<detail>
>				<ns2:CardValidationFault xmlns:ns2="http://chapter14.javaee7.book.agoncal.org/">
>					<message>The credit card number is invalid</message>
>				</ns2:CardValidationFault>
>			</detail>
>		</soap:Fault>
>	</soap:Body>
></soap:Envelope>
>```

## Ciclo di Vita del SOAP
Il ciclo di vita è banale ed è inerente all'esecuzione di metodi annotati con `@PostConstruct` e `@PreDestroy`.

![[Pasted image 20231124123704.png|300]]

## WebServiceContext
SOAP dispone di un'ambiente basato sul [[027 Context e Dependency Injection#^d27126|contesto]] a cui si può accedere iniettandone il riferimento tramite `@Resource`, permettendo di ottenere informazioni come l'endpoint della classe di implementazione, il contesto del messaggio, le informazioni di sicurezza.

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