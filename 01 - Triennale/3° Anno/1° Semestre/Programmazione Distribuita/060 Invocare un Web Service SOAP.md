---
aliases: 
tags:
  - corsi/informatica/programmazione_distribuita
paragrafo: Web Services
cssclasses:
  - 
---
Invocare un [[055 SOAP|Web Service SOAP]] è molti simile ad invocare un oggetto con [[017 Java RMI|RMI]].

>All'interno del client (*Client Container*) si trova un **Consumer Proxy**. Tali proxy *forniscono una rappresentazione Java di un endpoint di servizio Web* (servlet o EJB) ed *instradano la chiamata Java locale* al servizio Web remoto tramite [[003 HTTP|HTTP]]. 

Quando viene invocato un metodo su questo proxy, esso converte i parametri del metodo in un messaggio SOAP (la richiesta) e lo invia all'endpoint del servizio Web. Per ottenere il risultato, la risposta SOAP viene riconvertita in un'istanza del tipo restituito. 

![[Pasted image 20231124124457.png|700]]

### Invocazione Programmatica
Se il consumer *viene eseguito al di fuori di un container*, è necessario invocare programmaticamente il servizio web SOAP. 

> [!example]- <font color="orange">Esempio</font>
>```Java
>public class WebServiceConsumer {
>	public static void main(String[] args) {
>		CreditCard creditCard = new CreditCard();
>		creditCard.setNumber("12341234");
>		creditCard.setExpiryDate("10/12");
>		creditCard.setType("VISA");
>		creditCard.setControlNumber(1234);
>		
>		CardValidator cardValidator = new CardValidatorService().getCardValidatorPort();
>		cardValidator.validate(creditCard);
>	}
>}
>```
>
>Il consumer utilizza un'istanza di `CardValidatorService` (generata dal WSDL grazie a `wsimport`), utilizzando la parola chiave `new`. Deve quindi ottenere la classe proxy `CardValidator` (`getCardValidatorPort()`) per invocare localmente i metodi di business. Viene effettuata una chiamata locale al metodo `validate()` del proxy, che a sua volta richiamerà il servizio web remoto, creerà la richiesta SOAP, eseguirà il marshalling dei messaggi della carta di credito e così via. Il proxy trova il servizio di destinazione perché l'URL dell'endpoint predefinito è incorporato nel file WSDL ed è quindi integrato nell'implementazione del proxy.

### Invocazione con Injection
Se il consumer *viene eseguito in un container*, si può usare l'iniezione (attraverso l'annotazione ***`@WebServiceRef`***) per ottenere un riferimento al proxy del client del servizio web SOAP. 

> [!example]- <font color="orange">Esempio</font>
>```Java
>public class WebServiceConsumer {
>	@WebServiceRef
>	private static CardValidatorService cardValidatorService;
>	
>	public static void main(String[] args) {
>		CreditCard creditCard = new CreditCard();
>		creditCard.setNumber("12341234");
>		creditCard.setExpiryDate("10/12");
>		creditCard.setType("VISA");
>		creditCard.setControlNumber(1234);
>		
>		CardValidator cardValidator = cardValidatorService.getCardValidatorPort();
>		cardValidator.validate(creditCard);
>	}
>}
>```
>
>Una semplice classe Java principale gira in un contenitore di applicazioni client (ACC) e che utilizza l'annotazione `@WebServiceRef`. Segue lo schema di annotazione `@Resource` o `@EJB`, ma per i servizi web. Quando questa annotazione viene applicata a un attributo (o a un metodo getter), il contenitore inietterà un'istanza del proxy del client del servizio web quando l'applicazione viene inizializzata

### Invocazione con CDI
Si può usare [[027 Context e Dependency Injection|CDI]] per iniettare un riferimento ad un web service proxy attraverso [[@Inject]] e [[@Produces]].

> [!example]- <font color="orange">Esempio</font>
>```Java
>public class WebServiceProducer {
>	@Produces
>	@WebServiceRef
>	private CardValidatorService cardValidatorService;
>}
>```
>
>```Java
>@Stateless
>public class EJBConsumerWithCDI {
>	@Inject
>	private CardValidatorService cardValidatorService;
>	
>	public boolean validate(CreditCard creditCard) {
>		CardValidator cardValidator = cardValidatorService.getCardValidatorPort();
>		return cardValidator.validate(creditCard);
>	}
>}
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